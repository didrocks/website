+++
title = "Unity: release early, release often… release daily! (part 1)"
date = "2013-01-22T16:25:00+01:00"
tags = [ "devel", "libre", "PU", "quality", "ubuntu", "uds", "unity" ]
aliases = ["/post/Unity%3A-release-early%2C-release-often%E2%80%A6-release-daily%21"]
+++
    <p>This post is part of the Unity daily release process blog post suite. This is part one, you can find:</p>
<ul>
<li>part 2 on <a href="/post/Unity%3A-release-early%2C-release-often%E2%80%A6-release-daily%21-%28part-2%29">upstream merge process</a></li>
<li>part 3 on <a href="/post/Unity%3A-release-early%2C-release-often%E2%80%A6-release-daily%21-%28part-3%29">the daily release machinery</a></li>
<li>part 4 on <a href="/post/Unity%3A-release-early%2C-release-often%E2%80%A6-release-daily%21-%28part-4%29">how dependencies are handled between stacks</a></li>
<li>part 5 for <a href="/post/Unity%3A-release-early%2C-release-often%E2%80%A6-release-daily%21-%28part-5%29">a FAQ and conclusion</a></li>
</ul>

<p>For almost the past 2 weeks (and some months for other part of the stacks), we have automated daily release of most of the Unity components directly delivered to Ubuntu raring. And you can see <a href="https://lists.ubuntu.com/archives/raring-changes/2013-January/004602.html">those</a> <a href="https://lists.ubuntu.com/archives/raring-changes/2013-January/004507.html">different</a> <a href="https://lists.ubuntu.com/archives/raring-changes/2013-January/004508.html">kinds</a> <a href="https://lists.ubuntu.com/archives/raring-changes/2013-January/004316.html">of</a> <a href="https://lists.ubuntu.com/archives/raring-changes/2013-January/004093.html">automated</a> <a href="https://lists.ubuntu.com/archives/raring-changes/2013-January/004086.html">uploads</a> published to the ubuntu archive.</p>


<p>I'm really thrilled about this achievement that we <a href="https://blueprints.launchpad.net/ubuntu/+spec/desktop-r-ps-processes">discussed and setup as a goal</a> for the next release at past <a href="http://uds.ubuntu.com/">UDS</a>.</p>


<h2>Why?</h2>

<p>The whole Unity stack has grown tremendously in the past 3 years. At the time, we were able to release all components, plus packaging/uploading to ubuntu in less than an hour! Keeping the one week release cadence by then was quite easy, even though a lot of work. The benefit was that it enabled us to ensure that we have a fluid process to push what we developped upstream to our users.</p>


<p>As of today, teams have grown by quite some extends, and if we count everything that we develop for Unity nowadays, we have more than 60 components. This covers from <a href="https://launchpad.net/globalmenu-extension">indicators</a> to the <a href="https://launchpad.net/unico">theme engine</a>, from the <a href="https://launchpad.net/oif">open input framework</a> to all our tests infrastructure like <a href="https://launchpad.net/autopilot">autopilot</a>, from <a href="https://launchpad.net/webapps">webapps</a> to <a href="https://launchpad.net/online-accounts">web credentials</a>, from lenses to libunity, and finally from compiz to Unity itself without forgetting nux, bamf… and the family is still growing rapidly with a bunch of new scopes coming down the pipe through the <a href="http://www.iloveubuntu.net/100-scopes-installed-default-parts-systemd-incorporated-upstart-phased-updates-ubuntu-1304s-official">100 scopes project</a>, <a href="http://developer.ubuntu.com/get-started/gomobile/">our own SDK for the Ubuntu phone</a>, the example applications for this platform we are about to upload to Ubuntu as well… Well, you got it, the story is far from ending!</p>


<p>So, it's clear that those numbers of components that we develop and support will only go higher and higher. The integration team already scaled by large extends their work hours and rush to get everything delivered timely to our user base<sup>[<a href="#pnote-186-1" id="rev-pnote-186-1">1</a>]</sup>. However, it's with no question that we won't be able to do that forever. We don't want as well introducing artificial delays to our own upstream on when we are delivering stuff to our users. We needed to solve that issue while not paying any price on quality, nor balancing the experience we deliver to our users. We want to keep high standards, and even, why not allying this need while providing an even a better, more reliable, and better evaluation before releasing of what we eventually upload to Ubuntu from our upstreams. Getting our cake and eat it too! :)<p>


<h2>Trying to change for the better</h2>

<p>What was done in the last couple of cycles was to separate between 2 groups the delivery of those packages to the users. There was an upstream integration team, which will hand over to the Ubuntu platform team theorically ready-to-upload packages, making the reviews, helping them, fixing some issues and finally sponsoring their work to Ubuntu. However, this didn't really work for various reasons and we quickly realized that this ended up just complexifying the process instead of easing it out. You can see a diagram of where we ended up when looking back at the situation:</p>


<p><a href="/public/ubuntu/daily-release/releasing_unity_pre_13.04.png" title="end of 12.04 and 12.10 release process"><img src="/public/ubuntu/daily-release/.releasing_unity_pre_13.04_m.jpg" alt="end of 12.04 and 12.10 release process" style="display:block; margin:0 auto;" title="end of 12.04 and 12.10 release process, janv. 2013" /></a></p>


<p>Seems easy isn't it? ;) Due to those inner loops and gotchas, in addition to the whole new set of components, we went from a weekly release cadence to doing 4 to 5 solid releases during a whole cycle.</p>


