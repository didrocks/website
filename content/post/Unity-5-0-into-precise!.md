+++
title = "Unity 5.0 into precise!"
date = "2012-01-13T17:56:00+01:00"
tags = [ "libre", "PU", "ubuntu" ]
aliases = ["/post/Unity-5.0-into-precise%21"]
+++
    <p>Now that the Canonical rally is ending, I'm happy to announce that we released and uploaded <a href="https://launchpad.net/unity/5.0/5.0.0">Unity 5.0</a> to precise!</p>


<p>This is by far the most exciting and the best Unity released we ever had, and I'm happy that we've be able to accomplish all of this thanks to the community and the excellent developer team. We received a lot of testing and got more than 50 test results from my previous <a href="/post/Releasing-a-precise-Unity-5.0-to-Ubuntu-12.04">call for testing</a>. Well done everyone! For some stat fans: 22 packages have been built for this release, and it's only the start! If you missed this test call, don't worry, next unity release is coming in a couple of weeks and you will surely hear about it! :)</p>


<p>Now, I think it would be useful to share the notes I've taken from the test results we collected. Here are some highlights on what seems to be relevant in my opinions:</p>


<p>We now have a tool to get the user test results from the result-tracker (posted by the hardware database). You can find it there: lp:~didrocks/+junk/resulttrackercollect (it only shows relevant data, not all tests that passed without any comment).</p>


<h2>For upstream code:</h2>
<ol>
<li>the wrapper we wrote using the "unity" binary to make restarting unity easy for testers didn't work quite well: a lot of people got compiz hanging (seen as well on the forum). This is mostly due to compiz randomly hanging when using compiz --replace. Proposed solution: add in the unity tool a "killall compiz" before restarting it.</li>
<li>when restoring a semi-maximized application, the window placement is not the expected one (shifted compared to the cursor position). This is not a regression.</li>
<li>the "most used app entry" is not available to some users, we experienced that ourselves as well and had to kill zeitgeist-daemon (mhr3 was looking on the system but has no clue on what it can be). This should be looked seriously.</li>
<li>8 people didn't get zeitgeist returning the newly created "foo" file in dash/global-search-files-apps. as in the previous point, mhr3 looked at it and restarting zg fixed it. There is something to investigate there.</li>
<li>each time you click on the minimize button in the panel, dash opens, it becomes brighter (not a regression IIRC)</li>
<li>Multiple complains that the music lens doesn't show all the banshee library content.</li>
<li>2 people reported a failed launcher/quicklist-pin telling that running application icons aren't in the launcher anymore. Seems a bamfdaemon issue.</li>
<li>alt + F1 enters the keyboard navigation mode, but the second alt + F1 doesn't exit it. This is a regression from oneiric.</li>
<li>when you put the mouse under the launcher area, launcher hidden, press super, release it, the launcher stays displayed when it should hidden because you didn't move the mouse (design request, was working on Natty, is failing since Oneiric. This is a nux event issue.)</li>
<li>there is multiple reports about dragging icons in the launcher that are frozen and you have to initialize a drag again to fix it. I think this is not a regression.</li>
<li>tabs navigate between lenses, which is not what design specified (not a regression)</li>
</ol>

<h2>For designers:</h2>
<ol>
<li>people complained on wm/menu_basic that right clicking in the title bar works and bring a menu, but not when the application is maximized (as the title bar is in the panel). If we don't want to change that design-wise, we need to rephrase the test.</li>
<li>alttab/windows-focus-betweenapps-multiple-one was a design oversight with the new alt-tab, we discussed that with Jason and John and it's getting fixed. Not a regression as well.</li>
</ol>

<h2>We fixed one ourself in our pre-testing and so it's fixed in the release:</h2>
<ol>
<li>dash/show -&gt; All lenses but the first one use the "broken" icon. I fixed it upstream.</li>
</ol>

<h2>Some work on checkbox/checkbox-unity</h2>
<ol>
<li>we need to rephrase launcher/intellihide-clickhold as it puzzled people or see how to make easier for people to trigger it (3 Fails, without any comments, seems user's error)</li>
<li>we removed "blank desktop" from the description on unity/startup after noticing some users were expecting "blank to be totally blank"</li>
<li>we made some checkbox patches (like, when checkbox crashes, the restore dialog asking you if you want to continue is focusing "yes" now instead of "no" (some people cried on the forum because of that! :/). There is one pending on making "escape" not quitting it.</li>
<li>checkbox doesn't work on a guest session due to sudo rights</li>
<li>on wm/auto_maximize, seems that there is wrong fail reports. We think that's because people are starting an application which is not big enough to be maximized (75% of the screen). We will write a test application for the user to just run "test" and size it at a suitable size.</li>
<li>we had more than 30 people starting the test on the french forum. 96 messages were exchanged related to those tests. We had people reporting the result manually on the forum because it seems they couldn't or they were not sured that the report got into result-tracker (there is no clear feedback that your report was successfully reported).</li>
<li>we need to rename in launcher/move-icons "bfb" to "first icon the launcher"</li>
</ol>

<p>So, as a summary, it seems that there is no big blocker there and that can be fixed in the next release. You can now enjoy your new shiny Unity 5.0 on your precise installation! Thanks again to everyone involved in all this hard work put together and all the testing that have been done. You all rock! :)</p>


<h2>On Oneiric?</h2>

<p>Oh, and if you are on Oneiric, there is a new stable release update for <a href="https://launchpad.net/unity/4.0/4.28.0">unity 4.28</a> that will soon reach oneiric-proposed. It comes with a bunch of bug fixes, and even more! :) Do not hesitate to test and report feedbacks following the traditional <a href="https://wiki.ubuntu.com/QATeam/PerformingSRUVerification">SRU procedure</a>.</p>