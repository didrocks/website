+++
title = "Netbeans and Rust support in Ubuntu Make 15.11"
date = "2015-11-10T17:00:00+01:00"
tags = [ "devel", "libre", "programmation", "PU", "quality", "ubuntu-make", "ubuntulovesdevs" ]
aliases = ["/post/Netbeans-and-Rust-support-in-Ubuntu-Make-15.11"]
+++
    <p>After some releases bringing updates, bug fixes, refactoring, tests improvements and more minor features and automations, here is time again for a noticeable feature release!</p>


<p>Thanks to <a href="https://plus.google.com/+FabioColella/posts">Fabio Colella</a>, we now have <a href="https://netbeans.org">NetBeans</a> support in <a href="https://github.com/ubuntu/ubuntu-make">Ubuntu Make</a>! Installing it is just a <code>umake ide netbeans</code> away and just relax while Ubuntu Make is doing the hard work so that you can enjoy this IDE.</p>


<p><a href="/public/ubuntu/uld/netbeans.png" title="netbeans.png"><img src="/public/ubuntu/uld/.netbeans_m.png" alt="netbeans.png" style="display:block; margin:0 auto;" title="netbeans.png, nov. 2015" /></a></p>


<p>Another new feature is the Rust support by <a href="https://twitter.com/yoink1">Jared Ravetch</a>. <code>umake rust</code> will do all the necessary steps so that you get a good rust developing experience on your favorite ubuntu distro!</p>


<p><a href="https://github.com/eldarkg">Eldar Khayrullin</a> (welcome to him for his first contribution!) updated the <a href="https://unity3d.com/">Unity 3D game engine</a> support to point to the latest beta released version and <a href="https://github.com/sschuberth">Sebastian Schuberth</a> fixed an android NDK environment variable to use a more widespread one.</p>


<p>Other noticeable changes, following upstream webstorm IDE, are update to get their latest available icons (thanks to our test granularity level, we were able to detect this small change!), fixes for the version option, global -r working as the new global --remove, some fixes for zsh users, and as well a bunch of new translations thanks to our awesome translator community (new languages: fa, pt_BR and updated de, en_AU, en_CA, en_GB, eu, hr, it, pl, ru, te, zh_CN, zh_HK). There is of course more refactoring and other tests changes. Full glory details are <a href="https://github.com/ubuntu/ubuntu-make/commit/0dee421841c7fca6404bb1796c616c9915ea1133">available here</a>.</p>


<p>As usual, all of those modifications and new features are backed up via a number of small, medium and large tests! We are currently running about <a href="https://jenkins.qa.ubuntu.com/job/udtc-trusty-tests/2246/">850 tests in our jenkins infrastructure</a> (running all the tests). All commits and pull requests are tested for pep8 and small tests using <a href="https://travis-ci.org/ubuntu/ubuntu-make">Travis CI</a> and the health status is of course reported in the <a href="https://github.com/ubuntu/ubuntu-make">README.md</a> file.</p>


<p>As usual, you can get this latest version direcly through <a href="https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/ubuntu-make">its ppa</a> for the 14.04 LTS, 15.05 and 15.10 ubuntu releases. Xenial version is available directly in the <a href="https://launchpad.net/ubuntu/+source/ubuntu-make/15.11.1">xenial ubuntu archive</a>. Thanks again to all our awesome contributors community! A lot more is still in the pipe, but that will be for next release!</p>


<p>Our <a href="https://github.com/ubuntu/ubuntu-make/issues">issue tracker</a> is full of ideas and opportunities, and pull requests remain opened for any issues or suggestions! If you want to be the next featured contributor and want to give an hand, you can refer to <a href="/post/How-to-help-on-Ubuntu-Developer-Tools-Center">this post</a> with useful links!</p>