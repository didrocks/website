+++
title = "Ubuntu Make 15.08 with Scala support and a visual studio code hot fix community story"
date = "2015-08-13T08:17:00+02:00"
tags = [ "devel", "libre", "programmation", "PU", "quality", "ubuntu", "ubuntu-make", "ubuntulovesdevs" ]
aliases = ["/post/Ubuntu-Make-15.08-with-Scala-support-and-a-visual-studio-code-hot-fix-community-story"]
+++
    <p>Here is a little bit of the start of my day:</p>


<p>As usual, I open the Ubuntu Make large test suite <a href="/post/Ubuntu-Developer-Tools-Center%3A-how-do-we-run-tests">running continuously</a> against trunk and latest release package. I saw that since yesterday 7PM CEST Visual Studio Code page changed its syntax and is not downloadable anymore by Ubuntu Make.</p>


<p>Jumping on the <a href="https://github.com/ubuntu/ubuntu-make">github's project page</a>, I saw a couple of bugs opened about it, and as well <a href="https://github.com/ubuntu/ubuntu-make/pull/135">a pull request</a> to fix this from a new contributor, <a href="https://github.com/vsimonian">Vartan Simonian</a>! All this in less than 12 hours of this breakage. I just had to merge it, changing the medium tests and cut a release. Hey community work!</p>


<p>That made my day, it was thus high time to release this <a href="https://github.com/ubuntu/ubuntu-make/commit/f0a2e8dadfb4d8a6b2245afea0644d3d995511da">new Ubuntu Make 15.08</a>. Notice that we are starting to follow the scheme "YY.MM" which is quite handy for versioning this kind of continously evolving projects.</p>


<p><img src="/public/ubuntu/uld/scala-logo-white.png" alt="Scala logo" style="float:left; margin: 0 1em 1em 0;" title="Scala logo, août 2015" />
In addition to this fix, you will notice that <a href="https://github.com/ivuk">Igor Vuk</a> added <a href="https://github.com/vsimonian">scala support</a>. Your always fresh-willingness of scala will now be satisfied through Ubuntu Make!</p>


<p>Some other fixes (progress bar out of range by <a href="https://github.com/syndbg">Anton Antonov</a>, new pep8 release issues found) are also part to make this a great release… And we have <a href="https://github.com/ubuntu/ubuntu-make/pulls">even more in the pipe</a>! Thanks again to all our Ubuntu Make contributors, this makes working on this project an awesome journey!</p>


<p>As usual, you can get this latest version direcly in Ubuntu Wily, and through <a href="https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/ubuntu-make">its ppa</a> for the 14.04 LTS, 15.05 ubuntu releases.</p>


<p>Our <a href="https://github.com/ubuntu/ubuntu-make/issues">issue tracker</a> is full of ideas and opportunities, and pull requests remain opened for any issues or suggestions! If you want to be the next featured contributor and want to give an hand, you can refer to <a href="/post/How-to-help-on-Ubuntu-Developer-Tools-Center">this post</a> with useful links!</p>