+++
title = "Unity: release early, release often… release daily! (part 2)"
date = "2013-01-23T12:40:00+01:00"
tags = [ "devel", "libre", "PU", "quality", "ubuntu", "uds", "unity" ]
aliases = ["/post/Unity%3A-release-early%2C-release-often%E2%80%A6-release-daily%21-%28part-2%29"]
+++
    <p>This post is part of the <a href="/post/Unity%3A-release-early%2C-release-often%E2%80%A6-release-daily%21">Unity daily release process blog post suite</a>.</p>


<p>As part of the new Unity release procedure, let's first have look at the start of the story of a branch, how does it reach trunk?</p>


<h2>The merge procedure</h2>


<p>Starting the 12.04 development cycle, we needed upstream to be able to reliably and easily get their changes into trunk. To ensure that every commits in trunk pass some basic unit tests and doesn't break the build, that would obviously mean some automation would take place. Here comes the merger bot.</p>


<p><a href="/public/ubuntu/daily-release/merge-upstream-branch.png" title="Merge upstream branch workflow"><img src="/public/ubuntu/daily-release/.merge-upstream-branch_m.jpg" alt="Merge upstream branch workflow" style="display:block; margin:0 auto;" title="Merge upstream branch workflow, janv. 2013" /></a></p>


<h3>Proposing my branch for being merged, general workflow</h3>

<p>We require peer-review of every merge request on any project where we are upstream. No direct commit to trunk. It means that any code change will be validated by an human first. In addition to this, once the branch is approved, the current branch will be:</p>
<ul>
<li>built on most architectures (i386, amd64, armhf) in a clean environment (chroot with only the minimal dependencies)</li>
<li>unit tests will be run (as part of the packaging setup) on those archs</li>
</ul>
<p>Only if all this passes, the branch will be merged into trunk. This way, we know that trunk is at a high standard already.</p>


<p>You will notice in this <a href="https://code.launchpad.net/~mandel/unity/action-link-selected/+merge/144335">example of a merge request</a> that in advance of phase (thanks to the work of <a href="https://launchpad.net/~mrazik">Martin Mrazik</a>, <a href="https://launchpad.net/~fginther">Francis Ginther</a>), a continuous integration job is kicking in to give some early feedback to both the developer and the reviewer. This can indicate if the branch is good for merging even before approving it. This job kicks back if additional commit is proposed as well. This rapid feedback loop helps to give an additional advice on the branch quality and direct link to a <a href="https://jenkins.qa.ubuntu.com/">public jenkins instance</a> to see the eventual issues during the build.</p>


<p>Once the global status of a merge request is set to "approved", the merger will validate the branch, then takes the commit message (and falling back to the description on some project as a commit message if nothing is set), will eventually take attached bug reports (that the developer attached manually to the merge proposal or directly in a commit with "bzr commit --fixes lp:&lt;bugnumber&gt;") and merge that to the mainline, as you can see <a href="http://bazaar.launchpad.net/~unity-team/unity/trunk/revision/3057">here</a>.</p>


<h3>How to handle dependencies, new files shipped and similar items</h3>

<p>We told in the previous section that the builds are done in a chroot, clean environnement. But we can have dependencies that are not released into the distribution yet. So how to handle those dependencies, detect them and taking the latest stack available?</p>


<p>For that, we are using our debian packages. As this is what the finale "product" will be delivered to our users, using packages here and similar tools that we are using for the Ubuntu distribution itself is a great help. This means that we have a local repository with the latest "trunk build" packages (appending "bzr&lt;revision&gt;" to the current Ubuntu package version) so that when it's building Unity, it will grab the latest (eventually locally built) Nux and Compiz.</p>


<p>Ok, we are using packages, but how to ensure that when I have a new requirement/dependency, or when I'm shipping a new file, the packaging will be in sync with this merge request? Previously and historically, the packaging branch was separated from upstream. This was mainly for 3 reasons:</p>
<ul>
<li>we don't really want to require that our upstream learns how to package and all the small details around this</li>
<li>we don't want to be seen as distro-specific for our stack and be as any other upstream</li>
<li>we the integration team wants to have the control over their packaging</li>
</ul>

<p>This mostly worked in the sense that people had to ping the integration team just before setting a merge to "approve", and ensure no other merges was in process meanwhile (to not take the wrong packaging metadata with other branches). However, it's quite clear this can't scale at all. We did have some rejections because ensuring that we can be in sync was difficult.</p>


<p>So, we decided this cycle to have the packaging inlined with the upstream branch. This doesn't change anything for other distributions as "make dist" is used to create a tarball (or they can grab any tarball from launchpad from the daily release) and those don't contains the packaging infos. So we are not hurting them here. However, this ensures that we are in sync between what we will deliver the next day to Ubuntu and what upstream is setting into their code. This work started at the very beginning of the cycle and thanks to the excellent work of <a href="http://mterry.name/">Michael Terry</a>, <a href="https://launchpad.net/~mathieu-tl">Mathieu Trudel-Lapierre</a>, <a href="http://blogs.gnome.org/kenvandine/">Ken VanDine</a> and <a href="https://launchpad.net/~robru">Robert Bruce Park</a>, we got that quickly in. Though, there were some exceptions where achieving this was really difficult, because of unit tests were not really in shape to work in isolation (meaning in a chroot, with mock objects like Xorg, dbus…). We are still working on getting the latest elements bootstrapped to this process and having those tests smoothly running. I clearly know this idea of using the packaging to build the upstream trunk and having this inlined is a drastic change, but from what we can see since October, we have pretty good results with this and it seems to have worked out quite well! I would like to thanks again the whole awesome product strategy (the canonical upstream) team to have let that idea going through and facilitate as much as possible this process. Thanks as well to the jenkins master (2 of them already announced previously, plus <a href="https://launchpad.net/~allanlesage">Allan LeSage</a> and <a href="https://launchpad.net/~victor-ruiz+qa">Victor R. Ruiz</a>) to have completed all the jenkins/merger machinery changes that were needed on each project for that.</p>


