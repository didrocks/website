---
title: "Ubuntu GNOME Shell in Artful: Day 10"
date: 2017-09-08T10:35:25+01:00
tags: [ "pu", "ubuntu", "gnome" ]
banner: "images/artful-shell-transition/dock_settings.png"
type: "post"
---

It's time for the 10th update iteration on how we are progressing in your new GNOME Shell Ubuntu Artful default session. This one is about Control Center updates to latest GNOME version. For more background on this, you can refer back to our decisions regarding our default session experience as [discussed in my blog post](/2017/08/03/ubuntu--guadec-2017-and-plans-for-gnome-shell-migration/).

# Day 10: Dock settings in its own panel

I've already blogged about it [a few weeks ago](https://didrocks.fr/2017/08/18/ubuntu-gnome-shell-in-artful-day-5/) when we introduced our Ubuntu Dock: we want to expose few settings of Dash to Dock to our user base for making them able to slightly customize their default experience. At that time, due to time constraints, we temporarily put those settings in the Display screen. Those settings are a subset of the ones we used to support in the Appearance panel.

![Previous display panel with Ubuntu Dock settings](/images/artful-shell-transition/gnome-control-center-ubuntu-dock-settings.png)

As explained by then, putting them there was neither the best in term of consistency, nor really were the should belong to in the long term. It also forced us to modify (in the ubuntu session only) one of the upstream GNOME Control Center panel, what we didn't want to starting with Artful.

We updated a couple of weeks ago GNOME Shell and most of GNOME components on the 3.25.9x branch in Artful. One of the remaining piece was the Control Center with its revamp UI. It's now done and updated to latest [upstream release](https://launchpad.net/ubuntu/+source/gnome-control-center/1:3.25.92.1-0ubuntu1).

This was a good opportunity to rebase the patch to put them in its own dedicated panel (thanks to [Sébastien](https://blogs.gnome.org/seb128/) bootstrapping that work). This was a little bit more work than just moving those settings as the monitor detection logic changed drastically with that new upstream release. After taking some inputs from design and considering tradeoffs, we came up with the following:

![Ubuntu Dock settings panel](/images/artful-shell-transition/dock_settings.png)

This leaves up space for future improvements. As usual, those changes are only visible in the ubuntu session and the GNOME vanilla one just doesn't display that panel (we reinvigorated a [small patch proposed upstream](https://bugzilla.gnome.org/show_bug.cgi?id=787347) to filter panels per session):

![GNOME Control Center in vanilla session](/images/artful-shell-transition/gnome-control-center-vanilla.png)

And I guess that's just it! You will be able to enjoy in Artful the latest new GNOME Control Center UI which has been [designed and under work](https://blogs.gnome.org/aday/2016/01/13/a-settings-design-update/) for quite long.

Oh, one last thing, I actually lied, we added another setting which has been, I think it's fair to say, quite controversial in the past years. The real panel looks rather like:

![Real ubuntu Dock settings panel](/images/artful-shell-transition/dock_settings_finale.png)

Can you spot the difference? ;)

As every day, if you are eager to experiment with these changes before they migrate to the artful release pocket, you can head over to [our official Ubuntu desktop team transitions ppa](https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/transitions) to get a taste of what's cooking!
