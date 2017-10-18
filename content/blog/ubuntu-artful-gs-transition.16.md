---
title: "Ubuntu GNOME Shell in Artful: Day 16"
date: 2017-10-18T11:15:25-05:00
tags: [ "pu", "ubuntu", "gnome" ]
banner: "images/artful-shell-transition/ubuntu-default-sessions.png"
type: "post"
manualdiscourse: "https://community.ubuntu.com/t/ubuntu-gnome-in-artful-day-16/790"
---

All good things must come to an end, however, in that particular case, it's rather a beginning! We are indeed almost done in our road to Artful, which means that 17.10 is just around the corner: official Ubuntu 17.10 release [is due tomorrow](https://wiki.ubuntu.com/ArtfulAardvark/ReleaseSchedule). Of course, it doesn't mean we stop right away working on it: you will have bug fixes and security updates for 9 months of support! It's thus time to close this series on Artful, and for this, we are going to tackle one topic we didn't get to yet, which is quite important approaching the release: upgrading from a previous Ubuntu release! For more background on our current transition to GNOME Shell in artful, you can refer back to our decisions regarding our default session experience as [discussed in my blog post](/2017/08/03/ubuntu--guadec-2017-and-plans-for-gnome-shell-migration/).

# Day 16: Flavors, upgrades and sessions!

## Different kind of sessions

Any new Ubuntu installation will have two sessions available at most, whose base name is "Ubuntu":

* The "Ubuntu" session corresponds to GNOME Shell experience with our modifications (Ubuntu Dock, appindicator support, our theme, small behavior changes…). You have probably seen those and followed their development on previous blog posts. This is the default session running under Wayland.
* The "Ubuntu on Xorg" session, being similar to the previous one, but running on Xorg as the name indicates :) Users who can't run Wayland (using nvidia proprietary driver or unsupported hardware) should be automatically fallbacked and only presented with that session.

![Ubuntu default installation sessions](/images/artful-shell-transition/ubuntu-default-sessions.png)

Those two sessions are available when you install the **ubuntu-session** package.

However, more sessions are available in the Ubuntu archives around GNOME technologies: the Unity and vanilla GNOME ones. The first one is available as soon as you install the **unity-session** binary package. The vanilla GNOME sessions simply appears once **gnome-session** is installed. After a reboot, GDM is presenting all of them for selection when login in.

![All available sessions](/images/artful-shell-transition/ubuntu-available-sessions.png)

Let's see how that goes on upgrades.

## Upgrading from Ubuntu 17.04 or Ubuntu 16.04 (LTS)

People running today Ubuntu 17.04 or our last LTS, Ubuntu 16.04, are generally using Unity. As with every releases when upgrading people being on one default, we upgrade them to the next new default. It means that on upgrades, those users will reboot in our default Ubuntu GNOME Shell experience, having the "**Ubuntu**" and "**Ubuntu on Xorg**" sessions available. The "**Ubuntu**" session is the default and will lead you to our default and fully supported desktop:

![Ubuntu GNOME Shell on 17.10](/images/artful-shell-transition/final_freeze_ubuntu_17.10_desktop.png)

However, we don't remove packages that are still available in the distribution on upgrades. Those users will thus have an additional "**Unity**" session option, which they can use to continue running Unity 7 (and thus, on Xorg only). Indeed, Unity is still present, in universe (meaning we don't commit to strong maintenance or security updates), but we will continue to have a look at it on a best effort bases (at least until our next LTS). Some features are slightly modified to either don't collide with GNOME Shell experience or to follow more the upstream philosophy, like the headerbars I mentioned in [my previous blog post](/2017/10/16/ubuntu-gnome-shell-in-artful-day-15/). In a nutshell, don't expect the exact same experience that you used to have, but you will reach similar familiarity for the main concepts and components.

![Unity on 17.10](/images/artful-shell-transition/unity-17.10.png)

## Upgrading from Ubuntu GNOME 17.04 or Ubuntu GNOME 16.04

Those people were experiencing a more vanilla upstream GNOME experience than our Ubuntu session. It was a little bit 50/50 in what to do for those users on upgrades as they were used to something different. The final experience that Ubuntu GNOME users will get is to have those 2 upstream vanilla GNOME session, ("**GNOME**" and "**GNOME on Xorg**"). Those will stay the default after upgrades.

![Vanilla GNOME Shell session on 17.10](/images/artful-shell-transition/vanilla-gnome-17.10.png)

In addition to those sessions, we still want to give an easy option for our users to try our new default experience, and thus, the 2 "Ubuntu" sessions (Wayland & Xorg) are automatically installed as well on upgrade. The sessions are just around for user's convenience. :)

## Fallback

I want to quickly mention and give kudos to [Olivier](https://launchpad.net/~osomon) who fixed [a pet bug of mine](https://launchpad.net/bugs/1718446), to ensure that Wayland to Xorg automatic fallbacking will always fallback to the correct sessions (Ubuntu will fallback to Ubuntu on Xorg and GNOME to GNOME on Xorg). His patches were [discussed upstream](https://bugzilla.gnome.org/show_bug.cgi?id=788552) and are [now committed in the gdm tree](https://git.gnome.org/browse/gdm/commit/?id=0c9f6629bf2b25898375d020603efd3326f8824d). This will be quickly available as a [stable release update](https://wiki.ubuntu.com/StableReleaseUpdates) conveniently as only impacting upgrades.

## In a nutshell

To sum all that up:

* New installs will have the "Ubuntu and Ubuntu on Xorg" options. Ubuntu under wayland being the default.
* Upgrades from Ubuntu 16.04 and 17.04 will get Ubuntu (default), Ubuntu on Xorg and Unity sessions installed.
* Upgrades from Ubuntu GNOME 16.04 and 17.04 will get GNOME (default), GNOME on Xorg, Ubuntu and Ubuntu on Xorg sessions installed.
* Fallbacking when Wayland is not supported will fallback to the corresponding Xorg session.

And this is it for our "road to Artful" blog post long series! I hope you had as much fun reading it as I had writing and detailing the work done by the Ubuntu desktop team to make this transition, we hope, a success. It was really great as well to be able to interact and answers to the many comments that you posted on the dedicated section. Thanks to everyone participating there.

You can comment on the [community HUB]() and participate and contribute from there! We will likely redo the same experiment and keep you posted on our technical advancement for the Ubuntu 18.04 LTS release. You should expect fewer posts as of course as the changes shouldn't be so drastic as they were this cycle. We will mostly focus on stabilizing, bug fixes and general polish!

Until then, enjoy the upcoming Ubuntu 17.10 release, watch the [ubuntu.com website](https://www.ubuntu.com/) for the release announcement on desktop, servers iot and clouds, [join our community HUB](https://community.ubuntu.com/)… and see you soon around! :)

Didier
