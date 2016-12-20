+++
title = "Release early, release often, release every 4h!"
date = "2013-08-14T09:40:00+02:00"
tags = [ "devel", "libre", "PU", "quality", "ubuntu" ]
aliases = ["/post/Release-early%2C-release-often%2C-release-every-4h%21"]
+++
    <p>It's been a long time I didn't talk about our daily release process on this blog.</p>


<p>For those who are you not aware about it, this is what enables us to <a href="https://wiki.ubuntu.com/DailyRelease">release continuously</a> most of the components we, as the ubuntu community, are upstream for, to get into the baseline.</p>


<p><img src="/public/ubuntu/.push_rock_m.jpg" alt="Sometimes, releases can feel like pushing huge rocks" style="display:block; margin:0 auto;" title="Sometimes, releases can feel like pushing huge rocks, août 2013" /></p>


<h3>Historic</h3>

<p>Some quick stats since the system is in place (nearly since last December for full production):</p>
<ul>
<li>we are releasing as of now <strong>245 components</strong> to distro every day (it means, everytime a meaningfull change is in any of those 245 components, we will try to release it). Those are segregated in 22 stacks.</li>
<li>this created <strong>309 uploads</strong> in raring</li>
<li>we are now at near <strong>800 uploads</strong> in saucy (and the exact number is changing hour after hour)</li>
</ul>

<p>Those numbers are even without counting feature branches (temporary forks of a set of branches) and SRUs.</p>


<h3>Getting further</h3>

<p>However, seeing the number of transitions we have everyday, it seems that we needed to be even faster. I was challenged by Rick and Alexander to speed up the process, even if it was always possible to run manually some part of it<sup>[<a href="#pnote-194-1" id="rev-pnote-194-1">1</a>]</sup>. I'm happy to announce that now, we are capable of releasing <strong>every 4h</strong> to distro! Time for a branch proposed to trunk to the distro is drastically reduced thanks to this. But we still took great care to  keep the same safety net though with tests running, ABI handling,  and not pushing every commit to distro.<p>


<p>This new process was in beta since last Thursday and now that everyone is briefed about it, it's time for this to get in official production time!</p>


<h3>Modification to the process</h3>

<p>For this to succeed, some modifications to the whole process have been put in place:</p>
<ul>
<li>now that we release every 4 hours, it means we need a production-like team always looking at the results. That was put in place with the <a href="https://launchpad.net/~ubuntu-unity">~ubuntu-unity</a> team, and the schedule is <a href="https://docs.google.com/a/canonical.com/spreadsheet/ccc?key=0AuDk72Lpx8U5dHFtUmlPOUtCRk8zR2dtaEpIbUVhMmc#gid=4">publically available here</a>.</li>
<li>the consequence is that we have no more "stack ownership", everyone in the team is responsible for the whole set now.</li>
<li>it means as well that upstream now have a window of 4 hours before the "tick" (look at the cross on the schedule) to push stuff in different trunks in a coherence piece rather than once a day, before 00 UTC. It's the natural pressure between speed versus safety for big transitions.</li>
<li>better communication and tracking were needed (especially as the production is looked after by different people along the day). We needed as well to ensure everything is available for upstreams to know where their code is, what bugs affects them and so on… So now, everytime there is an issue, a bug is opened, upstream is pinged about it and we write about those <a href="https://docs.google.com/a/canonical.com/spreadsheet/ccc?key=0AuDk72Lpx8U5dHFtUmlPOUtCRk8zR2dtaEpIbUVhMmc#gid=3">on that tab</a>. We escalate after 3 days if nothing is fixed by then.</li>
<li>we will reduce as much as possible "manual on demand rebuild" as the next run will fix it.</li>
</ul>

<p>Also, I wanted that to not become a burden for our team, so some technical changes have been put in place:</p>
<ul>
<li>not relying anymore on the jenkins lock system as the workflow is way more complex than what jenkins can handle itself.</li>
<li>possible to publish the previous run until the new stack started to build (but still possible even if the stack started to wait).</li>
<li>if a stack B is blocked on stack A because A is in manual publishing mode (packaging changes for instance), forcing the publication of A will try to republish B if it was tested against this version of A. (also, having that scaling up and cascading as needed). So less manual publication needed and push button work \o/</li>
<li>Some additional "force rebuild mode" which can retrigger automatically some components to rebuild if we know that it's building against a component which doesn't ensure any ABI stability (but only rebuild if that component was renewed).</li>
<li>ensure as well that we can't deadlock in jenkins (having hundreds of jobs running every 4h).</li>
<li>the dependency and order between stacks are not relying anymore on any scheduling, it's all computed and ordered properly now (thanks Jean-Baptiste to have help on the last two items).</li>
</ul>

<h3>Final words</h3>

<p>Since the beginning of the week, in addition to seeing way more speed up for delivering work to the baseline, we also have seen the side benefit that if everybody is looking at the issues more regularly, there are less coordination work to do and each tick is less work in the end. Now, I'm counting on the <a href="https://launchpad.net/~ubuntu-unity">~ubuntu-unity</a> team to keep that up and looking at the production throughout the day. Thanks guys! :)</p>


<p>I always keep in mind the motto "when something is hard, let's do it more often". I think we apply that one quite well. :)</p>
<div class="footnotes"><h4 class="footnotes-title">Note</h4>
<p>[<a href="#rev-pnote-194-1" id="pnote-194-1">1</a>] what some upstreams were used to ask us</p><div>
