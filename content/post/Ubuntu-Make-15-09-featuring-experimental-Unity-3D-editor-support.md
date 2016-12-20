+++
title = "Ubuntu Make 15.09 featuring experimental Unity 3D editor support"
date = "2015-09-01T11:30:00+02:00"
tags = [ "devel", "libre", "programmation", "PU", "quality", "ubuntu", "ubuntu-make", "ubuntulovesdevs" ]
aliases = ["/post/Ubuntu-Make-15.09-featuring-experimental-Unity-3D-editor-support"]
+++
    <p>Last thurday, the <a href="http://blogs.unity3d.com/2015/08/26/unity-comes-to-linux-experimental-build-now-available/">Unity 3D team announced</a> providing some experimental build of Unity editor to Linux.</p>


<p>This was quite an exciting news, especially for me as a personal Unity 3D user. Perfect opportunity to implements this install support in Ubuntu Make, and this is now available for download! The "experimental" comes from the fact that it's experimental upstream as well, there is only one version out (and so, no download section when we'll always fetch latest) and no checksum support. We talked about it on upstream's IRC channel and will work with them on this in the future.</p>


<p><a href="/public/ubuntu/uld/Unity3d-editor.png" title="Unity3D editor on Ubuntu!"><img src="/public/ubuntu/uld/.Unity3d-editor_m.jpg" alt="Unity3D editor on Ubuntu!" style="display:block; margin:0 auto;" title="Unity3D editor on Ubuntu!, sept. 2015" /></a></p>


<p>Of course, all things is, as usual, backed up with tests to ensure we spot any issue.</p>


<p>Speaking of tests, this release as well fix Arduino download support which broke due to upstream versioning scheme changes. This is where our heavy tests <a href="https://jenkins.qa.ubuntu.com/job/udtc-trusty-tests/1744/">investment really shines as we could spot it</a> before getting any bug reports on this!</p>


<p>Various more technical "under the wood" changes went in as well, to make contributors' life easier. We got recently even more excellent contributions (it's starting to be hard for me to keep up with them to be honest due to the load!), more on that next week with nice incoming goodies <a href="https://github.com/ubuntu/ubuntu-make/pulls">which are cooking up</a>.</p>


<p>The whole release details are <a href="https://github.com/ubuntu/ubuntu-make/commit/10eee76d4ec66ab2db125badfe34f547d458dd70">available here</a>. As usual, you can get this latest version direcly through <a href="https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/ubuntu-make">its ppa</a> for the 14.04 LTS, 15.05 and wily ubuntu releases.</p>


<p>Our <a href="https://github.com/ubuntu/ubuntu-make/issues">issue tracker</a> is full of ideas and opportunities, and pull requests remain opened for any issues or suggestions! If you want to be the next featured contributor and want to give an hand, you can refer to <a href="/post/How-to-help-on-Ubuntu-Developer-Tools-Center">this post</a> with useful links!</p>