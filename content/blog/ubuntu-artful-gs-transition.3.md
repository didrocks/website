---
title: "Ubuntu GNOME Shell in Artful: Day 3"
date: 2017-08-16T11:00:25+02:00
tags: [ "pu", "ubuntu" ]
banner: "images/artful-shell-transition/wednesday-16-august-sound.png"
type: "post"
draft: true
---

After introducing yesterday a real GNOME vanilla session, let's see how we are using this to implement small behavior differences and transforming current Ubuntu Artful. For more background on this, you can refer back to our decisions regarding our default session experience as [discussed in my blog post](/2017/08/03/ubuntu--guadec-2017-and-plans-for-gnome-shell-migration/).

# Day 3: a story of sound and collaboration…

Some devices have very low volume even when pushed at their maximum. One example for this is the x220 when most of videos on youtube, or listening to music in rhythmbox doesn’t give great results even at maximum volume.

Pulseaudio can amplify some of those sound devices (detecting if amplification is available on those output sources). Doing it, at the price of sound quality, makes at least the embedded speakers usable.

There are 3 components involved:
* gnome-control-center, which has a slider to push volume for supported device upper than 100%.
* gnome-settings-daemon, detecting multimedia keys, for volume up and down.
* gnome-shell, showing up a slider in the volume menu.

However, this brings some issues in the current design: if you set the volume above 100% in GNOME Control Center, and then press the multimedia key up, the volume is reset to 100%, and you can't go further using only keys. A similar behavior is observable using GNOME Shell slider which reset the sound below what you did set in GNOME Control Center.

As a video will explain it better than words:

{{< youtube jptl2L15kKI >}}

This behavior gets a little bit annoying when using it day to day on such hardware (you set the volume greater than 100%, then press the multimedia key "up" and it's back to unhearable state) and not that consistent as the sliders between the OSD, GNOME Shell and GNOME Control Center aren't in sync.

What we did few cycles ago in Ubuntu, was to introduce the idea to be able to set the volume above 100% from all of those, but conditionally. This was done in unity control center, Ubuntu settings daemon and unity for 14.04.

When the switch is off, which is the default, we removed any possibility to amplify and set the sound volume above 100%, being from GNOME control center, multimedia keys or any Shell visible slider to keep consistency. The option appears only though if any output source supports amplified volume. It's disabled otherwise. If it's available and you then switch it on, all of those methods enables amplifying the volume, syncing the sliders in a consistent manner.

We alked about it at GUADEC with [Allan](https://blogs.gnome.org/aday/) who needed a little bit more context and time to think about it (it might be a little bit late for this GNOME 2.26 cycle anyway). However, we didn't want to keep Ubuntu 17.10 with the current behavior, so we reimplemented that design, but made it fully conditional to the `XDG_CURRENT_DESKTOP` variable [we talked about yesterday](https://didrocks.fr/2017/08/15/ubuntu-gnome-shell-in-artful-day-2/). Consequently, the GNOME vanilla session is showing off the behavior you can see in the above video, and nothing in the upstream experience is impacted. However, the Ubuntu session, conditioned by this variable (as for any gsetting keys override), will show off the modifications we made in [gnome settings daemon](https://launchpad.net/ubuntu/+source/gnome-settings-daemon/3.24.3-0ubuntu3), [GNOME Shell](https://launchpad.net/ubuntu/+source/gnome-shell/3.24.3-0ubuntu3) and [GNOME Control Center](https://launchpad.net/ubuntu/+source/gnome-control-center/1:3.24.3-0ubuntu3), using our own gsettings key that we [seeded back](https://launchpad.net/ubuntu/+source/ubuntu-meta/1.394) under the `com.ubuntu` settings namespace so that we don't abuse the `org.gnome` one until this is blessed upstream. People who enabled that settings in previous Unity session will thus be automatically transitioned.

Here is a video illustrating this behavior:

{{< youtube rZ17JB9SRvk >}}

For people who want to see the discussion we're having with the GNOME design team to get something similar to this (maybe it won't be completely the same, but in the same vein), you can head over to the [corresponding bug](https://bugzilla.gnome.org/show_bug.cgi?id=710424).

We are trying to use these conditional per-session patches to keep the upstream vanilla session as vanilla as possible when it makes sense (meaning, when it's not about integrating with a debian package based distro for instance). That enables us to deliver the desired and current upstream look and feel, while still be able to propose and implement slightly subtle changes we think are important for our user base.

As yesterday, if you are eager to experiment with these changes before they migrate to the artful release pocket, you can head over to [our official Ubuntu desktop team transitions ppa](https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/transitions) to get a taste of what's cooking!

There is still something in the GNOME vanilla session which is different from upstream, can you spot it? (Hint: it's an icon). It feels as well a little bit out of place in the Ubuntu one. We are going to fix it tomorrow :)



