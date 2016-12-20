+++
title = "Unity: release early, release often… release daily! (part 3)"
date = "2013-01-24T14:33:00+01:00"
tags = [ "devel", "libre", "PU", "quality", "ubuntu", "uds", "unity" ]
aliases = ["/post/Unity%3A-release-early%2C-release-often%E2%80%A6-release-daily%21-%28part-3%29"]
+++
    <p>This post is part of the <a href="/post/Unity%3A-release-early%2C-release-often%E2%80%A6-release-daily%21">Unity daily release process blog post suite</a>.</p>


<p>Now that we know how branches are flying to trunk and how we ensure that the packaging metadata are in sync with our delivery, let's swing to the heart of the daily release process!</p>


<h2>Preliminary notes on daily release</h2>

<p>This workflow is heavily using other components that we rely on. In addition to our own tool for daily release, which is <a href="https://launchpad.net/cupstream2distro">available here</a>, we needed to use jenkins for scheduling, controlling and monitoring the different parts of the process. This would not have been possible without the help of <a href="https://launchpad.net/~jibel">Jean-Baptiste</a> and the precious and countless hours of hangouts on setting up jenkins, debugging, looking at issues and investigating autopilot/utah failures. In addition to that, thanks to the #bzr channel as this process demanded us to investigate some bzr internal behavior, and even patching it (for lp-propose). Not forgetting pbuilder-classic and other uses that we needed to benchmark and ensure we are doing the right thing for creating the source packages. Finally, we needed to fix (and automate the fix) of a lot of configuration on the launchpad side itself for our projects. Now it's clean and we have tools to automatically keeps that this way! Thanks to everyone involved into those various efforts.</p>


<h2>General flow of daily release</h2>


<p><a href="/public/ubuntu/daily-release/daily-release-summary.png" title="Daily release, general flow"><img src="/public/ubuntu/daily-release/.daily-release-summary_m.jpg" alt="Daily release, general flow" style="display:block; margin:0 auto;" title="Daily release, general flow, janv. 2013" /></a>
The global idea of the different phases for achieving a stack delivery is the following:</p>
<ul>
<li>For every component of the stack (new word spotted, will be definied in the next paragraph), prepare a source package</li>
<li>Upload them to ppa and build the packages</li>
<li>Once built, run the integration tests associated to the stack</li>
<li>If everything is ok, publish those to Ubuntu</li>
</ul>

<p>Of course, this is the big picture, let's enter into more details now. :)</p>


<h2>What is a stack?</h2>

<p>Daily release is working per stack. Jenkins is showing <a href="https://jenkins.qa.ubuntu.com/view/cu2d/">all of them here</a>. We validate a stack all together or reject all components that are part of it. A stack are various components having close interactions and relationships between themselves. They are generally worked on by the same team. For instance, those are the different stacks we have right now:</p>
<ul>
<li>the <a href="https://jenkins.qa.ubuntu.com/view/cu2d/view/Indicators%20Head/">indicator stack</a> for indicators :)</li>
<li>the <a href="https://jenkins.qa.ubuntu.com/view/cu2d/view/OIF%20Head/">open input framework stack</a></li>
<li>the <a href="https://jenkins.qa.ubuntu.com/view/cu2d/view/Misc.%20Head/">misc stack</a> contains all testing tools, as well as misc components like theming, wallpapers, design assets. This is generally all components that don't have integration tests (and so we don't run any integration tests step)</li>
<li>the <a href="https://jenkins.qa.ubuntu.com/view/cu2d/view/WebApps%20Head/">web application stack</a> (in progress to have integration tests so that we can add more components)</li>
<li>the <a href="https://jenkins.qa.ubuntu.com/view/cu2d/view/WebCreds%20Head/">web credentials stack</a> (in progress to have integration tests so that we can add more components)</li>
<li>the <a href="https://jenkins.qa.ubuntu.com/view/cu2d/view/Unity%20Head/">Unity stack</a>, containing the core components of what defines the Unity experience.</li>
</ul>

