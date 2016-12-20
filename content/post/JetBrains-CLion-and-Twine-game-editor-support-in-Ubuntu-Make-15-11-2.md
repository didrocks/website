+++
title = "JetBrains CLion and Twine game editor support in Ubuntu Make 15.11.2"
date = "2015-11-24T16:30:00+01:00"
tags = [ "devel", "libre", "programmation", "PU", "ubuntu", "ubuntu-make", "ubuntulovesdevs" ]
aliases = ["/post/JetBrains-CLion-and-Twine-game-editor-support-in-Ubuntu-Make-15.11.2"]
+++
    <p>It's already time for a third release of <a href="https://wiki.ubuntu.com/ubuntu-make">Ubuntu Make</a> this month! Thanks to the help of existing and new contributors, here are what's noticeable on this release.</p>


<p><a href="https://www.jetbrains.com/clion/">JetBrains' excellent C/C++ IDE named CLion</a> is now available! A simple <code>umake ide clion</code> will get it you at your disposal!</p>


<p><a href="/public/ubuntu/uld/clion.png" title="clion.png"><img src="/public/ubuntu/uld/.clion_m.png" alt="clion.png" style="display:block; margin:0 auto;" title="clion.png, nov. 2015" /></a></p>


<p>The <a href="http://twinery.org/">non linear game editor Twine</a> (that our community team is using as well for other QA purposes) also entered this release and is just a <code>umake games twine</code> away!</p>


<p><a href="/public/ubuntu/uld/twine.png" title="twine.png"><img src="/public/ubuntu/uld/.twine_m.png" alt="twine.png" style="display:block; margin:0 auto;" title="twine.png, nov. 2015" /></a></p>


<p>Our ZSH users will be pleased to know that the advanced shell completion that we have in bash is now available to them. We refreshed and fixed some translations, especially in russian, portuguese and french for this release. A <a href="https://translations.launchpad.net/ubuntu-make">lot of opportunities in term of translations</a> are available! Do not hesitate to jump in. :)</p>


<p>A bunch of work on tests and the <a href="https://jenkins.qa.ubuntu.com/job/udtc-trusty-tests/">testing infrastructure</a> (cutting the testing time approximately by half!) have been done. Speaking of tests, we spotted and fixed the upstream  renamed icon in Visual Studio Code thanks to one of them failing (nice to be at that level of quality granularity)! We also worked on ensuring that people using <a href="https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/ubuntu-make">our PPA</a> with previous ubuntu releases only download the minimal requirements and not our testing dependency (by shifting to <a href="https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/ubuntu-make-builddeps">another ppa</a> only containing them). Of course, the contributor guide has been updated for matching all of this.</p>


<p>You will thus understand that we got a lot of other small fixes and enhancements with this new package. If you want to read the full and detailed list of what's in this release, please <a href="https://github.com/ubuntu/ubuntu-make/commit/1469477890a10eba2b17d74d461c22567c24838c">have a read here</a>!</p>


<p>As usual, you can get this latest version direcly through <a href="https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/ubuntu-make">its ppa</a> for the 14.04 LTS, 15.04 and 15.10 ubuntu releases. Xenial version is available directly in the <a href="https://launchpad.net/ubuntu/+source/ubuntu-make/15.11.2">xenial ubuntu archive</a>. This wouldn't be possible withoutl our awesome contributors community, thanks to them again!</p>


<p>Our <a href="https://github.com/ubuntu/ubuntu-make/issues">issue tracker</a> is full of ideas and opportunities, and pull requests remain opened for any issues or suggestions! If you want to be the next featured contributor and want to give an hand, you can refer to <a href="/post/How-to-help-on-Ubuntu-Developer-Tools-Center">this post</a> with useful links!</p>