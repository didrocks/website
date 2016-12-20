+++
title = "Unity 5.2 is now released!"
date = "2012-02-03T11:50:00+01:00"
tags = [ "libre", "PU", "ubuntu", "unity" ]
aliases = ["/post/Unity-5.2-is-now-released%21"]
+++
    <p>Phew! It's been a crazy ride to release Unity 5.2 once ubuntu precise released its alpha 2, but we finally get there!</p>


<p>Thanks a lot for all the community participation, we actually got 27 testers answering to Nick's <a href="http://www.theorangenotebook.com/2012/01/unity-52-whats-new-and-call-for-testing.html">call for testing</a>. Those were high quality contributions and enabled us to get closer to the unity release.</p>


<p>So, what's new since 5.0? Well, a lot! :) More precisely, we got multimonitor support with screen edge detection, "push to reveal" launcher behavior to avoid false positive when hitting the back button of firefox, per workspace alt-tab switcher, new home dash, automaximize only on netbooks and a lot of small details that matter.</p>


<h2>Test results</h2>


<p>Here are some feedback after 5 hours that I took to collect and analyze the test results from all the (numerous) comments that were on the test results:</p>

<ul>
<li>Testers confirmed that some of the issues spotted on 5.0 are now fixed, which is a great news! Not all of them, and of course, we have some minor regressions. I added those issues to the list of "distro priorities". You can look at them <a href="http://people.canonical.com/~platform/design/upstream.html">there</a>. This list doesn't show all the defects we have, of course, but give a good overview of the big ones we track to ensure they are fixed as soon as possible.</li>
<li>Some tests have been updated due to new upstream behavior (like the per workspace scale option and new home dash which now retains its search status). Thanks for people testing it and to have spotted that we missed those changes when updating the tests! We also rephrased with the given suggestions some of them.</li>
<li>Some people seem to get difficulties to open menus from the application and the indicator when clicking on them in the panel (only Alt or F10 seems to work). I strongly invite them to open a bug repot with a video attached and giving more info as I couldn't reproduce it there.</li>
<li>There is a bamf bug revealing only on some particular circumstances (8 fails, and last time, we also get some failures on this test) when testing launcher/quicklist-pin. I personnaly couldn't reproduce it here. Then, I asked seb128 to give it a shot and he could get the issue. I tried again and this time, I got it! However, this seems to not be reliable or reproducible 100%. We opened a bug and put it on the priority list. Well spotted everyone! :)</li>
<li>Also some testers made some interesting design request, I'm reminding you of <a href="http://unity.ubuntu.com/getinvolved/#design">this link</a> on how to join the relevant mailing list to participate in unity design (the introduction text stated it though ;)).</li>
<li>We got also some comments of "key above tab" and why we used this terminology rather than directly telling, let's say "`". Please remember that this is a keyboard dependent configuration! The usa keyboard is normally using `, my azerty keyboard is using ², it seems that for some other configuration it's &lt; or ~. So yeah, we have to keep the test cases as generic as possible, bare with us, please! :)</li>
<li>We added some new test cases as well due to a very particular way of triggering some bugs like for instance <a href="https://bugs.launchpad.net/unity-distro-priority/+bug/877778">bug #877778</a>. Thanks to the one adding a comment to explain how to trigger it!</li>
<li>Despite our strong efforts to make an easy way for unity restarting on a simple click from the tests (and improving it), it seems that the glibmm/compiz bug preventing to restart it reliably on demand is still an issue. It's not a very important bug for everyday use, however, it will be nice that we can get it over for the tests in particular. Fortunately, checkbox enables you to continue the tests where you stopped even if you had to restart your session.</li>
</ul>

<h2>A story of boot time</h2>

<p>Finally and probably the most important feedback from the whole list, peope started to feel that "it was longer to start/boot". Jumping on this fact, we made some bootcharts on our machines to get real and precise values and you know what… the comments were right! The multimonitor support made the boot time badly regressing. Consequently, we decided to delay the release until today to get that fixed rather than pushing a version with this performance impact on intel cards. We finally got the fix, push it to trunk and now, this is all old story! :) Thanks to all the community for spotting this one, it's better to remark it earlier than later and this participation really had a visible impact (or rather avoided some real visible impact ;)) for a bunch of users. Well done!</p>


<h2>The importance of testing defaults</h2>

<p>Some testers remarked that in the system settings test, we never told to add gnome control center to the launcher. However, in the introduction text, we clearly expressed that we expect testers having the default settings (you have the guest session for it, use it, love it!) and the system settings is by default pinned in the launcher. :) For instance, intellihide is the default behavior and we didn't say anything to ensure that intellihide is there. If we did it, there will be a long list of prerequesites on the top of each test that I'm sure testers don't want to see? ;) We strongly recommend people using the guest session to ensure all settings and environment are correct for the tests!</p>


<h2>To sum up</h2>

<p>Unity 5.2 is now building in the official repositories and should soon be available to all precise users. Thanks again to everyone participating in this project and see you soon for… 5.4 (or maybe a little bit before for an incoming compiz release that I heard of)&nbsp;! :)</p>