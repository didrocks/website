+++
title = "Ubuntu Make 15.09.2 enables you to install Android SDK only."
date = "2015-09-10T11:30:00+02:00"
tags = [ "android", "devel", "libre", "programmation", "PU", "quality", "ubuntu", "ubuntu-make", "ubuntulovesdevs" ]
aliases = ["/post/Ubuntu-Make-15.09.2-enables-you-to-install-Android-SDK-only."]
+++
    <p>I'm proud to announce this new Ubuntu Make release, with excellent new feature and fixes from our community.</p>


<p>First, welcome <a href="https://github.com/sebschub">Sebastian Schubert</a> to the Ubuntu Make contributor family. He did some awesome work on implementing Android SDK only support (for those not wanting to install the whole Android Studio bundle) in Ubuntu Make! As usual, this is backed up with large and medium tests to cover us, great enhancement! :)</p>


<p>The new option to install android sdk is:</p>

<pre>$ umake android android-sdk</pre>


<p>You will get into your user PATH (after next login) the expected android platform tools.</p>


<p>Secondly, <a href="https://github.com/oijazsh">Omer Sheikh</a>, who already implemented <a href="/post/Ubuntu-Make-0.9.2-hot-from-the-builders%2C-with-Firefox-Developer-Edition-language-support">language selection in firefox developer edition</a>, came back with some heavy duty of rationalizing every exit codes accross Ubuntu Make, to ensure we always exit with the expected error code in every situation. Not only he implemented this, but also he did grow our testsuite to ensure that any bad download page are properly detected! Awesome work.</p>


<p>Smaller fixes sneaked in as well and you can get the <a href="https://github.com/ubuntu/ubuntu-make/commit/6cc70d2a2613c864cd4c2e7fa2725bbc00cae562">full release content details here</a>. As usual, you can get this latest version direcly through <a href="https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/ubuntu-make">its ppa</a> for the 14.04 LTS, 15.05 and wily ubuntu releases.</p>


<p>Our <a href="https://github.com/ubuntu/ubuntu-make/issues">issue tracker</a> is full of ideas and opportunities, and pull requests remain opened for any issues or suggestions! If you want to be the next featured contributor and want to give an hand, you can refer to <a href="/post/How-to-help-on-Ubuntu-Developer-Tools-Center">this post</a> with useful links!</p>