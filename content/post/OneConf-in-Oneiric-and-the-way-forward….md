+++
title = "OneConf in Oneiric and the way forward…"
date = "2011-10-05T14:40:00+02:00"
tags = [ "devel", "libre", "PU", "ubuntu", "uds" ]
aliases = ["/post/OneConf-in-Oneiric-and-the-way-forward%E2%80%A6"]
+++
    <h2>A little bit of retrospective</h2>


<p>OneConf is a pet project I'm trying to push since <a href="https://blueprints.launchpad.net/ubuntu/+spec/desktop-maverick-oneconf">UDS Barcelona</a>. The full idea is originally described <a href="https://wiki.ubuntu.com/OneConf">on this wiki page</a>: <q>OneConf is a mechanism for recording software information in Ubuntu One, and synchronizing with other computers as needed. In Maverick, the list of installed software is stored. This may eventually expand to include some application settings and application state. Other tools like Stipple can provide more advanced settings/control.</q>.</p>


<p>Well, the definition today is quite different as you will see below.</p>


<p>Since then, a lot of other projects like unity took my time off and I couldn't really concentrate in this pet project. But now, even with some small late churn due to software-center moving to gtk3, OneConf is installed by default in Oneiric and syncing is active since beta2! It doesn't depend anymore on <a href="https://one.ubuntu.com/">Ubuntu One</a> but using its own infrastructure (linked to ubuntu sso, so a launchpad account, a ratings and review account, or still an Ubuntu One account is working with it). The sync is done linked to the new <a href="https://apps.ubuntu.com/">software-center webui</a> infrastructure.</p>


<p>I'm really happy I've been able to take enough time to bring the needed polish so that I'm confident with OneConf installed by default right now on your Oneiric machine.</p>


<h2>Usage</h2>


<p>You can activate it in software-center by clicking on File -&gt; Sync between computers… You will get the first time an ubuntu sso dialog asking for your crendential if you never reviewed any application in software-center (OneConf is using the same credential). Then, you are directed to the "Installed view" which is tweaked to show OneConf hosts.</p>


<p><a href="/public/projects/oneconf/Screenshot_at_2011-10-05_13_25_39.png" title="OneConf Software-Center Oneiric"><img src="/public/projects/oneconf/.Screenshot_at_2011-10-05_13_25_39_m.jpg" alt="OneConf Software-Center Oneiric" style="display:block; margin:0 auto;" title="OneConf Software-Center Oneiric, oct. 2011" /></a></p>


<p>The first item "This computer (name)" will show the traditional Installed view from software-center. What is interesting is when you start setting up more than one computer in software-center to share your inventory. Those will show up in the view, and selecting one of them will show the difference between installed applications (and technical items) on the current computer and the remote one. Selecting the different elements enables you to install/uninstalled applications as you are traditionnaly able to in software-center.</p>


<p>Syncing is done regularly in the background as soon as you start sharing your inventory and you can see the last time the computer was synced. All data (basically the package list) on the syncing infrastructure are personals and linked to your unique sso id.</p>


<p>Note that the applications and package list is refreshed with any kind of installation method you are using: software-center of course, but as well, apt, synaptic, aptitude… PPAs application also will show up if the ppa is installed on the local computer.</p>


<p>You can as well stop sharing the current computer in the UI, and you will see it synced shortly to others.</p>



<h2>That's cool, but can I have more?</h2>


<p>Now is the right time to wonder what can be done for the next LTS.</p>


<h3>Improving what we already have</h3>

<p>Finishing plumbing the testsuite and some tweaks to the software-center ui is probably the way to go. In particular a way to "install all"/"remove all" buttons in the UI to just sync the same packages in both computer. I would love as well bringing back search as we had in the gtk2 version. Small nice speed enhancement opportunities are also totally achievable.</p>


