+++
title = "Ubuntu Make 16.02 brings Nodejs, Lighttable IDE, Spring Tools Suite and JetBrains' Datagrip"
date = "2016-02-11T17:48:00+01:00"
tags = [ "devel", "programmation", "PU", "ubuntu", "ubuntu-make", "ubuntulovesdevs" ]
aliases = ["/post/Ubuntu-Make-16.02-brings-Nodejs%2C-Lighttable-IDE%2C-Spring-Tools-Suite-and-JeBrains-Datagrip"]
+++
    <p>What a great <a href="https://github.com/ubuntu/ubuntu-make">Ubuntu Make release</a> today! The best is that all of those new supports (<a href="https://nodejs.org/en/">Nodejs</a>, <a href="http://lighttable.com/">Lighttable IDE</a>, <a href="https://spring.io/tools">Spring Tools Suite</a> and <a href="https://www.jetbrains.com/datagrip/">JeBrains' Datagrip</a>) are all brought thanks to our awesome Ubuntu Make community!</p>


<p><a href="https://github.com/LyzardKing">Galileo Sartor</a> brought a lot of goodness in previous releases, but he didn't stop there! In addition to starting reviewing other branches, he brought the nodejs and Lighttable support with extensive testsuites!</p>


<p><img src="/public/ubuntu/uld/.node_m.png" alt="node.png" style="display:block; margin:0 auto;" title="Node logo" /></p>



<p>I'm sure a lot of developer will appreciate those new frameworks. In addition to that, I'm happy to announce that Galileo has now gained commit access to the project itself! This reflects his ability to quickly dive into the deep layers of the projects and very great skills and questioning when it's needed! Welcome and well done Galileo. \o/</p>


<p><a href="/public/ubuntu/uld/lighttable.png" title="Lighttable IDE"><img src="/public/ubuntu/uld/.lighttable_m.png" alt="lighttable.png" style="display:block; margin:0 auto;" title="Lighttable IDE" /></a></p>



<p>He also implemented symlink creation in a shared bin/ directory added to user's path. This means that you will be able starting from now on to run directly from the command like "android-studio" and others UI tools when there is a desktop file associated. This is only for newly installed frameworks with Ubuntu make 16.02 and onwards.</p>


<p><a href="https://github.com/pperez">Patricio Pérez</a> is a newcomer to the Ubuntu Make contributor family, he jumped on bringing to us excellent Spring Tools Suite support, a customized all-in-one Eclipse based distribution that makes application development easy.</p>


<p>Finally, <a href="https://github.com/oijazsh">Omer Sheikh</a>, a good well-known returning contributor, extended the JetBrains support adding the Datagrip IDE that is tailored to suit specific needs of professional SQL developers and DBAs.</p>


<p><a href="/public/ubuntu/uld/datagrip.jpg" title="JetBrain&#039;s Datagrip"><img src="/public/ubuntu/uld/.datagrip_m.jpg" alt="datagrip.jpg" style="display:block; margin:0 auto;" title="JetBrain&#039;s Datagrip" /></a></p>


<p>You will note that we <a href="https://launchpad.net/ubuntu/+source/ubuntu-make/16.02.1">released as well 16.02.1</a>. We needed to fix Visual Studio Code which changed its website, and thanks to <a href="https://github.com/ubuntu/ubuntu-make/issues/247">a lot of excellent discussions and research from the community</a>, we were able to publish this fix quickly! It's excellent to see both the Ubuntu Make's contributor growing and the code itself, with an excellent meritocracy spirit, based on collaborations!</p>


<p>As usual, you can get this latest version direcly through <a href="https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/ubuntu-make">its ppa</a> for the 14.04 LTS and 15.10 ubuntu releases. Xenial version is available directly in the <a href="https://launchpad.net/ubuntu/+source/ubuntu-make/16.02">xenial ubuntu archive</a>. This wouldn't be possible without our awesome contributors community, thanks to them again!</p>


<p>Our <a href="https://github.com/ubuntu/ubuntu-make/issues">issue tracker</a> is full of ideas and opportunities, and pull requests remain opened for any issues or suggestions! If you want to be the next featured contributor and want to give an hand, you can refer to <a href="/post/How-to-help-on-Ubuntu-Developer-Tools-Center">this post</a> with useful links!</p>