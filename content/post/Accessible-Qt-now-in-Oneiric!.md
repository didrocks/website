+++
title = "Accessible Qt now in Oneiric!"
date = "2011-06-23T12:15:00+02:00"
tags = [ "devel", "libre", "PU", "qml", "qt", "ubuntu" ]
aliases = ["/post/Accessible-Qt-now-in-Oneiric%21"]
+++
    <p>Accessibility is one of the <a href="https://wiki.ubuntu.com/Accessibility">core value</a> of Ubuntu.</p>


<p><img src="https://wiki.ubuntu.com/Accessibility/HeaderMain?action=AttachFile&amp;do=get&amp;target=accessibilityteam.png" alt="Ubuntu accessibility team logo" /></p>


<p>When we announced that Unity 2D will be the fallback if you have no required hardware or driver for unity 3D, one of the compulsory request for that is that Unity 2D should be accessible. Even if one of the goal for the cycle is to make Unity fully accessible<sup>[<a href="#pnote-118-1" id="rev-pnote-118-1">1</a>]</sup> we should still consider people needed accessibility and not filling the requirements for it.<p>


<p>In addition to that, we have now in Oneiric <a href="http://www.markshuttleworth.com/archives/568">Qt installed by default as previously decided</a>, and if we want to be toolkit agnostic, we need a good accessibility story on that toolkit as well… and Qt is known to have a blurry story about accessibility :)</p>


<p>Unity 2D is written in <a href="http://doc.qt.nokia.com/4.7-snapshot/qdeclarativeintroduction.html">QML</a>, and so based on Qt technology. So what happens?</p>


<p>It's been already some monthes that Qt people are working on accessibility in Qt and QML itself, basically working with at-spi2 technology (the same used in GNOME). Last week, a lot of Qt contributors gathered at the <a href="http://labs.qt.nokia.com/2011/03/16/save-the-date-%E2%80%93-qt-contributors-summit/" title="Qt contributor summit"></a> and I had the chance to spend some time in Berlin there too. The awesome and very friendly <a href="http://blogs.fsfe.org/gladhorn">Frederik Gladhorn</a> worked to backport the QML accessible branch (done in a in-development 4.8 branch, with some nice features of Qt 5 ;)) to cut all non relevant parts down and backport to a 4.7 branch (as we have 4.7.3 in ubuntu and will probably stick with 4.7.4 in Oneiric looking at the schedule). I just started from it, rebased on 4.7.3 and uploaded today (after some staging test days in the ubuntu-desktop ppa) to Oneiric!</p>


<p>So, if you upgrade your oneiric box and install as well the new <a href="apt://qt-at-spi">qt-at-spi</a> package (in universe right now, but should be soon land in main), you will get an accessible Qt and QML! To activate it (it's not activated by the global configuration over dbus right now), launch Orca or accerciser (common accessibility tools) and run your Qt application with <strong>QT_ACCESSIBILITY=1</strong>. We know right now that it's a little crashy (only with the environment variable set), but do not hesitate to test! Please report crashes/issues to the Qt package in launchpad and tag them <strong>a11y</strong> so that it's easier for us to find and leverage issues upstream. We can't do it without your helps and we need feedbacks, so please test. :)</p>


<p>That won't unfortunately make unity 2D instantly accessible though. Indeed, QML has not right now a standard desktop toolkit<sup>[<a href="#pnote-118-2" id="rev-pnote-118-2">2</a>]</sup>, every QML project creates right now its own components, and so, there is no accessiblity magic to now what to expose or not. Consequently, the unity 2D guys will implement and test the new property to be set so that Orca (for instance), can read the widgets content. Accerciser though already introspects unity 2D and other QML projects content, which means that it seems to work. :)<p>


<p>Thanks again to Frederik, it's a pleasure to work with him and thanks to all guys working on Qt making that possible! Let's continue working together and push accessibility in Qt and QML as far as possible :)</p>
<div class="footnotes"><h4>Notes</h4>
<p>[<a href="#rev-pnote-118-1" id="pnote-118-1">1</a>] which is almost there thanks to the awesome work done last cycle<p>
<p>[<a href="#rev-pnote-118-2" id="pnote-118-2">2</a>] but works is <a href="http://labs.qt.nokia.com/2011/03/10/qml-components-for-desktop/">under way</a></p><div>
