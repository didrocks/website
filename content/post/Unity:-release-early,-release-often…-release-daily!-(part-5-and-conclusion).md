+++
title = "Unity: release early, release often… release daily! (part 5 and conclusion)"
date = "2013-02-05T13:40:00+01:00"
tags = [ "devel", "libre", "PU", "quality", "ubuntu", "uds", "unity" ]
aliases = ["/post/Unity%3A-release-early%2C-release-often%E2%80%A6-release-daily%21-%28part-5-and-conclusion%29"]
+++
    <p>This post is part of the <a href="/post/Unity%3A-release-early%2C-release-often%E2%80%A6-release-daily%21">Unity daily release process blog post suite</a>.</p>


<p>After a week to let people ask questions about the daily release process, I guess it's time to catch up and conclude this serie with a FAQ and some thoughts for the future.</p>


<h2>FAQ</h2>


<p>The FAQ is divided in multiple sequences depending on your role in the development of ubuntu, with the hope that you will be able to find what you are looking for quicker this way. If you have any question that isn't addressed below, please ping didrocks on IRC freenode (#ubuntu-unity) and I'll complete this page.</p>



<h3>Upstream developers</h3>


<h4>What's needed to be done when proposing a branch or reviewing?</h4>

<p>As discussed <a href="/post/Unity%3A-release-early%2C-release-often%E2%80%A6-release-daily%21-%28part-2%29">in this part</a>, when proposing a branch or reviewing a peer's work, multiple rules have been established. The developers needs to:</p>
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

<p>When you approve a change, do not forget to check that there is a commit message in launchpad provided and to set the global status to "approved".</p>


<h4>When do changes need to land in a coherent piece?</h4>

