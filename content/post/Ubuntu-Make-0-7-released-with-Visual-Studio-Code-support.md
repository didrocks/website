+++
title = "Ubuntu Make 0.7 released with Visual Studio Code support"
date = "2015-04-30T13:38:00+02:00"
tags = [ "devel", "libre", "programmation", "PU", "quality", "ubuntu", "ubuntu-make", "ubuntulovesdevs" ]
aliases = ["/post/Ubuntu-Make-0.7-released-with-Visual-Studio-Code-support"]
+++
    <p>If you followed recent news, yesterday Microsoft announced <a href="https://code.visualstudio.com/">Visual Studio Code</a> support on stage during their <a href="http://www.buildwindows.com/">Build</a> conference. One of the nice surprise was that this new IDE, focused on web and cloud platforms, is available on Mac OS X and of course, on Linux! Some screenshots were presented at the conference with Visual Studio Code running on Ubuntu in an Unity Session.</p>


<p>This sounded like a nice opportunity for <a href="http://wiki.ubuntu.com/ubuntu-make">Ubuntu Make</a> to shine again, and we just added this new support! And yeah, it's a <a href="https://developer.ubuntu.com/en/snappy/">snappy</a> feeling to get it delivered as fast! This release of course brings as well the required non regression large and medium tests to ensure we can track that the installation is working as expected as time pass by and detect any server-side or client-side regression.</p>


<p>To install it, just run:</p>


<p><code>$ umake web visual-studio-code</code></p>


<p>Here is the required screenshot of a fresh Visual Studio Code installation with Ubuntu Make!</p>


<p><a href="/public/ubuntu/uld/visual-studio-code.png" title="Visual Studio Code"><img src="/public/ubuntu/uld/.visual-studio-code_m.jpg" alt="Visual Studio Code" style="display:block; margin:0 auto;" title="Visual Studio Code, avr. 2015" /></a></p>


<p>You can get Ubuntu Make 0.7 through <a href="https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/ubuntu-make">its ppa</a>, for the 14.04 LTS, 14.10 and 15.05 ubuntu releases.</p>


<p>Our <a href="https://github.com/ubuntu/ubuntu-make/issues">issue tracker</a> is full of ideas and opportunities, and pull requests remain opened for any issues or suggestions! For all the various form of contributions and how to give an hand, you can refer to <a href="/post/How-to-help-on-Ubuntu-Developer-Tools-Center">this post</a>!</p>