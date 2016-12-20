+++
title = "Ubuntu Developer Tools Center: how do we run tests?"
date = "2014-09-24T13:30:00+02:00"
tags = [ "android", "devel", "libre", "programmation", "PU", "quality", "ubuntu", "ubuntulovesdevs" ]
aliases = ["/post/Ubuntu-Developer-Tools-Center%3A-how-do-we-run-tests"]
+++
    <p>We are starting to see <a href="https://github.com/didrocks/ubuntu-developer-tools-center/pull/26">multiple</a> <a href="https://github.com/didrocks/ubuntu-developer-tools-center/pull/10">awesome</a> <a href="https://github.com/didrocks/ubuntu-developer-tools-center/pull/21">code</a> <a href="https://github.com/didrocks/ubuntu-developer-tools-center/pull/30">contributions</a> and <a href="https://github.com/didrocks/ubuntu-developer-tools-center/issues">suggestions</a> on our Ubuntu Loves Developers effort and we are eagerly <a href="/post/How-to-help-on-Ubuntu-Developer-Tools-Center">waiting on yours</a>! As a consequence, the spectrum of supported tools is going to expand quickly and we need to ensure that all those different targeted developers are well supported, on multiple releases, always delivering the <a href="https://wiki.ubuntu.com/ubuntu-developer-tools-center">latest version of those environments</a>, at anytime.</p>


<p>A huge task that we can only support thanks to a large suite of tests! Here are some details on what we currently have in place to achieve and ensure this level of quality.</p>


<h3>Different kinds of tests</h3>


<h4>pep8 test</h4>


<p>The <a href="https://github.com/didrocks/ubuntu-developer-tools-center/blob/master/tests/__init__.py#L31">pep8 test</a> is there to ensure code quality and consistency checking. Tests results are trivial to interpret.</p>


<p>This test is running on every commit to master, on each release <a href="https://launchpadlibrarian.net/184394394/buildlog_ubuntu-utopic-i386.ubuntu-developer-tools-center_0.0.5_UPLOADING.txt.gz">during package build</a> as well as <a href="https://jenkins.qa.ubuntu.com/view/All/job/udtc-trusty-tests/label=ps-trusty-desktop-amd64-1,type=pep8/218/console">every couple of hours on jenkins</a>.</p>


<h4>small tests</h4>


<p><a href="https://github.com/didrocks/ubuntu-developer-tools-center/tree/master/tests/small">Those</a> are basically unit tests. They are enabling us to quickly see if we've broken anything with a change, or if the distribution itself broke us. We try to cover in particular multiple corner cases that are easy to test that way.</p>


<p>They are running on every commit to master, on each release during <a href="https://launchpadlibrarian.net/184394394/buildlog_ubuntu-utopic-i386.ubuntu-developer-tools-center_0.0.5_UPLOADING.txt.gz">package build</a>, <a href="https://jenkins.qa.ubuntu.com/view/Utopic/view/AutoPkgTest/job/utopic-adt-ubuntu-developer-tools-center/">every time a dependency is changed in Ubuntu</a> thanks to autopkgtests and <a href="https://jenkins.qa.ubuntu.com/view/All/job/udtc-trusty-tests/label=ps-trusty-desktop-amd64-1,type=small/218/console">every couple of hours on jenkins</a>.</p>


<h4>large tests</h4>


<p><a href="https://github.com/didrocks/ubuntu-developer-tools-center/tree/master/tests/large">Large tests</a> are real user-based testing. We execute udtc and type in stdin various scenarios (like installing, reinstalling, removing, installing with a different path, aborting, ensuring the IDE can start…) and check that the resulting behavior is the one we are expecting.</p>


<p>Those tests enables us to know if something in the distribution broke us, or if a website changed its layout, the download links are modified, or if a newer version of a framework can't be launched on a particular Ubuntu version or configuration. That way, we are aware, ideally most of the time even before the user, that something is broken and can act on it.</p>


<p>Those tests are running <a href="https://jenkins.qa.ubuntu.com/view/All/job/udtc-trusty-tests/label=ps-trusty-desktop-amd64-1,type=large/218/console">every couple of hours on jenkins</a>, using real virtual machines running an Ubuntu Desktop install.</p>


<h4>medium tests</h4>


<p>Finally, the <a href="https://github.com/didrocks/ubuntu-developer-tools-center/tree/master/tests/medium">medium tests</a> are inheriting from the large tests. Thus, they are running exactly the same suite of tests, but in a <a href="https://www.docker.com/">Docker</a> containerized environment, with mock and small assets, not relying on the network or any archives. This means that we ship and emulate a webserver delivering web pages to the container, pretending we are, for instance, https://developer.android.com. We then deliver fake requirements packages and mock tarballs to udtc, and running those.</p>