<p>Some changes can happen and touch various components (even across different stacks). To avoid loosing a daily release because only half of transition landed in some upstream projects (and so, tests will fail because they catch this issues, isn't it? ;)), it's asked to ensure that all transitions are <em>merged</em> by 00 UTC. Then, you can start approving again transition starting from 06 UTC. We can try to make this window shorter in the future, but let's see first how this work in practice. For any big transition, please coordinate with the <a href="https://launchpad.net/~ubuntu-unity">ubuntu-unity team</a> team so that they can give a hand.</p>


<h4>I have a change involving packaging modifications</h4>

<p>As told in a previous section, just ask for the <a href="https://launchpad.net/~ubuntu-unity">ubuntu-unity team</a> to do some review or assisting in doing those changes.</p>


<h4>I want to build the package locally myself</h4>

<p>If you have done some packaging changes, or added a C symbol, you maybe want to ensure that it's building fine. Quite simple, just go into your branch directory and run <q>$ bzr bd</q> (from the bzr-builddeb package). This will build your package on your machine using the same flags and checks than on the builders. No need to autoreconf if you are using autotools, everything is handled for you. You will get warned as well if some installed files are missing. ;)</p>


<h4>I need to break an API/ABI</h4>

<p>The first question is: "are you sure?". Remember that API changes are a PITA for all third parties, in particular if your API is public. Retro-compatibility is key. And yes, your dbus interface should be considered as part of your API!</p>


<p>If you still want to do that, ensure that you bump your soname if the ABI is broken, then, multiple packaging changes are needed. Consequently, ensure to ping your lovely <a href="https://launchpad.net/~ubuntu-unity">ubuntu-unity team</a> to request assistance. The transition should happen (if no retrocompatibility is provided) within the previously mentionned timeframe to have everything landing in coherence.</p>


<p>You also need to ensure that for all components build-depending on the API you changed, you bumped the associated build dependency version in debian/control.</p>


<h4>To what version should I bump the build dependency version?</h4>

<p>Let's say you added an API to package A. B is depending on A and you want to start using this new awesome API. What version should be used in debian/control to ensure I'm building against the new A?</p>


<p>This is quite easy, the build-dependencies in debian/control should look like: A &gt;= 6.0daily13.03.01 (eventually ended by -0ubuntuX). Strip the end -0ubuntuX and append then bzr&lt;rev_where_you_added_the_api_in_trunk&gt;. For instance, let's imagine you added the API in rev 42 in trunk, so the build-dep will change from:</p>


<p><q>A &gt;= 6.0daily13.03.01</q></p>


<p>to:</p>


<p><q>A &gt;= 6.0daily13.03.01bzr42</q></p>


<p>This is to ensure the upstream merger will be able to take it.</p>


<p>If no "daily" string is provided in the dependency you add, please check with the <a href="https://launchpad.net/~ubuntu-unity">ubuntu-unity team</a>.</p>


<h4>I'm exposing a new C symbols in my library, it seems that some packaging changes are needed…</h4>

<p>Debian packages have a debian/&lt;packagename&gt;.symbols file which lists all exposed symbols from your library (we only do that for C libraries as C++ does mangle the name per architecture). When you try to build the package containing a new symbol, you will see something like:</p>



<blockquote><p>--- debian/libdee-1.0-4.symbols (libdee-1.0-4_1.0.14-0ubuntu2_amd64)</p>
<p>
+++ dpkg-gensymbolsvPEBau	2013-02-05 09:43:28.529715528 +0100</p>
<p>
<code> -4,6 +4,7 </code></p>
<p>
dee_analyzer_collate_cmp@Base 0.5.22-1ubuntu1</p>
<p>
dee_analyzer_collate_cmp_func@Base 0.5.22-1ubuntu1</p>
<p>
dee_analyzer_collate_key@Base 0.5.22-1ubuntu1</p>
<p>
+ dee_analyzer_get_type@Base 1.0.14-0ubuntu2</p>
<p>
dee_analyzer_new@Base 0.5.22-1ubuntu1</p>
<p>
dee_analyzer_tokenize@Base 0.5.22-1ubuntu1</p>
<p>
dee_client_get_type@Base 1.0.0</p></blockquote>



<p>The diff shows that debian/libdee-1.0-4.symbols doesn't list the exposed <q>dee_analyzer_get_type</q> new symbol. You see a version number next to it, corresponding to the current package version. The issue is that you don't know what will be the package version in the future when the next daily release will happen (even if you can infer), what to put then?</p>


<p>The answer is easier than you might think. As explained in the <a href="/post/Unity%3A-release-early%2C-release-often%E2%80%A6-release-daily%21-%28part-3%29">corresponding section</a>, the daily release bot will know what version is needed, so you just need to give a hint that the new symbol is there for the upstream merger to pass.</p>


<p>For than, just include the symbol, with <em>0replaceme</em><sup>[<a href="#pnote-190-1" id="rev-pnote-190-1">1</a>]</sup> as the version for the branch you will propose as a merge request. You will thus set in your symbols file:<p>

<blockquote><p>dee_analyzer_collate_cmp@Base 0.5.22-1ubuntu1</p>
<p>
dee_analyzer_collate_cmp_func@Base 0.5.22-1ubuntu1</p>
<p>
dee_analyzer_collate_key@Base 0.5.22-1ubuntu1</p>
<p>
dee_analyzer_get_type@Base 0replaceme</p>
<p>
dee_analyzer_new@Base 0.5.22-1ubuntu1</p>
<p>
dee_analyzer_tokenize@Base 0.5.22-1ubuntu1</p>
<p>
dee_client_get_type@Base 1.0.0</p></blockquote>



<p>The <q>dee_analyzer_get_type@Base 0replaceme</q> line will be replace with the exact version we release in the next daily release.</p>


<h4>My name deserves to be in the changelog!</h4>

<p>I'm sure you want the world to know what great modifications you have introduced. To get this praise (and blame ;)), we strongly advise you to link your change to a bug. This can be done in multiple ways, like link bugs to a merge proposal before it's approved (you link a branch to the bug report in launchpad), or use "bzr commit --fixes lp:XXXX" so that automatically links it for you when proposing the merge for reviewing, or put in a commit message something like "this fixes bug…". This will avoid the changelog to have only empty content.</p>


<p>I invite you to read "Why not including all commits in debian/changelog?" from <a href="/post/Unity%3A-release-early%2C-release-often%E2%80%A6-release-daily%21-%28part-3%29">this section</a> to understand why we don't include everything, but only bug reports title in the changelog.</p>


<h3>Ubuntu Maintainers</h3>


<h4>I want to upload a change to a package under daily release process</h4>


<p>Multiple possibilities there.</p>


<h5>The change itself can wait next daily release</h5>

<p>If it's an upstream change, we strongly advise you to follow the same procedure and rules than the upstream developers, meaning, proposing a merge request and so on… <q>bzr lp-propose</q> and <q>bzr branch lp:&lt;source_package_name&gt;</q> are regularly and automatically adjusted as part of the daily release process to point to the right branches so that you can find them easily. Vcs-Bzr in debian/control should point to the right location as well.</p>


<p>Then, just refer to the upstream merging guidelines above. Once approved, this change will be in the next daily release (if tests pass).</p>


<h5>An urgent change is needed</h5>

<p>If the change needs to be done right away, but can wait for a full testing round (~2 hours), you can:</p>
<ul>
<li>Propose your merge request</li>
<li>Ping someone from the <a href="https://launchpad.net/~ubuntu-unity">ubuntu-unity team</a> who will ensure the branch is reviewing in priority and rerun a daily release manually including your change. That way, your change will get the same quality reviews and tests checking than anything else entering the distro.</li>
</ul>

<h5>It's really urgent</h5>

<p>You can upload right away your change then, the next daily will be blocked for that component though (and only for that component) until your change reaches upstream. So please, be a good citizen and avoid more churn in proposing your change back to the upstream repo (including changelog), pinging the <a href="https://launchpad.net/~ubuntu-unity">ubuntu-unity team</a> preferably.</p>


<h4>I'm making a change, should I provide anything in debian/changelog?</h4>

<p>Not really, as mentioned earlier, if you link to a bug or mention in a commit message a bug number in your branch before proposing for review, this will be taken into account and debian/changelog will be populated automatically with your name as part of the next daily release. You can still provide it manually and the daily release will ensure there is no duplication if it was already mentionned.</p>


<p>If you provide it manually, just: <q>dch -i</q> and ensure you have UNRELEASED for an unreleased version to ubuntu (without direct upload).</p>


<h3>ubuntu-unity team</h3>

<p>Note that in all following commands, if -r &lt;release&gt; is not provided, "head" is assumed. Also, ensure you have your credentials working and be connected to the VPN (the QA team is responsible for providing those jenkins credentials).</p>


<h4>Where is the jenkins start page?</h4>

<p><a href="http://jenkins.qa.ubuntu.com/view/cu2d/">There it is</a>.</p>


<h4>Forcing a stack publication</h4>

<p>After reviewing and ensuring that we can release a stack (see section about manual publication causes, like packaging changes to review or upstream stack failing/in manual mode). Then run:</p>


<blockquote><p>$ cu2d-run -P &lt;stack_name&gt; -r &lt;release&gt;</p></blockquote>


<p>example for indicators, head release:</p>

<blockquote><p>$ cu2d-run -P indicators</p></blockquote>


<p>Remember that the master "head" job is not rerun, so it will stay in its current state (unstable/yellow). The publish job should though goes from unstable/yellow to green if everything went well.</p>


<h4>Rerun a full stack daily release, rebuilding everything</h4>

<blockquote><p>$ cu2d-run -R &lt;stack_name&gt; -r &lt;release&gt;</p></blockquote>


<h4>Rerun a partial stack daily release, rebuilding only some components</h4>

<blockquote><p>$ cu2d-run -R &lt;stack_name&gt; -r &lt;release&gt; &lt;components&gt;</p></blockquote>


<p>/!\ this will keep the state and take into account (failure to build state for instance) and the binaries packages for the other components which were part of this daily release, but not rebuilt.</p>


<p>for instance, keeping dee and libunity, but rebuilding compiz and unity from the unity, head release stack:</p>

<blockquote><p>$ cu2d-run -R unity compiz unity</p></blockquote>


<p>The integration tests will still take everything we have (so including dee and libunity) at the latest stack with the new trunk content for compiz and unity when we relaunched this command.</p>


<h4>Rerun integration tests, but taking all ubuntu-unity ppa's content (like transition needing publication of the indicator and unity stacks):</h4>

<blockquote><p>$ cu2d-run -R &lt;stack_name&gt; -r &lt;release&gt; --check-with-whole-ppa</p></blockquote>


<p>This command doesn't rebuild anything, just run the "check" step, but with the whole ppa content (basically doing a dist-upgrading instead of selecting only desired components from this stack). This is handy when for instance a transition involved the indicator and unity stack. So the indicator integration tests failed (we don't have the latest unity stack built yet). Unity built and tests pass. To be able to release the indicator stack as well as the unity stack, we need the indicator stack to publish, for that, we relaunch only the indicator tests, but with the whole ppa content, meaning last Unity.</p>