<p>The "cu2d" group that you can see in the above links is everything that is related to the daily release.</p>


<p>As we are working per stack, let's focus here on one particular stack and seeing the life of its components.</p>



<h2>Different phases of one stack</h2>

<h3>General diagram</h3>


<p>Let's have a look at the general diagram in a different way to see what is running concurrently:</p>


<p><a href="/public/ubuntu/daily-release/daily-release-jenkins-jobs.png" title="Daily Release, jenkins jobs"><img src="/public/ubuntu/daily-release/.daily-release-jenkins-jobs_m.jpg" alt="Daily Release, jenkins jobs" style="display:block; margin:0 auto;" title="Daily Release, jenkins jobs, janv. 2013" /></a></p>


<p>You should recognize here the different jenkins jobs that we can list per stack, like <a href="https://jenkins.qa.ubuntu.com/view/cu2d/view/Unity%20Head/">this one</a> for instance.</p>


<h3>The prepare phase</h3>

<p>The prepare phase is running one sub-prepare job per components. This is all control by the <a href="http://bazaar.launchpad.net/~cupstream2distro-maintainers/cupstream2distro/trunk/view/head:/prepare-package">prepare-package script</a>. Each projects:</p>
<ul>
<li>Branch latest trunk for the component</li>
<li>Collect latest package version that were available to this branch and compare to distro ones, we can get multiple case here:
<ul>
<li>the version in the distro is the same than the one we have in the branch, we are then downloading the source code from Ubuntu of the latest available package to check its content against the content of the branch (this is a double security check).</li>
<li>If everything matches, we are confident that we won't overwrite anything in the distribution (by a past direct upload) that is not in the upstream trunk and can go on.</li>
<li>However, maybe a previous release was tempted as part of the daily release, so we need to take into account the version in the ppa we are using for daily release to eventually bump the versioning (it would be like the previous version in the ppa never existed)</li>
<li>if the version is less than the one in the distro, it means that there has been a direct upload to the distro not backported to the upstream trunk. The prepare job for this component is exiting as unstable, but we don't block the rest of the process. We'll simply ignore that component and try to validate all the other changes on the other projects without anything new from that one (if this component was really needed, the integration tests will fail later on). The <a href="https://launchpad.net/~ubuntu-unity">integration team</a> will then work to get those additional changes in the distro merged back into trunk for the next daily release.</li>
</ul></li>
<li>We then check that we have new useful commits to publish. It means that a change which isn't restricted to debian/changelog or po files update has been made. If no useful revision is detected, we won't release that component today, but still go on with the other projects.</li>
<li>We then create the new package versionning. See the paragraph about versionning below.</li>
<li>We then update the symbols file. Indeed, the symbols file needs to know in which exact version a new symbol was introduced. This can't be done as part of the merge process as we can't be sure that next day, the release will be successful. So upstream is using the magic string "0replaceme" instead of providing a version as part the merge adding a symbol to a library. As part of making that package a candidate for daily release, we then replace automatically all the occurences of "0replaceme" by the exact versionning we computed just before. We add an entry to the changelog if we had to do that to document.</li>
<li>Then, we prepare the changelog content.
<ul>
<li>We first scan the changelog itself to grab all bugs that were manually provided as part of upstream merges by the developers since that last package upload.</li>
<li>Then, we scan the whole bzr history until latest daily release (we know the revision of the previous daily release where the snapshot has been taken by extracting the string "Automatic snapshot from revision &lt;revision&gt;". Fetching in detail this history (included merged branch content), we are looking for any trace of bugs attached or commit message talking about "bug #…" (there is a quite flexible regexp for extracting that).</li>
<li>We then look at the committer for each bug fixes and grab the bug title from launchpad. We finally populate debian/changelog (if it wasn't already previously mentionned in our initial parsing of debian/changelog to avoid duplication). This mean that it's directly the person fixing the issue who gets the praise (and blames ;)) for this fix. You can then see <a href="https://lists.ubuntu.com/archives/raring-changes/2013-January/004507.html">a result like that</a>. As we only deduplicate since the last package upload, it means that we can mention again the same bug number if the previous fix in last upload wasn't enough. Also all the fixes are grouped and ordered by names, making the changelog quite clear (<a href="https://lists.ubuntu.com/archives/raring-changes/2013-January/004090.html">see here</a>).</li>
<li>We finally append the revision from where the snapshot was taken to the changelog to make clear what's in this upload and what's not.</li>
</ul></li>
<li>Then, we generate a diff if we have any meaningful packaging changes since last upload to Ubuntu (so ignoring changelog and symbols automatically replaced). In case we detect some, we include all meta-build information (like configure.ac, cmake files, automake files) that changed as well since last release to be able to have a quick look at why something changed. This information will be used during the publisher step.</li>
<li>We then sync all bugs that were previously detected, opening downstream bugs in launchpad so that they are getting closed by the package upload.</li>
<li>We then commit the branch and keep it there for now.</li>
<li>Building the source package, inside a chroot to try to get all build-dependencies resolved is then the next step. This is using pbuilder-classic and a <a href="http://bazaar.launchpad.net/~cupstream2distro-maintainers/cupstream2distro/trunk/files/head:/chroot-tools/">custom setup</a> to do exactly the job we want to do (we have our own .pbuilderrc and triggering tool, using cowbuilder to avoid having to extract a chroot). This step is creating as well the upstream tarball that we are going to use.</li>
<li>Finally, we upload that source package that we just got to the <a href="https://launchpad.net/~ubuntu-unity/+archive/daily-build">ubuntu-unity ppa</a>, and save various configuration parts.</li>
</ul>

<p>All those steps are happening in parallel for any component of the stack.</p>


<p>For those interested, here are <a href="https://jenkins.qa.ubuntu.com/view/cu2d/view/Unity%20Head/job/cu2d-unity-head-1.1prepare-unity/35/console">the logs</a> when there is some new commits to publish for that component. Here is <a href="https://jenkins.qa.ubuntu.com/view/cu2d/view/Unity%20Head/job/cu2d-unity-head-1.1prepare-dee/38/console">another example</a>, when there is nothing relevant. Finally, an upload not being on trunk is signaled <a href="https://jenkins.qa.ubuntu.com/view/cu2d/view/Indicators%20Head/job/cu2d-indicators-head-1.1prepare-appmenu-gtk/10/console">like that</a> (and the component is ignored, but other goes on).</p>


<h3>Monitoring the ppa build</h3>

<p>This is mostly the job of this <a href="http://bazaar.launchpad.net/~cupstream2distro-maintainers/cupstream2distro/trunk/view/head:/watch-ppa">watch-ppa script</a>. It monitors the <a href="https://launchpad.net/~ubuntu-unity/+archive/daily-build">ubuntu-unity daily-ppa</a> build status for the components we just uploaded  and ensure that they are published and built successfully on all architectures. As we are running unit tests as part of the packaging build, we already have unit tests passing with latest Ubuntu ensured that way. In addition, the script generates some meta-information when packages are published. It will make the jenkins job failing if any architecture failed to build, or if a package that was supposed to start building had never its source published (after a timeout) in the ppa.</p>

<ul>
<li>Logs of a <a href="https://jenkins.qa.ubuntu.com/view/cu2d/view/Unity%20Head/job/cu2d-unity-head-2.1build/34/console">successful build</a></li>
<li>Logs of a <a href="https://jenkins.qa.ubuntu.com/view/cu2d/view/Indicators%20Head/job/cu2d-indicators-head-2.1build/45/console">failed build</a></li>
</ul>

<h3>Running the integration tests</h3>

<p>In parallel to monitoring the build, we are running the integration tests (if any). For that, we start monitoring using the previous script, but only on i386. Once all new components for this arch are built and published, we start a slave jenkins jobs running the integration tests. Those will, using UTAH, provision a machine, installing latest Ubuntu iso for each configuration (intel, nvidia, ati), then we add the packages we need from this stack using the ppa and run the tests corresponding to the current stack. Getting all Unity autopilot tests stabilized for that, was a huge task. Thanks in particular to <a href="https://launchpad.net/~sil2100">Łukasz Zemczak</a> and with some help of the upstream Unity team, we finally got the number of autopilot tests failing under control (from 130 to 12/15 over the 450 tests). We are using them to validate Unity nowadays. However, the indicator and oif tasks had no integration tests at all. To not block the process and trying to move on, we then "stole" the relevant (hud + indicators one for indicators) Unity autopilot tests and only run those. As oif has no integration test at all, we just ensure that Unity is still starting and that the ABI is not broken.</p>


<p>We are waiting on web credentials and web apps stack to grow some integration tests (which are coming in the next couple of weeks with our help) to be able to add more components from them and ensuring the quality status we target for.</p>


<p>Then, the check job is getting back the results of those tests and thanks to a configuration per-stack mechanism, we have triggers to decide if the current state is acceptable or not to us. For instance, over the 450 tests running for Unity (x3 as we run on intel, nvidia and ati), we accept for instance 5% of failures. We have different level for regressions and skipped tests as well. If the collected values are below those triggers, we are considering that the tests are passing.</p>

<ul>
<li>Logs of <a href="https://jenkins.qa.ubuntu.com/view/cu2d/view/Unity%20Head/job/cu2d-unity-head-2.2check/40/console">tests passing</a>. You can get a more detailed view on which tests passed and failed looking at the son job, which is the one running really the tests: https://jenkins.qa.ubuntu.com/job/ps-unity-autopilot-release-testing/. (unstable, yellow means that some tests failed). The parent one is only launching it when ready and doing the collect.</li>
<li>Logs of <a href="https://jenkins.qa.ubuntu.com/view/cu2d/view/Indicators%20Head/job/cu2d-indicators-head-2.2check/33/console">more tests</a> than the acceptable threshold failing.</li>
<li>Logs of tests failing because <a href="https://jenkins.qa.ubuntu.com/view/cu2d/view/Unity%20Head/job/cu2d-unity-head-2.2check/34/console">UTAH failed to provision the machine or the daily build iso wasn't installable</a>.</li>
</ul>

<h3>And finally, publishing</h3>

<p>If we reach this step, it means that:</p>
<ul>
<li>we have at least one component with something meaningful to publish</li>
<li>the build was successful on all architectures.</li>
<li>the tests are in the acceptable tests passing rate</li>
</ul>

<p>So, let's go on and publish! The <a href="http://bazaar.launchpad.net/~cupstream2distro-maintainers/cupstream2distro/trunk/view/head:/publisher">script</a> has still some steps to proceed. In addition to have a "to other ppa" or "to distro" mode as a publishing destination, it:</p>
<ul>
<li>will check if we have manual packaging changes (remember this diff we did for each component in the prepare stage?). If we actually do have some, we put the publishing in a manual mode. This means that we won't publish this stack automatically and only people with upload rights have special credentials to force the publish.</li>
<li>it will then propose and autoapprove upstream for each components the changes that we made to the package (edition of debian/changelog, symbols files…). Then, the upstream merger will grab those branches and merge that upstream. We are only pushing those changes back upstream now (and not during the prepare steps) as we are only finally sure we are releasing them.</li>
</ul>

<p>Most of the time, we don't have packaging changes, so the story is <a href="https://jenkins.qa.ubuntu.com/view/cu2d/view/Unity%20Head/job/cu2d-unity-head-3.0publish/29/console">quite easy</a>.
When we do, the job is <a href="https://jenkins.qa.ubuntu.com/view/cu2d/view/OIF%20Head/job/cu2d-oif-head-3.0publish/9/console">showing it in the log</a> and is marked as unstabled. The packaging diff, plus the additional build system contexts are <a href="https://jenkins.qa.ubuntu.com/view/cu2d/view/OIF%20Head/job/cu2d-oif-head-3.0publish/9/">attached as artefacts</a> for an easy review. If the integration team agreed with those changes, they have special credentials to run <a href="http://bazaar.launchpad.net/~cupstream2distro-maintainers/cupstream2distro/trunk/view/head:/jenkins/cu2d-run">cu2d-run</a> in a special "manual" mode, ignoring the packaging changes and doing the publish if nothing else is blocking. This way, we address the concerns of "only people with upload right can ack/review packaging changes". This is our secondary safety net (the first one being the integration team looking at upstream merges).</p>


<p>The reality is not that easy and that's not the only case when the publish can be set as manual, but I'll discuss that later on.</p>


<p>Small note on the publisher, you see that we are merging back our changes to upstream, but upstream didn't sleep while we take our snapshot, build everything and have those integration tests running, so we usually end up where some commits were done in trunk in between the latest snapshot and the time we push back that branch. That's the reason why we have this "Automatic snapshot from revision &lt;revision&gt;" in the changelog to clearly state where we did the cut and don't rely on the commit of this changelog modification to be merged back.</p>



<h3>Copy to distro</h3>

<p>In fact, publishing is not the end of the story. <a href="https://launchpad.net/~cjwatson">Colin Watson</a> had the concern that we need to separate the power and not having too many launchpad bots having upload rights to Ubuntu. That's true that all previous operations are using a bot for committing, pushing to the ppa, with its own gpg and ssh key. Having those credentials widespread on multiple jenkins machine can be scary if they give upload privileges to Ubuntu. So instead of the previous step directly being piloted by a bot having upload rights to the distro, we only generated a sync file with various infos, <a href="https://jenkins.qa.ubuntu.com/view/cu2d/view/Unity%20Head/job/cu2d-unity-head-3.0publish/26/artifact/packagelist_rsync_cu2d-unity-head">like this one</a>.</p>


<p>Then, on the archive admin machines, we have a cron using this <a href="http://bazaar.launchpad.net/~cupstream2distro-maintainers/cupstream2distro/trunk/view/head:/copy2distro">copy script</a> which:</p>
<ul>
<li>collects all available rsync files over the wire</li>
<li>then, check that the info in this file are valid, like ensuring that components to be published are part of this whitelist of projects that can be synced (which is the same list and metainfo used to generate the jenkins jobs and lives <a href="http://bazaar.launchpad.net/~cupstream2distro-maintainers/cupstream2distro/trunk/files/head:/jenkins/etc/">there</a>).</li>
<li>do a last final check from version compared to the distro if new version were uploaded since the prepare phase (indeed, maybe an upload happened on one of those components while we were busy building/testing/publishing).</li>
<li>if everything is fine, the sources and binaries are copied from the ppa to the proposed repository.</li>
</ul>

<p><img src="/public/ubuntu/daily-release/.rsync-to-distro_m.jpg" alt="Copy to distro" style="display:block; margin:0 auto;" title="Copy to distro, janv. 2013" /></p>


<p>The fact to execute binary copies from the ppa (meaning that the .debs in the ppa are exactly the one that are copied to Ubuntu) gives us the confidence that what we tested is exactly what is delivered, built in the same order, to our users. The second obvious advantage as well is that we don't rebuild what's already ready.</p>



<h2>Rebuild only one or some components</h2>

<p>We have an additional facility. Let's say that test integration failed because of one component (like one build dependency not being bumped), we have the possibility to only rerun (still using the same <a href="http://bazaar.launchpad.net/~cupstream2distro-maintainers/cupstream2distro/trunk/view/head:/jenkins/cu2d-run">cu2d-run</a> script by people having the credentials) the whole workflow for this or those defined on the command line components. We keep for the others components exactly the same state, meaning, not rebuilding them, but they would still be part of what we tests as part of the integration suite and as part of what we are going to publish. Some resources are spared this way and we can get faster to our result.</p>


<p>However, for the build results, it will still take all the components (even those we don't rebuild) into account. We won't let a FTBFS slipping by that easily! :)</p>


<h2>Why not including all commits in debian/changelog?</h2>

<p>We made the choice to only mention changes associated bugs. This also mean that if upstream never link bugs to merge proposal, or use "bzr commit --fixes lp:XXXX" or put in a commit message something like "this fixes bug…", the upload will have an empty changelog (just having "latest snapshot from rev …") and won't detail the upload content.</p>


<p>This can be seen as suboptimal, but I think that we should really engage our upstream to link fixes to bugs when they are important to be broadcasted. The noise of every commit message to trunk isn't suitable for people reading debian/changelog, which is visible in update-manager. If people wants to track closely upstream, they can look at the bzr branch for that. You can see debian/changelog as a NEWS file, containing only important information from that upload. We need of course to have more and more people upstream realizing that and being educated that linking to a bug report is important.</p>


<p>If an information is important to them but they don't want to open a bug for it if there isn't one, there is still the possibility, as part of the upstream merge, to directly feed debian/changelog with a sensible description of the change.</p>


<h2>Versionning scheme</h2>

<p>We needed to come with a sane versionning schema. The current one is:</p>
<ol>
<li>based on current upstream version, like 6.5</li>
<li>appending "daily" to it</li>
<li>adding the current date in yy.mm.dd format</li>
</ol>
<p>So we end up with, for instance: 6.5daily13.01.24. Then, we add -0ubuntu1 as the packaging is separated in the diff.gz by using split mode.</p>


<p>If we need to rerun the same day a release for the same component, as we detect that a version was already released in Ubuntu or eventually in the ppa, this will become: 6.5daily13.01.24.1, and then 6.5daily13.01.24.2… I think you got it :)</p>