<p>We can't expect that every upstream will know everything about the packaging, consequently the integration team is here and available for giving any help which is needed. I think on the long term that basic packaging changes will be directly done by upstream (we are already seeing some people bumping the build-dependency requirement themselves, adding a new file to install, declaring a new symbol as part of the library…). However, we have processes inside the distribution and only people with upload rights in Ubuntu is supposed to do or review the changes. How does this work with this process? Also we have some feature freeze and other Ubuntu processes, how will we ensure that upstream are not breaking those rules?</p>



<h3>Merge guidelines</h3>

<p>As you can see in the first diagram, some requirements during a merge request, both controlled by the acceptance criterias and the new conditions from inline packaging, are set:</p>
<ul>
<li>Design needs to acknowledge the change if there is any visual change involved</li>
<li>Both the developer, reviewer and the integration team ensure that ubuntu processes are followed (<a href="https://wiki.ubuntu.com/UserInterfaceFreeze">UI Freeze</a>/<a href="https://wiki.ubuntu.com/FeatureFreeze">Feature Freeze</a> for instance). If exceptions are required, they check before approving for merging that they are acknowledged by different parts. The integration team can help smooth to have this happened, but the requests should emerge from developers.</li>
<li>Relevant bugs are attached to the merge proposal. This is useful for tracking what changed and for generating the changelog as we'll see in the next part</li>
<li>Review of new/modified tests (for existence)</li>
<li>They ensure that it builds and unit tests are passing <a href="/post/automated" title="automated">automated</a></li>
<li>Another important one, especially when you refactor, is to ensure that integration tests are still passing</li>
<li>Review of the change itself by another peer contributor</li>
<li>If the change seems important enough to have a bug report linked, ensure that the merge request is linked to a bug report (and you will get all the praise in debian/changelog as well!)</li>
<li>If packaging changes are needed, ping the integration team so that they acknowledge them and ensure the packaging changes are part of the merge proposal.</li>
</ul>

<p>The <a href="https://launchpad.net/~ubuntu-unity">integration team</a>, in addition to be ready to help on request by any developer of our upstream, is having an active monitoring role over everything that is merged upstream. Everyone has some part of the whole stack attributed under his responsibility and will spot/start some discussion as needed if we are under the impression that some of those criterias are not met. If something not following those guidelines are confirmed to be spotted, anyone can do a revert by simply proposing another merge.</p>


<p>This enables to mainly answer the 2 other fear of what inline packaging may interfere by giving upstream control over the packaging. But this is the first safety net, we have a second one involved as soon as there is a packaging change since last daily release that we'll discuss in the next part.</p>


<h3>Consequences for other Ubuntu maintainers</h3>

<p>Even if we have our own area of expertise, Ubuntu maintainers can touch any part of what constitutes the distribution (MOTUs on universe/multiverse and core developers on anything). On that purpose, we didn't want daily release and inline packaging to change anything for them.</p>


<p>We added some warning on debian/control, pointing the Vcs-Bzr to the upstream branch with a comment above, this should highlight that any packaging change (if not urgent) needs to be a merge proposal against the upstream branch, as if we were going to change any of the upstream code. This is how the integration team is handling transitions as well, as any developer.</p>


<p>However, it happens that sometimes, an upload needs to be done in a short period of time, and we can't wait for next daily. If we can't even wait for manually triggering a daily release, the other option is to directly upload to Ubuntu, as we normally do for other pieces where we are not upstream for and still propose a branch for merging to the upstream trunk including those changes. If the "merge back" is is not done, the next daily release for that component will be paused, as it's detecting that there are newer changes in the distro, and the integration team will take care of backporting the change to the upstream trunk (as we are monitor as well uploads to the distributions).</p>


<p>Of course, the best is always to consult any person of the <a href="https://launchpad.net/~ubuntu-unity">integration team</a> first in case of any doubt. :)</p>


<h3>Side note on another positive effects of inline packages</h3>

<p>Having to inline all packages was a hard and long work, however in addition to the previous benefits hilighted, this enabled us to standardize all ~60 packages from our internal upstream around best practices. They should now all look familar once you have touch any of them. Indeed they all use debhelper 9, --fail-missing to ensure we ship all installed files, symbol files for C libraries using -c4 to ensure we force updating them, running autoreconf, debian/copyright using latest standard, split packages… In addition to be easier for us, it's as well easier for upstream as they are in a familar environment if they need to do themselves any changes and they can just use "bzr bd" to build any of them.</p>


<p>Also, we were able to remove and align more the distro patch we had on those components to push them upstream.</p>


<h2>Conclusion on an upstream branch flow</h2>

<p>So, you should now know everything on how upstream and packaging changes are integrated to the upstream branch with this new process, why we did it this way and what benefits we immediately can get from those. I guess having the same workflow for packaging and upstream changes is a net benefit and we can ensure that what we are delivering between those 2 are coherent, of a higher quality standard, and in control. This is what I would keep in mind if I would have only one thing to remember from all this :). Finally, we are taking into account the case of other maintainers needing to make any change to those components in Ubuntu and try to be flexible in our process for them.</p>



<p>Next part will discuss what happens to validate one particular stack and uploading that to the distribution on a daily basis. Keep in touch!</p>