+++
title = "Unity 5.8 out, ready for beta2!"
date = "2012-03-23T17:30:00+01:00"
tags = [ "libre", "PU", "ubuntu", "unity" ]
aliases = ["/post/Unity-5.8-out%2C-ready-for-beta2%21"]
+++
    <p>As I write Unity 5.8 is currently building on our official builders and will reach ubuntu precise soon. For this release, a big part of the stack was uploaded (14 components) including unity, unity-2d, nux of course, but also compiz 0.9.7.2 and the lenses (with some ABI breaks in the middle).</p>


<p>What's new on this unity release? Well, you can see the <a href="https://launchpad.net/unity/+milestone/5.8.0">3d milestone</a> and <a href="https://launchpad.net/unity-2d/+milestone/5.8">2d bug fixes</a> (look also at <a href="https://launchpad.net/unity-2d/+milestone/5.7">this one</a> for unity 2d which landed last monday). Keep in mind also that some 3d bug fixes benefits 2d as well! So, a lot of bug unitfixes, but that's not only it! We also got some UI refinements, some very visible, some more hidden, but it's all those little touch which makes the whole experience better!</p>


<p>Do not forget as well that the music lens is now taking back an important role as it supports rhythmbox in addition to banshee!</p>


<p><a href="/public/ubuntu/gnome-control-center-display-panel.png" title="Gnome Control Center dIsplay Unity panel"><img src="/public/ubuntu/.gnome-control-center-display-panel_m.jpg" alt="Gnome Control Center dIsplay Unity panel" style="display:block; margin:0 auto;" title="Gnome Control Center dIsplay Unity panel, mar. 2012" /></a></p>


<p>Also, we won some multimonitors new capabilities in both 2d and 3d. Basically, you can now decide if you want sticky edges between your monitors or not<sup>[<a href="#pnote-161-1" id="rev-pnote-161-1">1</a>]</sup>, and decide if you want one launcher (which is then set on your primary monitor), or a launcher on each monitor. Gnome Control Center was patched to add those default options (which only appears on an unity session) back to the main experience. You will now notice as well a more "unityish" preview with a panel and a launcher displayed. Dragging the launcher also enables to change where you want to set it in addition to the combobox (think about clicking on apply to get the choice taken into account!).<p>


<p>We encountered some hiccups but we either fixed them or workarounded other issues for landing the new stack in beta2. We still have an annoying issue which seems to be appearing rarely (only on some configurations). This is logged as critical <a href="https://bugs.launchpad.net/ubuntu/+source/unity/+bug/963093">on this bug</a>. If you encounter this latter, you should choose the unity-2d session for now on your logging screen. Well done to everyone involved on this release!</p>


<p>We are trying as well a new approach to avoid having the current development version of ubuntu uninstallable for a while (because of a long dependency chain in all the related unity components) while everything is building. Basically, we are using experimentally the -proposed pocket and once everything is built, it will be copied to the main archive. Thanks for the release team to push the right buttons to enable this, let's hope this experiment will be successful and that we can generalize it in Q for every unity release and other components (and not only when we are frozen)!</p>


<p>Hope you will enjoy this release and precise beta2, which is <a href="https://wiki.ubuntu.com/PrecisePangolin/ReleaseSchedule">just around corner</a>!</p>
<div class="footnotes"><h4>Note</h4>
<p>[<a href="#rev-pnote-161-1" id="pnote-161-1">1</a>] warning, <a href="https://bugs.launchpad.net/unity-distro-priority/+bug/961285">still some work has to be done</a></p><div>
