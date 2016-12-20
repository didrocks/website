+++
title = "Bringing appmenu support for java application and Ubuntu Make 0.4.1 with an Intellij IDEA fix"
date = "2015-01-22T09:52:00+01:00"
tags = [ "devel", "libre", "programmation", "PU", "ubuntu", "ubuntu-make", "ubuntulovesdevs", "unity" ]
aliases = ["/post/Bringing-appmenu-support-for-java-application-and-Ubuntu-Make-0.4.1-with-an-Intellij-IDEA-fix"]
+++
    <p>Today we released <a href="https://github.com/ubuntu/ubuntu-make/commit/17d8acbaf596de07d913db322dcecbd719faf4b8">Ubuntu Make 0.4.1</a> which validates the application menu support for some java application using swing (like Intellij, Android Studio…) and fixes <a href="https://www.jetbrains.com/idea/">Intellij IDEA</a> support.</p>


<p>Vertical screen estate is particularly valuable for developers, to maximize the place where you can visualize your code and not bother too much about the shell itself. Also, in complex menu structure, it can be hard to find the relevant items. Unity <a href="http://design.canonical.com/2010/05/menu-bar/">introduced a while back</a> (2010!) the excellent application menu and then grows the <a href="http://www.markshuttleworth.com/archives/939">HUD support</a> to search through those menus. We even got recently <a href="http://arstechnica.com/information-technology/2014/02/ubuntu-desktop-moving-application-menus-back-into-application-windows/">new options</a> for menu integration without renouncing on those principles. However, until now, some java-based IDEs didn't get default appmenu and HUD support. It was time to get that fixed with our <a href="/post/Ubuntu-loves-Developers">Ubuntu Loves Developers</a> focus!</p>


<p><a href="/public/ubuntu/uld/jayatana.png" title="Appmenu support in intellij IDEA"><img src="/public/ubuntu/uld/.jayatana_m.jpg" alt="Appmenu support in intellij IDEA" style="display:block; margin:0 auto;" title="Appmenu support in intellij IDEA, janv. 2015" /></a></p>


<p>The application menu support is <a href="https://launchpad.net/ubuntu/+source/indicator-appmenu/13.01.0+15.04.20150114-0ubuntu1">installed by default</a> on Ubuntu Vivid thanks to our work with <a href="http://www.webupd8.org/2014/02/get-unity-global-menu-hud-support-for.html">jayatana</a>'s excellent contributor <a href="https://launchpad.net/~danjaredg">Dan Jared</a>! We did some cleaning and worked with him to get jayatana into ubuntu vivid, and then, promote it on the Ubuntu Desktop image<sup>[<a href="#pnote-205-1" id="rev-pnote-205-1">1</a>]</sup>. On older releases, we pushed jayatana into the <a href="https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/ubuntu-make">Ubuntu Make ppa</a> and every new install through that tool will install as well this support as needed.<p>


<p>We also saw <a href="https://www.jetbrains.com/">jetbrains</a> changing their download page structure, making Intellij IDEA not being installable anymore. Less than 24 hours after <a href="https://github.com/ubuntu/ubuntu-make/issues/67">a bug report being opened</a>, we got this new 0.4.1 release including <a href="https://github.com/ubuntu/ubuntu-make/pull/68">a fix</a> from Intellij IDEA support contributor to Ubuntu Make, <a href="https://github.com/Tinche">Tin Tvrtković</a>. Big kudos to him for the prompt reaction! The tests have also been updated to reflect this new page structure.</p>


<p>Those nice goodies and fixes are <a href="https://launchpad.net/ubuntu/+source/ubuntu-make/0.4.1">available on ubuntu vivid</a> (ensure you install Ubuntu Make 0.4.1), and as well, through <a href="/post/">its ppa</a> for 14.04 LTS and 14.10 ubuntu releases. Keep in mind that you need to restart your session once jayatana has been installed to get the menu export to Unity available.</p>


<p>Another release demonstrating how the Ubuntu and Ubuntu Make community really work well together, get things quickly under control and shine! If you want to help out defining and contributing in making Ubuntu the best platform for developers, please <a href="/post/How-to-help-on-Ubuntu-Developer-Tools-Center">head here</a>!</p>
<div class="footnotes"><h4 class="footnotes-title">Note</h4>
<p>[<a href="#rev-pnote-205-1" id="pnote-205-1">1</a>] which won't install java on the image by default, don't be scared ;)</p><div>
