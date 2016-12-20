+++
title = "Releasing a precise Unity 5.0 to Ubuntu 12.04"
date = "2012-01-11T16:10:00+01:00"
tags = [ "libre", "PU", "PUF", "ubuntu" ]
aliases = ["/post/Releasing-a-precise-Unity-5.0-to-Ubuntu-12.04"]
+++
    <p>The <a href="http://www.canonical.com">Canonical</a> ubuntu platform and product strategy teams are gathering in Budapest this week to tackle as much work as possible on precise pangolin. Despite the promise of snow and cheap beers, we are working hard on getting <a href="https://launchpad.net/unity/+milestone/5.0.0">Unity 5.0</a> out of the door.</p>


<p>One of the goal of this release is to increase quality, precision, no regression on the work we push to the unstable version of ubuntu. The desktop experience team made automated and manual tests for that and we can already see the first benefits from it. We pushed an <a href="https://jenkins.qa.ubuntu.com/view/Precise%20Unity%20Merger/">automated building infrastructure with public test reports</a> to have commits automatically tested, pushed to the <a href="http://en.wikipedia.org/wiki/Trunk_(software)">trunk of development branch</a> as well as available packages in a ppa.</p>


<p>With all those news features and requirements, we needed to redesign the release process and that's what we have done last Monday. Let me expose the few steps I will explain there.</p>

<ol>
<li>On Monday evening, we have frozen the trunk, which means, no more new code can enter unity at this point (as well for all the related components like unity-2d, nux, dee, libunity, bamf, unity lenses). Only selected branches can now get in, and those are picked only if they contribute to getting closer to this release quality.</li>
<li>Then, after ensuring on Tuesday that people can safely install the new release candidate, the <a href="https://launchpad.net/~unity-team/+archive/ppa">unity-team ppa</a> started to contain the whole latest of what will soon be the 5.0 version of unity. If you install from this ppa, you will see a kind prompt asking you to contribute when logging back to your desktop.</li>
<li>This prompts help getting to our main goal, which is ensurin the quality of the new release. Multiple things have been put in practice for that. The desktop experience team qualified the release using their manual tests and running automated ones again. <a href="http://agateau.wordpress.com/">Aurélien</a> and I run our own manual tests (120 of them, trying to covering the whole Unity functionnalities). This finished on Tuesday evening (we rephrased some) and we rebuilt all needed packages again, as well as some other dependencies like update-manager, usb-creator, nautilus, empathy, and gwibber to still make them working when you install from the ppa (ABI bumps). From those test we spotted regressions and get them fixed/fix them, regenerate everything and such.</li>
<li>The manual test wrapper over checkbox is also automatically installed from the ppa. Which means that *YOU* can help too! I'm bootstrapping this process with the <a href="http://www.markshuttleworth.com/archives/938">French Musketeers</a> to ensure everything is correct and ready for the next release. How to help there will be widespread for Unity 5.2. More on that soon!</li>
<li>On Thursday morning, we will collect the results from the tests, see what's still needed to be fixed (if it's the case) and then cycle back on the previous steps.</li>
<li>At the same time, the bugs that are fixed will be milestoned, some cleanup will be done and everything will be then ready to format an explicit text of what's in the new release.</li>
<li>Then, the process is well known: we will issue tarballs for every projects we need to upload</li>
<li>Packaging them properly, with the right build-dependencies and needed tweaks will be done, and upload to precise to share the love!</li>
<li>Finally, every non fixed bugs but targeted will be reported to the next milestones.</li>
</ol>

<p>And that's it! Everyone will be able to enjoy the whole new shiny Unity 5.0, containing a bunch of bug fixes, as well, as all the layout and ground for being rock solid, speeded up and just… precise Unity version!</p>