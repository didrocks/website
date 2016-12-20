+++
title = "Just released Ubuntu Developer Tools Center 0.1.1"
date = "2014-11-04T10:25:00+01:00"
tags = [ "android", "devel", "libre", "PU", "quality", "ubuntu", "ubuntulovesdevs" ]
aliases = ["/post/Just-released-Ubuntu-Developer-Tools-Center-0.1.1"]
+++
    <p>In a nutshell, <a href="https://github.com/didrocks/ubuntu-developer-tools-center/commit/110a68c2506da303d37cc51ab9fa9e2528af0794">this release</a> is fixing the changes introduced by the new Android Studio (0.8.14) download in beta channel.</p>


<p>The web page changed from a md5 checksum to a sha1. We do that check as we grab that information from a secure https connexion, but then, we download from dl.google.com which isn't https enabled.</p>


<p>Thanks again to <a href="https://github.com/Tinche">Tin Tvrtković</a> who did the work to add this support and makes the necessary changes! :) I just had to do a little cleanup and then releasing it!</p>


<h3>How did we noticed it?</h3>

<p>Multiple source of inputs:</p>
<ul>
<li>The <a href="https://jenkins.qa.ubuntu.com/view/All/job/udtc-trusty-tests/410/">automated tests</a> which are running every couple of hours were both <a href="https://jenkins.qa.ubuntu.com/view/All/job/udtc-trusty-tests/label=ps-trusty-desktop-i386-1,type=large/410/testReport/">failing reliably</a> in trunk and latest release version, on both architecture, only for android studio in the large tests (medium tests, with fake assets were fine). This clearly meant that a third-party broke us!</li>
<li><a href="https://github.com/didrocks/ubuntu-developer-tools-center/issues/44">Some bug reports</a> from the community</li>
<li>Some directly addressed emails!</li>
</ul>

<p>It was a nice way to see that Ubuntu Developer Tools Center is quite used, even if this is through an external breakage. Thanks to all those inputs, the fix went in shortly!</p>


<h3>On Android Studio not shipping the sdk anymore.</h3>

<p>With this new release, Android Studio is not shipping the sdk embedded anymore and it needs to be downloaded separately. I think this is a nice move: decoupling editor code from sdk tools just makes sense.</p>


<p>However, in my opinion, the initial setup seems a little bit not obvious for people who just want to start android development (you have to download the sdk tools, and then, find in Android Studio where to set that path to the sdk, knowing that reopening the same window will keep you previous path only you have one sdk version installed from those sdk tools).</p>


<p>It seems as well that there is no way to set that up from the command line or configuration file without listing installed sdk versions, which will then not be a robust way for the ubuntu developer tools center to do it for you.</p>


<p>So, for now, we are following the upstream (I think they have a plan for better integration in the future) way of letting the user downloading the sdk, I will be interested in getting feedback from new users on how it went for them on that step.</p>


<h3>Availability</h3>

<p>0.1.1 is now available to the regular channels:
- in vivid
- in the <a href="https://launchpad.net/~didrocks/+archive/ubuntu/ubuntu-developer-tools-center">ubuntu-developer-tools-center</a> ppa for Ubuntu 14.04 LTS and 14.10.</p>