+++
title = "Eclipse and android adt support now in Ubuntu Developer Tools Center"
date = "2014-10-29T12:20:00+01:00"
tags = [ "android", "devel", "libre", "PU", "quality", "ubuntu", "ubuntulovesdevs", "uds" ]
aliases = ["/post/Eclipse-and-android-adt-support-now-in-Ubuntu-Developer-Tools-Center"]
+++
    <p>Eclipse and Android ADT support now in Ubuntu Developer Tools Center</p>



<p>Now that the excellent <a href="http://www.ubuntu.com/download">Ubuntu 14.10</a> is released, it's time to focus as part of our Ubuntu Loves Developers effort on the Ubuntu Developer Tools Center and cutting a new release, bringing numerous new exciting features and framework support!</p>


<h3>0.1 Release main features</h3>


<h4>Eclipse support</h4>


<p>Eclipse is now part of the Ubuntu Developer Tools Center thanks to the excellent work of <a href="https://github.com/Tinche">Tin Tvrtković</a> who implemented the needed bits to bring that up to our users! He worked on the test bed as well to ensure we'll never break unnoticed! That way, we'll always deliver the latest and best Eclipse story on ubuntu.</p>


<p>To install it, just run:</p>
<pre>
$ udtc ide eclipse
</pre>


<p>and let the system set it up for you!</p>


<p><a href="/public/ubuntu/uld/eclipse.png" title="eclipse.png"><img src="/public/ubuntu/uld/.eclipse_m.jpg" alt="eclipse.png" style="display:block; margin:0 auto;" title="eclipse.png, oct. 2014" /></a></p>



<h4>Android Developer Tools support (with eclipse)</h4>


<p><a href="/post/Ubuntu-loves-Developers">The first release</a> introduced the Android Studio (beta) support, which is the default in UDTC for the Android category. In addition to that, we now complete the support in bringing ADT Eclipse support with this release.</p>


<p><a href="/public/ubuntu/uld/eclipse-adt.png" title="eclipse-adt.png"><img src="/public/ubuntu/uld/.eclipse-adt_m.jpg" alt="eclipse-adt.png" style="display:block; margin:0 auto;" title="eclipse-adt.png, oct. 2014" /></a></p>


<p>It can be simply installed with:</p>
<pre>
$ udtc android eclipse-adt
</pre>


<p>Accept the SDK license like in the android studio case and be done! Note that from now on <a href="https://github.com/didrocks/ubuntu-developer-tools-center/issues/31">as suggested</a> by a contributor, with both Android Studio and Eclipse ADT, we add the android tools like adb, fastboot, ddms to the user PATH.</p>


<p>Ubuntu is now a truly first-class citizen for Android application developers as their platform of choice!</p>


<h4>Removing installed platform</h4>


<p>As per a feature request on the <a href="https://github.com/didrocks/ubuntu-developer-tools-center/issues/14">ubuntu developer tools center issue tracker</a>, it's now really easy to remove any installed platform. Just enter the same command than for installing, and append --remove. For instance:</p>

<pre>
$ udtc android eclipse-adt --remove
Removing Eclipse ADT
Suppression done
</pre>


<h4>Enabling local frameworks</h4>


<p><a href="https://github.com/didrocks/ubuntu-developer-tools-center/issues/29">As requested as well</a> on the issue tracker, users can now provide their own local frameworks, by using either <strong>UDTC_FRAMEWORKS=/path/to/directory</strong> and dropping those frameworks here, or in <strong>~/.udtc/frameworks/</strong>.</p>


<p>On glorious details, duplicated categories and frameworks loading order is the following:</p>
<ol>
<li>UDTC_FRAMEWORKS content</li>
<li>~/.udtc/frameworks/ content</li>
<li>System ones.</li>
</ol>

<p>Note though that duplicate filenames aren't encouraged, but supported. This will help as well testing for large tests with a basic framework for all the install, reinstall, remove and other cases common in all BaseInstaller frameworks.</p>


<h4>Other enhancements from the community</h4>


<p>A lot of typo fixes have been included into that release thanks to the excellent and regular work of <a href="https://github.com/ivuk">Igor Vuk</a>, providing regular fixes! A big up to him :) I want to highlight as well the great contributions that we got in term of translations support. Thanks to everyone who helped providing or updating de, en_AU, en_CA, en_GB, es, eu, fr, hr, it, pl, ru, te, zh_CN, zh_HK support in udtc! We are eager to see what next language will enter into this list. Remember that the guide on how to contribute to Ubuntu Developer Tools Center is <a href="/post/How-to-help-on-Ubuntu-Developer-Tools-Center">available here</a>.</p>


<h3>Exciting! How can I get it?</h3>


<p>The 0.1 release <a href="https://github.com/didrocks/ubuntu-developer-tools-center/commit/0d96aee9d98ca29848ad9b786166512bfee2fdbb">is now tagged</a> and <a href="https://jenkins.qa.ubuntu.com/job/udtc-trusty-tests/363/">all tests are passing</a> (this release brings 70 new tests). It's available directly on <a href="http://www.markshuttleworth.com/archives/1425">vivid</a>.</p>


<p>For 14.04 LTS and 14.10, use the <a href="https://launchpad.net/~didrocks/+archive/ubuntu/ubuntu-developer-tools-center">ubuntu-developer-tools-center ppa</a> where it's already available.</p>


<h3>Contributions</h3>


<p>As you have seen above, we really listen to our community and implement &amp; debate anything coming through. We start as well to see great contributions that we accept and merge in. We are just waiting for yours!</p>


<p>If you want to discuss some ideas or want to give a hand, please refer to this <a href="#ubuntu-desktop on freenode. We'll likely have an opened hangout soon during the upcoming <a href="http://summit.ubuntu.com/uos-1411/">Ubuntu Online Summit</a> as well. More news in the coming days here. :)<p>