+++
title = "New, shiny, Unity 5.6 released!"
date = "2012-03-12T15:10:00+01:00"
tags = [ "libre", "PU", "ubuntu", "unity" ]
aliases = ["/post/New%2C-shiny%2C-Unity-5.6-released%21"]
+++
    <p>Phew! it's been a long road to release the next <a href="http://unity.ubuntu.com/">unity</a>, but I'm more than happy to finally announce the<a href="https://launchpad.net/unity/5.0/5.6.0"> release of 5.6</a>. Unity components (dee, libunity, bamf, lenses, nux) and unity itself, plus some compiz snapshots (post 0.9.7.0) are part of this release. The packages are currently building on the official builders and should be soon available to you.</p>


<p>No particular new feature apart from better ibus support are part of it, plus <a href="https://launchpad.net/unity/5.0/5.6.0">a tons of bug fixes</a> and some miscelleanous improvements:
- Daniel van Vungt landed a patch in compiz that enhances its performance for more than 51%! When you test it, I can ensure you feel a real noticeable difference (in particular on older machines, like mine).
- The alt tap false positive revealing the HUD is now part of the past. We know this one was annoying people, I can only tell you it's been technically challenging ;). This has been a rocking combined effort in compiz/unity sides.
- the file lens can now find files that were never opened before.</p>


<p>I mentioned challenge… Yeah this release was challenging both process-wise and technically one. We noticed once we had frozen the code quite a fair number regressions. Tracking them and fixing took some times, regressed other things to track, asked for a new round of testing, regressed another part… That was a funny quite of race between issues as you can infer.
But we keep it straight, we didn't release before everything was ready. Precise is about quality, and we will keep this strict path until 12.04 LTS is here (and continue after that!). All branches that enter trunk, at release time, have sensible tests associated. Tests are improved everyday and becoming easier to write thanks to the restless Thomi's work. We are more and more confident in everyone's work thanks to that.</p>


<p>Of course, everything is not perfect, the freeze process is debated and we will discuss how things can be improved, particularly when we have a long freeze time because of multiple regressions like this time. We are building upon those feedbacks and hope to make the process even better as soon as for executing it for unity 5.8, our next target.</p>


<p>Well done product stategy team. Keep it on!</p>