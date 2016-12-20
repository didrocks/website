+++
title = "Arduino support and various fixes in Ubuntu Make 0.9"
date = "2015-07-21T11:17:00+02:00"
tags = [ "devel", "libre", "programmation", "PU", "ubuntu", "ubuntu-make", "ubuntulovesdevs" ]
aliases = ["/post/Arduino-support-and-various-fix-in-Ubuntu-Make-0.9"]
+++
    <p>A warm summer has started in some part of the world and holidays: beach and enjoying various refreshements!</p>


<p>However, the unstoppable <a href="http://wiki.ubuntu.com/ubuntu-make">Ubuntu Make</a> team wasn't on a pause and we continued making improvements thanks to the vibrant community around it!</p>


<p>What's new in this release? First Arduino support has been added with the vast majority of work done by Tin Tvrtković. Thanks to him for this excellent work! To be able to install arduino IDE, just run:</p>


<p><code>$ umake ide arduino</code></p>


<p>Note that your user will eventually be added to the right unix group if it was not already in. In that case, it will ask you to login back to be able to communicate with your arduino device. Then, you can enjoy the arduino IDE:</p>


<p><a href="/public/ubuntu/uld/arduino.png" title="Arduino IDE"><img src="/public/ubuntu/uld/.arduino_m.jpg" alt="Arduino IDE" style="display:block; margin:0 auto;" title="Arduino IDE, juil. 2015" /></a></p>


<p>Some other hilights from this release is the deprecation of the Dart Editor framework and replacement by Dart SDK one. As of Dartlang 1.11, the Dart Editor <a href="http://news.dartlang.org/2015/04/the-present-and-future-of-editors-and.html">isn't supported and bundled anymore</a> (it still exists as an independent eclipse plugin though). We thus marked the Dart Editor framework for removal only and added this Dart SDK (adding the SDK to the user's PATH) instead. This is the new default for the Dart category.</p>


<p>Thanks to our extensive tests, we saw that the <a href="https://jenkins.qa.ubuntu.com/job/udtc-trusty-tests/1491/label=ps-trusty-desktop-i386-1,type=large/testReport/tests.large.test_web/VisualStudioCodeTest/test_default_install/">32 bits of Visual Studio Code page changed</a> and wasn't thus installable anymore. It's as fixed as of this release.</p>


<p>A lot of other improvements (in particular in the tests writing infra and other minor fixes) are also part of 0.9. A more detailed changelog is <a href="https://github.com/ubuntu/ubuntu-make/commit/fc848aebf1278f1052f8843392cb0674617afad3">available here</a>.</p>


<p>0.9 is already <a href="https://launchpad.net/ubuntu/+source/ubuntu-make/0.9.0">available in Wily</a>, and through <a href="https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/ubuntu-make">its ppa</a>, for 14.04 LTS and 15.04 ubuntu releases! Get it while it's <del>warm</del>hot!</p>