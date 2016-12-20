+++
title = "Added rhythmbox radio support to unity music lens"
date = "2012-07-04T08:55:00+02:00"
tags = [ "devel", "libre", "programmation", "PU", "ubuntu", "unity" ]
aliases = ["/post/Added-rhythmbox-radio-support-the-unity-music-lens"]
+++
    <p>Just got it merged (and will be freshly available in Unity 6.0 coming soon in quantal)!</p>


<p>I spent few hours last week to add rhythmbox radios (and writing unit tests) for the music lens. It's been a long time I didn't write some serious vala (I guess last time was for unity's Alt+F2). I confirm it's still not my favorite langage ;)</p>


<p>Coming back to the radios, they will now show as the last row (after tracks, albums and eventually online purchase ones), and they respect (if the metadata are provided in rhythmbox) to every usual music lens filters. Some obligatory screenshot:
<a href="/public/ubuntu/radio_in_music_lenses.jpg" title="Rhythmbox radios in music lens"><img src="/public/ubuntu/.radio_in_music_lenses_m.jpg" alt="Rhythmbox radios in music lens" style="display:block; margin:0 auto;" title="Rhythmbox radios in music lens, juil. 2012" /></a></p>


<p>Coming soon, some more news about online radios searching directly in unity (just need a patch in dee for python3 support being released first) ;)</p>