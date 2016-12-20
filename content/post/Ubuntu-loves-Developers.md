+++
title = "Ubuntu loves Developers"
date = "2014-09-02T14:30:00+02:00"
tags = [ "android", "devel", "libre", "programmation", "PU", "ubuntu" ]
aliases = ["/post/Ubuntu-loves-Developers"]
+++
    <p>Ubuntu is one of the best Linux platforms with an awesome desktop for regular users (and soon phone and tablets and more!) and great servers for system administrators and devops. A number of developers are choosing Ubuntu as their primary development system of choice, even if they develop for platforms other than Ubuntu itself, like doing some Android development, web development and so on.</p>


<p><img src="/public/ubuntu/uld/uld.png" alt="uld.png" style="display:block; margin:0 auto;" title="Ubuntu Loves Devs" /></p>


<p>However, even if we fill the basic needs for this audience, we decided a few months ago to start a development and integration effort to make those users completely feel at home. Ubuntu loves developers and we are going to showcase it by making Ubuntu the best available developer platform!</p>



<h3>Sounds great! What's up then?</h3>


<p>We decided to start by concentrating on Android developers. We'll ramp up afterwards on other use cases like Go developers, web developers, Dart… but we want to ensure we deliver a stunning experience for each targeted audience before moving on to the next topic.</p>


<p>After analyzing how to setup an Android development machine on Ubuntu we realized that, depending on the system, it can takes up to 9 different steps to get proper IDE integration and all the dependencies installed. The whole goal was to reduce that to one single command!</p>


<p>Concretely speaking, we created the <a href="https://launchpad.net/ubuntu-developer-tools-center">Ubuntu Developer Tools Center</a>, a command line tool which allows you to download the latest version of <a href="/post/[https://developer.android.com/sdk/installing/studio.html">Android Studio (beta)</a>, alongside the latest Android SDK, and all the required dependencies (which will only ask for sudo access if you don't have all the required dependencies installed already), enable multi-arch on your system if you are on a 64 bit machine, integrate it with the Unity launcher…</p>


<p><img src="/public/ubuntu/uld/launcher-integration.png" alt="launcher-integration.png" style="display:block; margin:0 auto;" title="Launcher integration for developer tools" /></p>


<p>As said, we focused on Android Studio (based itself on <a href="http://www.jetbrains.com/idea/">Intellij IDEA</a>) for now as it seems that’s where Google has been focusing its Android tools development effort for over a year. However, the system is not restrictive and it will be relatively trivial in the near future to add <a href="http://developer.android.com/tools/sdk/eclipse-adt.html">ADT support</a> (Android Development Tools using Eclipse)<sup>[<a href="#pnote-196-1" id="rev-pnote-196-1">1</a>]</sup>.<p>


<p><a href="/public/ubuntu/uld/android-studio.png" title="android-studio.png"><img src="/public/ubuntu/uld/.android-studio_m.jpg" alt="android-studio.png" style="display:block; margin:0 auto;" title="Android Studio" /></a></p>


<p>Indeed, The Ubuntu Developer Tools Center is targeted as being a real platform for all developer users on Ubuntu. We carefully implemented the base platform with a strong technical foundation, so that it's easily extensible and some features like the advanced bash shell completion will even make more sense once we added other development tools support.</p>


<h3>Availability</h3>


<p>We will always target first the latest Ubuntu LTS version alongside the latest version in development. Yes! It means that people who want to benefit for the extensively tested and strong base experience that a Long Term Support version offers will always be up to date on their favorite developer tools and be first-class citizen. We strongly believe that developers always want the latest available tools on a strong and solid stable base and this is one of the core principle we are focusing on.</p>


<p>For now, the LTS support is through our <a href="https://launchpad.net/~didrocks/+archive/ubuntu/ubuntu-developer-tools-center">official Ubuntu Developer Tools Center ppa</a>, but we plan to move that to the <a href="https://help.ubuntu.com/community/UbuntuBackports">backports archive</a> with all the newly or updated libraries. For Utopic, it's already available in the <a href="https://launchpad.net/ubuntu/utopic/+source/ubuntu-developer-tools-center">14.10 Ubuntu archive</a>.</p>


<h3>Initial available version</h3>


<p>Be aware that the Ubuntu Developer Tools Center is currently in alpha. This tool will evolve depending on your feedback, so it's up to you to suggest the direction you want it to go! A blog post on how to contribute will follow in the next days. This initial version is available in English, French and Chinese!</p>


<p>Another blog post will expand as well how we test this tool. For now, just be aware that the extensive test suite is running daily, and it ensures that on all supported platforms we don't break, that the Ubuntu platform itself doesn't break us, or that any 3rd party on which we rely on (like website links and so on) don't change without us spotting it. This will ensure that our tools is always working, with limited downtime.</p>


<h3>Example: how to install Ubuntu Developer tools and then, Android Studio</h3>


<h4>Ubuntu Developer Tools Center</h4>


<p>If you are on Ubuntu 14.04 LTS, first, add the UDTC ppa:</p>


<blockquote><p>$ sudo add-apt-repository ppa:didrocks/ubuntu-developer-tools-center
$ sudo apt-get update</p></blockquote>


<p>Then, installing UDTC:</p>

<blockquote><p>$ sudo apt-get install ubuntu-developer-tools-center</p></blockquote>


<h4>How to install android-studio</h4>

<p>Simply executes<sup>[<a href="#pnote-196-2" id="rev-pnote-196-2">2</a>]</sup>:<p>


<blockquote><p>$ udtc android</p></blockquote>


<p>And then, accept the installation path and Google license. It will download, install all requirements alongside Android Studio and latest android SDK itself, then configure and fit it into the system like by adding an Unity launcher icon…</p>


<p>And that's it! Happy Android application hacking on Ubuntu. You will find the familiar experience with the android emulator and sdk manager + auto-updater to always be on the latest. ;)</p>


<p><a href="/public/ubuntu/uld/android-sdk-tools.png" title="android-sdk-tools.png"><img src="/public/ubuntu/uld/.android-sdk-tools_m.jpg" alt="android-sdk-tools.png" style="display:block; margin:0 auto;" title="Android SDK tools on Ubuntu" /></a></p>


<h3>Feedback</h3>


<p>We welcome any ideas and feedback, as well as contributions as we'll discuss more in the next post. Meanwhile, do not hesitate to reach me on IRC (didrocks on freenode, #ubuntu-desktop as the primary channel to discuss), or on <a href="https://plus.google.com/+DidierRoche">Google+</a>. You can as well open bugs on the <a href="https://bugs.launchpad.net/ubuntu/+source/ubuntu-developer-tools-center">launchpad project</a> or <a href="https://github.com/didrocks/ubuntu-developer-tools-center/issues">github one</a></p>


<p>I'm excited about this opportunity to work on the developer desktop. Ubuntu loves Developers, and it's all on us to create a strong developer community so that we can really make Ubuntu, the developer-friendly platform of choice!</p>
<div class="footnotes"><h4 class="footnotes-title">Notes</h4>
<p>[<a href="#L58">less than 60 lines</a><p>
<p>[<a href="#rev-pnote-196-2" id="pnote-196-2">2</a>] android-studio is the default for the android development platform, you can choose it explicitely by executing "$ udtc android android-studio". Refer to --help or use the bash completion for more help and hints</p><div>
