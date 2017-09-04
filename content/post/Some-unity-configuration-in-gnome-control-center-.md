+++
title = "Some unity configuration in gnome-control-center."
date = "2012-01-26T11:10:00+01:00"
tags = [ "devel", "libre", "PU", "ubuntu", "unity", "gnome" ]
aliases = ["/post/Some-unity-configuration-in-gnome-control-center."]
+++
    <p>Just finished some hacking for implementing some unity configuration options that are blessed by the design team, as shown in this <a href="https://docs.google.com/document/d/1ILTJDiDCd25Npt2AmgzF8aOnZZECxTfM0hvsbWT2BxA/edit?hl=en_US#heading=h.kijjueu5tjdf">official specification</a>.</p>


<p>It contains as well other ui tweaks. You can notice in particular the "Restore defaults" options that work on each tabs and restore every page's defaults.</p>


<p><a href="/public/ubuntu/gnome_control_center_unity1.png" title="Gnome Control Center Unity tab 1"><img src="/public/ubuntu/.gnome_control_center_unity1_m.jpg" alt="Gnome Control Center Unity tab 1" style="display:block; margin:0 auto;" title="Gnome Control Center Unity tab 1, janv. 2012" /></a></p>


<p>Those options are impacting both unity and unity-2d. This gave particular challenges as their features don't align (for instance, we don't show the "set launcher icon size" for 2d) and they don't have the same kind of "launcher hide mode". Also, some configuration options have more choices in ccsm than those shown in the ui (like if you want to reveal the launcher on the bottom-left corner, or if you are using the "dodge active window" mode. We tried to be clever on the ui side and not resetting any different setting you can have set in ccsm by just launching the ui.</p>


<p>We also had to do some choices, like what settings to take by default (on first ui launch), when you have different settings between unity 2d and unity 3d? As there are more ui to tweak 3d than 2d, I thus decided to take the settings from 3d at startup (and then, the settings will align).</p>


<p><a href="/public/ubuntu/gnome_control_center_unity2.png" title="Gnome Control Center Unity tab 2"><img src="/public/ubuntu/.gnome_control_center_unity2_m.jpg" alt="Gnome Control Center Unity tab 2" style="display:block; margin:0 auto;" title="Gnome Control Center Unity tab 2, janv. 2012" /></a></p>


<p>Note that the "reveal spot" doesn't work right now for Top Left corner, but this is a compiz/unity bug and not yet (but soon will be!) implemented feature in 2d.</p>


<p>Finally, if you are using a non unity session like the gnome-panel or gnome-shell one, you won't be impacted by those new settings. You will still gain a new "Restore defaults" option though. :)</p>


<p>The package is currently building and will be available soon in Precise, enjoy!</p>