<p>Finally, having the wallpaper as preview on the host icon is totally doable (in fact, it's already there, but not synced) but need some syncing infrastructure discussion.</p>


<h3>Ubiquity integration</h3>

<p>Wouldn't it be cool to be able to install a new computer and just tell "please, install it like &lt;list of computer&gt;". We need as well a checkbox to tell "this is the new foo" (if you are reinstalling foo). As we have the information that you installed an application on purpose (manually installed) or as only a dependency without doing that on your own, we can have an accurate list of what need to be installed.</p>


<p>Ubiquity integration will probably give us an unique feature and push the concept further. It only needs some UI integration work that can be done for next LTS.</p>


<h3>WebUI</h3>

<p>This one can be particularly enjoyable. Now that we have the really nice <a href="http://apps.ubuntu.com">http://apps.ubuntu.com website</a> and knowing that our package list is stored there, can we make both of them communicate? What if I log in with my sso account on this online software center, then see my computer list, select one, and look at what I have installed on this computer? What if I can add some pending actions, like "install this, remove that…". Finally, once I'm back on my machine and launching software-center again, I get a dialog "you have xx pending actions", do you want to complete them now?</p>


<p>I don't think we should do that automagically but rather rely on a confirmation in the software-center ui side as this requires root permissions, and can impact the current host: hence this software-center additional step. We should give a clear UI about what will happen on your computer if you acknowledge the change, let a way to remove pending actions (and then, sync that back in the webui), or just dismiss them for a later ack (like, I'm on battery right now).</p>


<p>This feature requires a lot of cross-team work (design, isd, platform), but I am really excited about it now that we have all the ground layers now ready and hope to be able to start working on it!</p>


<h3>Hosts syncing</h3>

<p>Another idea, linked to previous one, is about hosts syncing. What if I can say "foo and bar should have the same applications installed" (because they are two desktop stations), and "tidus, yuna and rikku are 3 netbooks and should be synced as well" (with a different application list as it's a different usage).</p>


<p>This one has challenging engineering question as, for the same reason as before, I don't think we should apply the changes without user confirmation and dismiss possbility. Adding than 3 PC and the previous "webui" idea as another source of changes and it results in very interesting cases, just of one of them is:</p>
<ul>
<li>yuna added "foo"</li>
<li>tidus had "foo" previously and just removed it</li>
<li>the webui has a request to remove "foo"</li>
</ul>

<p>-&gt; which one should win? We can work on timetamp to estimate what's the last action was and this one wins. But in this case, if removing "foo" was the last action and I dismiss it on "yuna" when seeing it wants to remove it, what should happen? installling "foo" on tidus and select it on the webui as it was the last action trigger by "no please, don't remove foo on yuna"? If not, the other hosts won't be in sync anymore?</p>


<p>I have thought about tons of cases like that and we should carefully design what to do in every one of them to present the options in a clear and not confusing way to the user.</p>


<p>Those "sets of installation" can also brings a very interesting and handy feature I've never seen anywhere before (but maybe I'm wrong and it already exists). From an engineering point, it can become quite challenging to get it done in a reliable and efficient way. All this complexity should be hidden to get a clean, simple, efficient UI to our user base.</p>


<h3>Recommendations</h3>

<p>With the data in the webui, we can have better recommendations in software-center, like "people installing the gimp also installs inkscape". All this needs to be anonymous of course as it is on popular website already. Can be a very nice way to discover new applications.</p>


<h3>User system settings syncing</h3>

<p>For this one, I would think we should go using <a href="https://one.ubuntu.com/">Ubuntu One</a>. This was part of the initial spec in May 2010 (see the <a href="https://wiki.ubuntu.com/OneConf">wiki link</a>): having your wallpaper automatically synced between computers, some credentials to not have to configure again your system and other dedicated system settings (like the theme choice, the suspend after &lt;x&gt; min configuration…). We need to provide a way to select what to sync and having a set of sensible defaults. We don't want to sync everything, my power policy is different on my laptop and my desktop, but targeted options picked with care can really enhance the ubuntu experience.</p>


<p>Note that applications settings syncing like thunderbird are a different topic to this one which target system user info and choice.</p>


<h2>Wrap up</h2>


<p>This is only few ideas on what can be done with some cycle works (obviously, we can't target everything for P…) to make our ubuntu experience better and (I hope) innovative. There will surely be some UDS discussions about it and I hope to see a lot of people coming there! And youn, what do you think? Do not hesitate to share any of your crazy ideas and come participating to this effort, every single help is welcomed. :)</p>


<h2>Thanks</h2>

<p>I want to thank everyone that helped in any way pushing that to Oneiric, and in particular, tremolux and mvo from the software-center (and motivation) side, mpt for the OneConf redesign in the new software-center and achuni for a wonderful job on the isd side. I'm really happy and thanksfull to have so many awesome coworkers!</p>