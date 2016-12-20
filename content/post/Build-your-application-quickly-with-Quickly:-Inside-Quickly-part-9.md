+++
title = "Build your application quickly with Quickly: Inside Quickly part 9"
date = "2009-09-25T13:15:00+02:00"
tags = [ "devel", "quickly", "ubuntu" ]
aliases = ["/post/Build-your-application-quickly-with-Quickly%3A-Inside-Quickly-part-9"]
+++
    <p>We are now almost ready to land! Here is the last part of this long suit of blog posts about Quickly.</p>


<h2>Packaging your project</h2>


<p>It's the last, but not the least issue when you are writing your software: once your application is functional, you surely want to enable other users to install it. Well, you can give it into a tar.gz and run from a trunk, but what about creating a nice package, containing all dependency information for you<sup>[<a href="#pnote-131-1">1</a>]</sup>? Let's see how to achieve this automagically.<p>


<h3>Build your project locally</h3>


<p>First good idea (even if it's not compulsory) is to try to build it locally. Just issue this command for that:</p>

<pre>
$ quickly package
</pre>


<p>It will creates a debian binary package (deb) from your project. The first time, quickly package creates the packaging too. Before running the package command you can edit the Icon and Category entry of your &lt;project_name&gt;*.desktop.in file.</p>


<p>Note that if you didn't run quickly release, quickly share or quickly change-lp-project you may miss the name, email and some other entries in setup.py file. So, you won't have those metada in your package. You can edit them if you don't want to use any of these commands afterwards. Those changes are not mandatory at all for testing purpose.</p>


<p>Quickly package will retrieve all dependencies on your project for you and then build the package locally. You will get a .deb file in the upper folder of your project directory and can install it for testing purpose.</p>


<p>In addition to giving a lot of hints to setup.py, default ubuntu-project enables you to have some automation to specify a data directory that works both from trunk or specified location. If you know a little about python packaging, you can build and install your own local version with python <em>setup.py build -homedir &lt;/usr/local/...&gt; (or -rootdir)</em> and this (and any other path) is taken into account at build time.</p>


<p>Debian package description is taken from setup.py where you can change them by editing</p>
<pre>
description='UI for managing …',
long_description='Here a longer description',
</pre>


<h3>licensing</h3>


<p>Stop! Before packaging and making it available to whole world, you really should license your project</p>


<p>Hum, I know what you think "licensing is boring, I never know what I must do and ship in my package" (how many upstreams never shipped the GPL vanilla file in their tarballs). That's an old story with Quickly. Just launch</p>
<pre>
$ quickly license
</pre>


<p>to create a GPL-3 project, shipping all desirable headers to all files as well as the right LICENSE and AUTHORS file.</p>


<p>It will warn you that you should modify &quot;# Copyright (C) YYYY &lt;Your Name&gt; &lt;Your E-mail&gt;&quot; in Copyright file if you &quot;$ launch quickly license&quot; the first time before launching &quot;$ quickly share&quot; or &quot;$ quickly release&quot; (in the other case, all files are licensed automatically with GPL-3 and name/email are taken from Launchpad). So, edit it manually in the former case.</p>


<p>But well, you are going to tell me that you don't want to license under GPL-V3? Shell-completion will reveal your all supported licenses by Quickly:</p>
<pre>
$ quickly license [Tab][Tab]
BSD     GPL-2   GPL-3   LGPL-2  LGPL-3
</pre>


<p>Quickly always ship the right files and put the header in an according form. Be aware<sup>[<a href="#pnote-131-2">2</a>]</sup> that you can relicensed your project with another license once if you have already licensed it. It will just override your previous license and clean all files accordingly.<p>


<p>If you want to put your own Quickly unsupported License, put your own license text in Copyright file between the tags ### BEGIN AUTOMATIC LICENCE GENERATION and ### END AUTOMATIC LICENCE GENERATION. Then "$ quickly license" does the trick and will add your personal license to every files.</p>


<p>Adding new licenses is really easy, if you think that another license ought to be included in ubuntu-project, do not hesitate to open a bug against Quickly.</p>


<p>So, no more mess when licensing your project. It also updates setup.py and next "$ quickly package" (or "$ quickly release/share) will update the Debian packaging too.</p>


<h3>sharing</h3>


<p>I guess that your are now ready to enable the world to discover an early beta of your application without publishing this version (or intermediate release) as stable one?</p>


<p>Launchpad ppa are great for that and avoid releasing an official version. It's as easy as:</p>
<pre>
$ quickly share
</pre>


<p>It updates your PPA with the the latest saved project changes.</p>


<p>So, before running "$ quickly release", you should: create your account on http://launchpad.net and add a PPA to your launchpad account. Quickly will check that you have an ssh and gpg key. We hope to be able to add advanced support to that in later release.</p>


<p>The first time the command is issued (and that "$ quickly release" was not issued before), it will ask to open a webpage for identifying your Launchpad account so that you can bind Quickly with Launchpad. This is done once by machine (that is to say, all projects on a host share the same launchpad account).</p>


<p>Then name, email and version setup.py will be automatically changed. Version will be &lt;current_release~publicX&gt; where X is incremented
at each &quot;$ quickly share&quot; execution before a new release is done.</p>


<p>If "$ Quickly package" was not issued before, it will run it. Same for licensing, updating name, email, license to every part of your project. No more unlicensed file.</p>


<p>Then, it pushes a source package to your ppa. Just wait for Soyuz (in Launchpad) builds your package and you can make your friend discover your project!</p>


<h3>releasing</h3>


<p>Well well well, when you have tested throughly your package, it's time to release it, isn't?</p>

<pre>
$ quickly release &lt;release_number&gt; notes about changes
</pre>


<p>&lt;version&gt; and &lt;description of the release&gt; are optional. Let&#39;s see first without them:</p>

<pre>
$ quickly release
</pre>


<p>This command first ask for a Launchpad account if you didn't run "$ quickly share" before in any other project. Then, it will propose you to bind your project with an existing Launchpad project, search it with some keywords and then enter the number corresponding to your project. That will be done once per project and finally, if you didn't packaged it previously (neither with quickly package, nor with quickly share), it will do it (same with license issue).</p>


<p>If you didn't put any release version, Quickly will then release it with current number in setup.py (0.1 when you create your project and then, incremented by +0.1 each time the command is issued). If you previously used "$ quickly shared" <a></a>publicX will be dropped to release &lt;current_release&gt; version (as &lt;current_release&gt;~publicX  is less than  &lt;current_release&gt; in packaging world).</p>


<p>If you feel the current version is an "0.8" release (without respecting the +0.1 order), your can launch:</p>
<pre>
$ quickly release 0.8
</pre>


<p>to enforce this value.</p>


<p>Then, Quickly will save the current state (with the optional notes about changes commit message), add a tag to it and bind your bzr branch with the corresponding project in Launchpad (pull and push). Finally, it will create the source package and upload it to your ppa before bumping the revision (+0.1), ready for next changes.</p>


<p>Of course, some setup.py values are updated too like setting project url, pushing it to debian/control, and so on.</p>


<p>And that's it, you have now a new rocking release, updated with all licensing and packaging issues done for you. Enjoy!</p>


<h3>Change Launchpad project</h3>

<p>If you want to unbind and bind again your project with another Launchpad one, you can issue this simple command:</p>

<pre>
$ quickly change-lp-project
</pre>


<p>It enables you to set or change the Launchpad project binded with the current ubuntu project.</p>



<h2>Summary:</h2>

<p>Ok, that's it for the tour, I hope you enjoyed it and it gave you some desire to use Quickly for your own project, develop new templates, and give new ideas!</p>


<p>In a nutshell, in two commands:</p>
<pre>
$ quickly create ubuntu-project myproject
$ quickly release
</pre>


<p>you can have a release of your project, licensed, packaged, under proper revision control. Easy and quick isn't?</p>


<p>Get Things Done Right and... Quickly!</p>


<h2>More info on Quickly:</h2>

<ul>
<li>project page: <a href="/post/en">https://launchpad.net/quickly</a></li>
<li>Quickly wiki page (will be updated soon with this tour content): <a href="/post/en">https://wiki.ubuntu.com/Quickly</a></li>
<li>Karmic Blueprint for Quickly: <a href="/post/en">https://blueprints.launchpad.net/ubuntu/+spec/desktop-karmic-quickly</a></li>
<li>Quickly IRC talk: <a href="/post/en">https://wiki.ubuntu.com/MeetingLogs/devweek0909/QuicklyFun</a></li>
</ul>
<ul>
<li>Previous part of the tour:</li>
</ul>
<ol>
<li><a href="/post/Build-your-application-quickly-with-Quickly%3A-part1" hreflang="en">Part 1: introduction</a></li>
<li><a href="/post/Build-your-application-quickly-with-Quickly%3A-Inside-Quickly-part2" hreflang="en">Part 2: general concepts, core and templates</a></li>
<li><a href="/post/Build-your-application-quickly-with-Quickly%3A-Inside-Quickly-part-3" hreflang="en">Part 3: getting help</a></li>
<li><a href="/post/Build-your-application-quickly-with-Quickly%3A-Inside-Quickly-part-4" hreflang="en">Part 4: shell-completion</a></li>
<li><a href="/post/Build-your-application-quickly-with-Quickly%3A-Inside-Quickly-part-5" hreflang="en">Part 5: miscellaneous core stuff</a></li>
<li><a href="/post/Build-your-application-quickly-with-Quickly%3A-Inside-Quickly-part-6" hreflang="en">Part 6: creating templates</a></li>
<li><a href="/post/Build-your-application-quickly-with-Quickly%3A-Inside-Quickly-part-7" hreflang="en">Part 7: ubuntu-project template presentation</a></li>
<li><a href="/post/Build-your-application-quickly-with-Quickly%3A-Inside-Quickly-part-8" hreflang="en">Part 8: ubuntu-project template code editing</a></li>
</ol>

<p>Press review:</p>

<ul>
<li><a href="http://lwn.net/Articles/351522/" hreflang="en">Quickly on LWN</a></li>
<li><a href="http://arstechnica.com/open-source/news/2009/08/quickly-new-rails-like-rapid-development-tools-for-ubuntu.ars" hreflang="en">arstechnica review</a></li>
<li><a href="http://www.maximumpc.com/article/news/canonical_releases_quickly_framework_speed_linux_app_development" hreflang="en">Maximum PC review</a></li>
</ul>
<div><h4>Notes</h4>
<p>[<a href="#rev-pnote-131-1">1</a>] thanks to distutils-extra and amazing pitti's work<p>
<p>[<a href="#rev-pnote-131-2">2</a>] that is to say, take care about legal form there</p><div>