<p>We can use this general pattern: &lt;upstream_version&gt;daily&lt;yy.mm.dd(.minor)&gt;-0ubuntu1 for the package. The next goal is to have upstream_version as small (one, two digits?) as possible.</p>


<h2>Configuring all those jobs</h2>

<p>As we have one job per project per series + some metajob per stack, we are about having 80 jobs right now per supported version. This is a lot, the only way to not have bazillions of people just to maintain those jenkins jobs is to automate, automate, automate.</p>


<p>So every stack <a href="http://bazaar.launchpad.net/~cupstream2distro-maintainers/cupstream2distro/trunk/view/head:/jenkins/etc/unity-head.cfg">defines some metadata</a> like a serie, projects that they cover, when they need to run, eventual extracheck step, ppa used as source and destination, optional branches involved… Then, using <a href="http://bazaar.launchpad.net/~cupstream2distro-maintainers/cupstream2distro/trunk/view/head:/jenkins/cu2d-update-stack">cu2d-update-stack -U</a>, this will update all the jenkins jobs to latest configuration using <a href="http://bazaar.launchpad.net/~cupstream2distro-maintainers/cupstream2distro/trunk/files/head:/jenkins/templates/">the templates we defined to standardize those jobs</a>. In addition, that will reset as well some bzr configuration in launchpad for those branches to ensure that various components like lp-propose or branching the desired target will indeed do the right thing. As told previously, as this list is as well used for filtering when copying to distro, we have very few chances to get out of sync by automating everything!</p>


<p>So, just adding a component to a stack is basically a one line change + the script to run! Hard to do something easier. :)</p>



<p>And btw, if you were quite sharp on the last set of links, you have seen a "dependencies" stanza in the stack definition. Indeed, we described here the simple case and eluded completely the fact that stacks are depending on each other. How do we resolve that? How do we avoid publishing an inconsistent state? That's a story for tomorrow! :)</p>