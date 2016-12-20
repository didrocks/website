+++
title = "Ubuntu Make: the new Ubuntu Developer Tools Center name!"
date = "2014-12-09T11:26:00+01:00"
aliases = ["/post/Ubuntu-Make%3A-the-new-Ubuntu-Developer-Tools-Center-name%21"]
+++
    <p>Remember that 2 weeks ago, we <a href="/post/Ubuntu-Developers-Tools-needs-you-for-its-new-name%21">launched a contest</a> for a new name for UDTC, as we discussed at last UOS?</p>


<p>We got since then some awesome community participation with more than 40 name proposals over IRC, <a href="https://plus.google.com/+DidierRoche/posts/jWJFV4e7C8a">g+</a> or through this blog comments. Thanks to everyone participating!
After casting some votes, it seems that <strong>Ubuntu Make</strong> is the preferred choice, and was then chosen as our #ubuntulovesdeveloper tools new name! Congrats to <a href="https://plus.google.com/+WalterLapchynski">Walter Lapchynski</a> who came up with this great name and who will be shortly contacted to get his T-Shirt!</p>


<p>With this choice, we gain a cute command: <strong>umake</strong>, which really emphasizes that you can use Ubuntu as a great developer platform and create awesome applications and services, whatever your targeted platform is.</p>


<p><a href="https://launchpad.net/ubuntu/+source/ubuntu-make/0.2">Ubuntu Make 0.2</a> is then just released, and available in an Ubuntu series near you. We handled the transitions in configuration and packaging from the old names to the new one (and still provides the previous udtc command for some period). We updated, in addition to the code, <a href="https://wiki.ubuntu.com/ubuntu-make">our documentation</a>, <a href="#pnote-202-1" id="rev-pnote-202-1">1</a>]</sup> to benefit from new Ubuntu Make content! Tests were of course adapted to comply with this new scheme.<p>


<p>The last remaining piece using the old name is the <a href="https://github.com/didrocks/ubuntu-developer-tools-center">github project</a> which is going to move in the coming week to amore official namespace as well. We need to be able to ensure we don't lose pull requests and bug reports before doing this change. Stay tuned!</p>


<p>What else brings this release? Android Studio is out of beta and released 1.0 a few hours ago. One consequence was a change in their download layout, which got caught by tests, and we deliver a fix to our users in less than 24 hours after the breaking change! Note as well that upstream completely removed the ADT (eclipse) bundle download, as <a href="http://developer.android.com/tools/help/adt.html">it's not in active development</a> anymore. We consequently did remove it as well from Ubuntu Make. Finally, the Android SDK (as discussed here) is now downloaded directly through Android Studio setup wizard.</p>


<p>We hope that you will enjoy UMAKE 0.2! Now, it's time with <strong>Ubuntu</strong> for you to <strong>Make</strong> something awesome!</p>
<div class="footnotes"><h4 class="footnotes-title">Note</h4>
<p>[<a href="#rev-pnote-202-1" id="pnote-202-1">1</a>] and you can remove the previous address</p><div>