<p>Implementing a medium tests is generally really easy, for instance:</p>


<blockquote><p>class BasicCLIInContainer(ContainerTests,  test_basics_cli.BasicCLI):</p>
<p>
"""This will test the basic cli command class inside a container"""</p></blockquote>


<p>is enough. That means "takes all the BasicCLI large tests, and run them inside a container". All the hard work, wrapping, sshing and tests are done for you. Just simply implement your large tests and they will be able to run inside the container with this inheritance!</p>


<p>We added as well more complex use cases, like emulating a corrupted downloading, with a md5 checksum mismatch. We generate this controlled environment and share it using trusted containers from <a href="https://registry.hub.docker.com/u/didrocks/docker-udtc/">Docker Hub</a> that we generate from the <a href="https://github.com/didrocks/ubuntu-developer-tools-center/blob/master/Dockerfile">Ubuntu Developer Tools Center DockerFile</a>.</p>


<p>Those tests are running as well <a href="https://jenkins.qa.ubuntu.com/view/All/job/udtc-trusty-tests/label=ps-trusty-desktop-amd64-1,type=medium/218/console">every couple of hours on jenkins</a>.</p>


<p>By comparing medium and large tests, as the first is in a completely controlled environment, we can decipher if we or the distribution broke us, or if a change from a third-party changing their website or requesting newer version requirements impacted us (as the failure will only occurs on the large tests and not in the medium for instance).</p>



<h3>Running all tests, continuously!</h3>


<p>As some of the tests can show the impact of external parts, being the distribution, or even, websites (as we parse some download links), we need to run all those tests regularly<sup>[<a href="#pnote-198-1" id="rev-pnote-198-1">1</a>]</sup>. Note as well that we can experience different results on various configurations. That's why we are running all those tests every couple of hours, once using the system installed tests, and then, with the tip of master. Those are running on various virtual machines (<a href="https://jenkins.qa.ubuntu.com/view/All/job/udtc-trusty-tests/">like here</a>, 14.04 LTS on i386 and amd64).<p>


<p>By comparing all this data, we know if a new commit introduced regressions, if a third-party broke and we need to fix or adapt to it. Each testsuites has a <a href="https://jenkins.qa.ubuntu.com/view/All/job/udtc-trusty-tests/label=ps-trusty-desktop-amd64-1,type=large/lastSuccessfulBuild/artifact/">bunch of artifacts</a> attached to be able to inspect the dependencies installed, the exact version of UDTC tested here, and ensure we don't corner ourself with subtleties like "it works in trunk, but is broken once installed".</p>


<p><a href="/public/ubuntu/uld/trend.png" title="jenkins test results"><img src="/public/ubuntu/uld/.trend_m.jpg" alt="jenkins test results" style="display:block; margin:0 auto;" title="jenkins test results, sept. 2014" /></a></p>


<p>You can see on that graph that trunk has more tests (and features… just wait for some days before we tell more about them ;)) than latest released version.</p>


<p>As metrics are key, we collect <a href="https://jenkins.qa.ubuntu.com/view/All/job/udtc-trusty-tests-collect/label=ps-trusty-desktop-amd64-1/203/console">code coverage and line metrics on each configuration</a> to ensure we are not regressing in our target of keeping high coverage. That tracks as well various stats like number of lines of code.</p>


<h3>Conclusion</h3>


<p>Thanks to all this, we'll probably know even before any of you if anything is suddenly broken and put actions in place to quickly deliver a fix. With each new kind of breakage we plan to back it up with a new suite of tests to ensure we never see the same regression again.</p>


<p>As you can see, we are pretty hardcore on tests and believe it's the only way to keep quality and a sustainable system. With all that in place, as a developer, you should just have to enjoy your productive environment and don't have to bother of the operation system itself. We have you covered!</p>


<p><img src="/public/ubuntu/uld/uld-small.png" alt="Ubuntu Loves Developers" style="display:block; margin:0 auto;" title="Ubuntu Loves Developers" /></p>


<p>As always, you can reach me <a href="https://plus.google.com/+DidierRoche">on G+</a>, #ubuntu-desktop (didrocks) on IRC (freenode), or sending any issue or even pull requests against the <a href="https://github.com/didrocks/ubuntu-developer-tools-center">Ubuntu Developer Tools Center project</a>!</p>
<div class="footnotes"><h4 class="footnotes-title">Note</h4>
<p>[<a href="#rev-pnote-198-1" id="pnote-198-1">1</a>] if tests are not running regularly, you can consider them broken anyway</p><div>