<p>In that case, the publish for the indicators will be manual (you will need to force the publication) and as well, you should ensure that you will publish the unity stack at the same time as well to have everything in coherence copied to the -proposed pocket. More information on the stack dependency <a href="/post/Unity%3A-release-early%2C-release-often%E2%80%A6-release-daily%21-%28part-4%29">in this section</a>.</p>


<h4>Adding/removing components to a stack</h4>


<p>The stack files are living in jenkins/etc/. This yaml file structure should be straightforward. Note that the tuple <em>component: branch_location</em> is optional (it will be set to lp:component if not provided). So add/change what you need in those file.</p>


<p>Be aware that all running jobs are stopped for this stack.</p>


<blockquote><p>$ cu2d-update-stack -U &lt;path_to_stack_file&gt;</p></blockquote>


<p>This will reset/reconfigure as well all branches, add -S if you don't have access to them (but ensure someone will configure them still at least once)</p>



<h4>Different level of failures acceptance.</h4>

<p>We do accept some integration tests failing (no unit tests should fail though as ran as part of package building). The different level of those triggers are defined in a file like <a href="http://bazaar.launchpad.net/~cupstream2distro-maintainers/cupstream2distro/trunk/view/head:/jenkins/etc/autopilot.rc">jenkins/etc/autopilot.rc</a>. Those are different level of failures, regressions, skipped and removed tests per stack. The real file depends on stackname and are found in $jenkins/cu2d/&lt;stack_name&gt;.autopilotrc as described in the <a href="http://bazaar.launchpad.net/~cupstream2distro-maintainers/cupstream2distro/trunk/view/head:/jenkins/templates/check-stack-config.xml.tmpl#L84">-check template</a>.</p>