<p>Discussing about it with <a href="http://theravingrick.blogspot.fr/">Rick Spencer</a>, he gave me a blank card on thinking how we can make this more fluid to our users and developers. Indeed, with all the work piling up, it wasn't possible to release immediatly the good work that upstream did in the past days, which can lead to some frustration as well. I clearly remember that Rick used the term "think about platform<sup>[<a href="#pnote-186-2" id="rev-pnote-186-2">2</a>]</sup> as a service". This kind of immediatly echoed in me, and I thought, "why not trying that… why not releasing all our code <em>everyday</em> and delivering a service enabling us to do that?"<p>


<p><img src="/public/ubuntu/daily-release/.daily_release_simplified_m.jpg" alt="Daily release, really simplified diagram" style="display:block; margin:0 auto;" title="Daily release, really simplified diagram, janv. 2013" /></p>


<p>Even if not planned from the beginning (sorry, no dark, hidden, evil plan here), thinking about it, this makes sense as part of some kind a logical progression from where we started since Unity exists:</p>
<ul>
<li>Releasing every week, trying manually to get the release in a reasonable shape before uploading to Ubuntu</li>
<li>Raise the quality bar, put some processes for merge reviewing.</li>
<li>Adding <a href="https://blueprints.launchpad.net/ubuntu/+spec/acceptance-criteria-general">Acceptance Criterias</a> thanks to Jason Warner, ensuring that we are getting some more and more good tests and formal conditions on doing a release</li>
<li>Automate those merges through a bot, ensuring that every commits in trunk builds fine, that unit tests are passing</li>
<li>Raise again the quality bar, adding more and more integration tests</li>
</ul>

<p>Being able and ensuring we are able to release daily seems really, looking at it, par of the next logical step! But it wouldn't have been possible without all those past achievements.</p>



<h2>Advantages of daily releases</h2>


<p>It's quite immediate to see a bunch of positive aspects of doing daily releases:</p>
<ul>
<li>We can spot way faster regressions. If a new issue arose, it's easier to bisect through packages and find the day when the new regression or incorrect behavior started to happen, then, looking at the few commits to trunk (3-5?) that was done this day and pinpoint what introduced it.</li>
<li>This enables us to deliver everything in a rapid, reliable, predictable and fluid process. We won't have crazy rushes as we had in the past cycles around goal dates like feature freezes to get everything in the hand of the user by then. This will be delivered automatically the day after to everyone.</li>
<li>I see also this as a strong motivation for the whole community who contributes to those projects. Not having to wait a random date hidden in a wiki for a "planned release date" to see the hard work you put into your code to be propagated to the user's machines. You can immediately see the effect of your hacking on the broader community. If it's reviewed and approved for merging (and tests passes, I'll come back to that later), it will be in Ubuntu tomorrow, less than 24 hours after your work reached trunk! How awesome is that?</li>
<li>This also means that developers will <em>only</em> need to build the components they are working on. No need for instance to rebuild compiz or nux to do an Unity patch because the API changed and you need "latest everything" to build. Lowering the entry for contributing and chances that you have unwanted files in /usr/local staying around conflicting with the system install.</li>
</ul>

<h2>Challenges of daily release</h2>

<p>Sure, this comes with various risks that we had to take into account when designing this new process:</p>
<ul>
<li>The main one is "yeah, it's automated, how can you be sure you don't break Ubuntu pushing blindly upstream code to it?". It's a reasonable objection and we didn't ignore it at all (having the history of years of personal pain on what it takes to get a release out in an acceptable shape to push to Ubuntu) :)</li>
<li>How to interact properly with the Ubuntu processes? Only <a href="https://wiki.ubuntu.com/UbuntuDevelopers#CoreDev">core developpers</a>, <a href="https://wiki.ubuntu.com/UbuntuDevelopers#MOTU">motus</a>, and <a href="https://wiki.ubuntu.com/UbuntuDevelopers#PerPackage">per-package uploaders</a> have upload rights to Ubuntu. Will this new process in some way give our internal upstream the keys to the archives, without having them proper upload rights?</li>
<li>How ensuring packaging changes and upstream changes are in sync, when preparing a daily release?</li>
<li>How making useful information in the changelog so that someone not following closely upstream merges but only looking at the published packages in <a href="https://lists.ubuntu.com/mailman/listinfo/Raring-changes">raring-changes</a> mailing list or update-manager can see what mainly changed in an upload?</li>
<li>How to deal with ABI breaks and such transitions, especially when they are cross-stacks (like a bamf API change impacting both indicators and Unity)? How to ensure what we deliver is consistent across the whole stacks of components?</li>
<li>How do we ensure that we are not bindly releasing useless changes (or even an upload with no change at all) and so, using more bandwith, build time, and so on, for nothing?</li>
</ul>

<h2>How then?</h2>

<p>I'll detail much of those questions and how we try to address those challenges in the subsequent suite of blog posts. Just for now, to sum up, we have a good automated test suite, and stacks are only uploaded to Ubuntu if:</p>
<ol>
<li>their internal and integration tests are passing above a certain theshold of accepted failures.</li>
<li>they don't regress other stacks</li>
</ol>

<p>Stabilizing those tests to get reliable result was a tremendous work for a cross-team effort, and I'm really glad that we are confident in them to finally enable those daily release.</p>


<p>On another hand, additional control is made to ensure that packaging changes are not committed without someone with upload right being in the loop for acking the final change before the copy to the distribution happens.</p>


<p>Of course, the whole machinery is not limited to this and is in fact way more complicated, I'll have the pleasure the write about those in separate blog posts in the following days. Stay tuned!</p>
<div class="footnotes"><h4>Notes</h4>
<p>[<a href="#rev-pnote-186-1" id="pnote-186-1">1</a>] particularly around the feature freeze<p>
<p>[<a href="#rev-pnote-186-2" id="pnote-186-2">2</a>] the Ubuntu platform team here, not Ubuntu itself ;)</p><div>
