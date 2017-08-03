---
title: "Ubuntu @ GUADEC 2017 and plans for GNOME Shell migration"
date: 2017-08-03T09:33:03+02:00
tags: [ "pu", "ubuntu" ]
banner: "images/guadec2017/group.jpg"
type: "post"
---

My first GUADEC was 8 years ago, but it's been a couple of years I haven't been to that conference. I can say that GUADEC 2017 in Manchester was a real blast!

Ubuntu had a very strong presence this year, people from Canonical, even if some only stayed for few days ([Chris](https://twitter.com/chrisccoulson), [Iain](https://medium.com/@iain.lane), [Ken](http://ken.vandine.org), [Matthew](https://twitter.com/mpt), [Robert](http://bobthegnome.blogspot.com/), [Will](https://www.whizzy.org/), [Sébastien](https://blogs.gnome.org/seb128/) and [I](https://didrocks.fr)) and from Ubuntu GNOME via [Tim Lunn](https://wiki.ubuntu.com/TimLunn), giving his perspective in a talk for this Ubuntu flavor.

Even if we have always used GNOME technology (default applications, Unity is using Glib & GTK as its toolkit), what about Ubuntu guys switching back to a full GNOME Shell experience? Of course, some people may have thought this could have gone like this at the conference:

![Alberto fighting hard](/images/guadec2017/alberto_fight.jpg)
(Alberto was working on Ubuntu at the time)

But it was rather like this:
![We are friends](/images/guadec2017/friends1.jpg)

or this:
![We are friends of friends](/images/guadec2017/friends2.jpg)

and even like this:
![Even more friendly](/images/guadec2017/friends3.jpg)

(those images are from GUADEC 2012 in A Coruña, more info on [Alberto's blog post](https://siliconislandblog.wordpress.com/2))

In a nutshell, we felt really welcomed at this GUADEC, it's time to reunite! I want to particular thanks [Alberto Ruiz](https://siliconislandblog.wordpress.com) and [Allan Day](https://blogs.gnome.org/aday/) to have helped us and eased that. We even had a good round of applause during the [GNOME AGM](https://2017.guadec.org/talks-and-events/#abstract-100-gnome_foundation_agm_part_1) which was profoundly heartwarming. Nice to be part of this awesome community and family, moving forward and leave history behind us!

# Default GNOME experience in Ubuntu 17.10

We came with some topics we wanted to discuss in mind but no pre-made decisions (Ubuntu artful is just a transitioned Unity -> GNOME Shell environment at the moment) and we wanted to get things figured out at GUADEC, as a community. Talking extensively with the GNOME design team and key GNOME Shell & experience contributor ([Alan](https://blogs.gnome.org/aday/), [Florian](https://blogs.gnome.org/fmuellner/), [Jakub](http://jimmac.musichall.cz/), [Matthias](https://blogs.gnome.org/mclasen/)) helped us figuring out where we should head down, with a common upstream agreement and understanding. We discussed what is great in the GNOME Shell experience, what we thought was good in the Unity one, how some notions may be desirable in the future, what we can port back to the upstream repositories and such.

So, what are we going to do for our incoming Ubuntu 17.10 release? After a lot of back and forth and based on [our user's survey results](https://insights.ubuntu.com/2017/06/12/ubuntu-desktop-gnome-extensions-poll-results/), we are going to use an always visible dock-like experience as a GNOME Shell extension by default. However, we won't push all the features that [dash to dock](https://extensions.gnome.org/extension/307/dash-to-dock/) has as part of our common (both GNOME Shell and Unity) design principles, but working with this extension upstream that we already contacted and worked with to ensure we don't diverge on the codebase. Also, we don't want to be incompatible with users who wants more tweaking options and install this extension instead of our slightly modified dock. There are other reasons for wanting to modify and host there in the upstream GNOME extensions repository, alongside GNOME classic (and few others) set of extensions. I'll detail that in a future post with more technical internals on how to achieve this.

Also, the default session will have our Ambiance GTK theme (including Shell), icon theme and ubuntu font by default. Applications control buttons will be …drum rolls… on the right[^1], with a minimize, maximize and close buttons! This vision is more compatible with having a dock always visible by default, while following more closely GNOME design for button placement. We want to keep those behavior changes like nautilus showing icons on the desktop as minimal as possible, and we are working upstream discussing all this, porting some of the changes and benefits discussed in the Unity experience back to GNOME when desired.

Finally, we are going to include more GNOME apps in our default experience set. The exact details of which ones are still being discussed.

# An upstream vanilla session

![Vanilla GNOME Shell 3.24](/images/gnome_3.24.jpg)

Alongside this direction to be more upstream, we want to provide as well a much more vanilla GNOME experience to our users for the first time! We will implement thus our modifications as a GNOME Shell mode (similar to how GNOME classic is implemented). This will enable us to deliver another session using upstream fonts, icons, adwaita and Shell themes, close only button by default and no extension! Although, some small behavior changed will be conditioned by session, and we are trying to fuel that back in those changes upstream (like setting volume above 100% using media keys under some conditions). All this will be available via just `apt install gnome-session` and those additional sessions will be selectable at the login screen. People upgrading from Ubuntu GNOME will have those sessions directly installed, and the ubuntu desktop team recommends to have users transitioned to that session rather than the ubuntu one. We leave that decision to the Ubuntu GNOME community though. A big thanks to [Allison](https://blogs.gnome.org/desrt/) who just came up during the unconference days with [some glib/gsettings patches](https://bugzilla.gnome.org/show_bug.cgi?id=746592) for enabling us doing this! (Remember Allison that we talked about per session overrides in 2010 first? ;)).

This will enable to do some user testing between upcoming GNOME Shell new designs, current GNOME Shell experience and our GNOME Shell session with this default dock, and report them back openly, sharing results inside our GNOME community.

# Changes for our users

So, that means as well that some things will change for our user coming from an Unity experience. Off the top of my head, global menu, the HUD, alt-tab behavior, messaging menus, volume notifications, launcher integration via running apps & software center, lenses & scopes, and other tweaks are thus not be part of the default experience anymore (at least in the upcoming release). Though, people have access to [a wide variety of extensions](https://extensions.gnome.org/) if they want to get similar experience. Those were the parts we discussed with the GNOME design team extensively and we hope to bring experience and patches for some of the commonly acknowledged and desired changes in the future. Some other jointly effort is on the way (thanks [Michael](http://mhall119.com/) and [Endless](https://endlessos.com/)), to ease user's transition, via documentation.

![Ubuntu 17.10 is NOT going to look like this](/images/not_ubuntu_2017.jpg)

Oh, one last thing… we are going to switch over Wayland by default on 17.10. The Xorg session will still be available alongside. This enables us to get some good set of feedback to be ready and make our final decision for our next LTS, 18.04.
 
All those changes will be rolling out in the next couple of weeks and you can see all changes coming in if you follow artful closely. A theming, fit and finish community hackfest will be held very soon in London which we'll publicize here. There is a lot to talk about the transition details itself, which keys are going to migrate, which keys aren't and why, but I'll cover that in future blog posts, when this work is in progress.

Thanks again to all our GNOME family and for this awesomely great GUADEC 2017! I really loved Manchester, despite the weather and will surely head there back on my own soon!

![GNOME is made of people](/images/gnome_people_logo.jpg)

**To a bright GNOME and Ubuntu future!**

[^1]: I need now to disable comments on my blog… Oh shoot! :p

Guadec group photo by [Jonathan Kang](http://jkangsc.blogspot.fr/), under CC BY-SA 2.0.