<h4>Monitoring upstream uploads and merges</h4>

<p>Remember to regularly look at the -changes mailing list to pick if a manual upload has been done and port the changes back (if not proposed by the ubuntu developer making the upload) to the upstream branch. The component which had a manual upload is ignored for daily release until this backport is done, meaning that no new commits will be published until then (you can see the <q>prepare</q> job for that component will turn unstable).</p>


<p>Also, ensure as you are watching all upstream merges to follow <a href="http://theravingrick.blogspot.fr/2011/11/i-mentioned-at-closing-session-of-uds.html">our acceptance criterias</a> that the "latestsnapshot" branches generated automatically by the daily release process has been merged successfully by the upstream merger.</p>


<h4>Boostrapping a new component for daily release</h4>

<p>A lot of information is available on <a href="https://wiki.ubuntu.com/DailyRelease/InlinePackaging">this wiki page</a>.</p>


<h2>Conclusion</h2>

<p>Phew! This was a long serie of blog post. I hoped that you appreciated to have a deeper look of what daily release means, how this work, and also that the remaining questions have been answered by the above FAQ. I've just ported all those information on the <a href="/post/Ubuntu wiki">https://wiki.ubuntu.com/DailyRelease</a>.</p>


<p>Of course, this is not the end of the road, I have my personal Santa Claus wishlist like UTAH being more stable (this is under work), the daily image provisionning with success more often than now, refactorisation work taking integration tests into account, less Xorg crashes when completing those tests… This is currently adding a little bit of overhead as of course, any of this will reject the whole test suite and thus the whole stack. We prefer to always be on the safe side and not accepting something in an unknown state!</p>


<p>Also, on the upstream stack and daily release process as well, there are rooms for improvments, like having less upstream integration tests failing (despite awesome progress already there), I wish I can soon turn that down to 3% of accepted failure at most for unity instead of the current 7% one. Dedicated integrations tests for indicators would be awesome as well (stealing the unity ones for now and only running those involving indicators, but of course, not everything is covered). Having also some oif, webapps and wecreds integration tests is one of my main priority, and we are working with the various upstream developers to get this done ASAP so that we have a better safety net than today. On the daily release process itself, maybe freezing all trunks in a go would be a nice improvement, more tests for the system itself as well is under work and my personal goal to reach 100% test coverage for the scripts before making any additional changes. Finally, some thoughts for later but a dashboard would enable to have a higher view than just using jenkins directly for reporting, to get a quicker view on all stack statuses.</p>


<p>Of course, I'm sure that we'll experience some bumpy roads with this daily release process through the cycle, but I'm confident we'll overcome them and I am really enthousiast about our first releases and how smoothly they went until now (we didn't break the distro… yet :p<sup>[<a href="#pnote-190-2" id="rev-pnote-190-2">2</a>]</sup>. I'm sure this is a huge change for everyone and I want again give a big thank you to everyone involved into that effort, from the distro people to upstream and diverse contributors on the QA team. Changing to such a model isn't easy, it's not only having yet another process, but it's really a change of mindset in the way we deliver new features and components to Ubuntu. This might be frightening, but I guess it's the only way we have to ensure a systematic delivery, continuous and increasing quality while getting well a really short feedback loop to be able to react promptly to the changes we are introducing to our audience. I'm confident we are already collecting all the fruits of this effort and going to expand this more and more in the future.<p>


<p>For all those reasons, thanks again to all parties involved and let's have <em><ins>our daily shot of Unity</ins></em>!</p>



<p>I couldn't finish without a special thanks to <a href="http://popey.com/">Alan Pope</a> for being a patient first reader, and fixing tediously various typos that I introduced into the first draft of those blog posts. ;)</p>
<div class="footnotes"><h4>Notes</h4>
<p>[<a href="#rev-pnote-190-1" id="pnote-190-1">1</a>] "0replacemepleasepleaseplease" or anything else appended to <em>0replaceme</em> is accepted as well if you feel adventurous ;)<p>
<p>[<a href="#rev-pnote-190-2" id="pnote-190-2">2</a>] fingers crossed, touching wood and all voodoo magic required</p><div>
