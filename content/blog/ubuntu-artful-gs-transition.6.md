---
title: "Ubuntu GNOME Shell in Artful: Day 6"
date: 2017-08-21T14:15:25+02:00
tags: [ "pu", "ubuntu" ]
banner: "images/artful-shell-transition/friday-18-august-default.png"
type: "post"
---

Today's change is hopefully an unnoticeale change for most of you, but gives better security, a smoother and great experience on our journey on transforming the default session in Ubuntu Artful. For more background on this, you can refer back to our decisions regarding our default session experience as [discussed in my blog post](/2017/08/03/ubuntu--guadec-2017-and-plans-for-gnome-shell-migration/).

# Day 6: Wayland is live on Ubuntu by default!

And here we go, the finale big change before we get further along now that our [Feature Freeze](https://wiki.ubuntu.com/ArtfulAardvark/ReleaseSchedule) deadline is approaching. Wayland is now the defaul session for both Ubuntu and vanilla GNOME session thanks to small changes in [gnome-session](https://launchpad.net/ubuntu/+source/gnome-session/3.24.1-0ubuntu22) and [gdm](https://launchpad.net/ubuntu/+source/gdm3/3.25.90.1-0ubuntu2).

After installing them and rebooting, you will hopefully be welcomed with the same screen than last Friday, being automatically transitionned to the Wayland session:

![Both Ubuntu and GNOME default sessions are using Wayland, can you see the difference with the Xorg one?](/images/artful-shell-transition/friday-18-august-default.png)

If you want to easy confirm, you can press `alt+F2`, then `r` and *Enter*, GNOME Shell will notify it can't restart in a wayland session contrary to Xorg.

This allows us to align way more closely to upstream technology, and with our current plan to update a bunch of components to GNOME 3.26 (thanks [Jeremy](https://twitter.com/jbicha) to have already started on this), we are on the road on a full, up to date, closely aligned with upstream choices.

We are fully aware than some applications which are executed by root under the hood aren't compatible with Wayland, as this is a security issue prevented by design. We know as well about the `pkexec` workarounds, but not really found of using them. Some of the well-known applications (which should be updated to use polkit) are for instance *gparted* (but GNOME disks is getting more and more feature-rich with the recent resize and repair changes coming thanks to [Google Summer of Code](https://summerofcode.withgoogle.com/projects/#5348422113558528)), *synaptic* (which we don't install by default), and *ubiquity*, our installer.

As it looks unlikely we'll be able to port gparted nor ubiquity to wayland this cycle, and those are needed to install Ubuntu, the live session (and only live session) is [running on Xorg](https://launchpad.net/ubuntu/+source/casper/1.384) for now. As well, in addition to the option that are available in GDM to select between the Wayland (default) and Xorg session for both Ubuntu and vanilla GNOME for people who want to, the upstream detection mechanism should cover most of the case.

So, what's next? GNOME is currently updated to **3.25.90** as part of our plan for **3.26**. We are also openly discussing on the [Ubuntu desktop mailing list](https://lists.ubuntu.com/archives/ubuntu-desktop/2017-August/thread.html) about the list of applications and components changes that are desired. Globally, the pace of changes is going to slow down as we approach Feature Freeze. However, we are still discussing around some notification/indicator-like support. Indeed, we are thinking about providing a solution working for us while not exposing the whole systray content for some specific cases like Instant Messaging applications (empathy, skype…), emails (thunderbird, evolution…) or sync (dropbox…) applications. Indeed, we think it's an important workflow for most of our users base, especially in light with the [removal of systray support](https://git.gnome.org/browse/gnome-shell/commit/?id=f1ee6c69d74884e294dd5872c73691d5fd2ba09a) in the incoming Shell 3.26. We discussed that heavily (a long couple of hours discussion) during GUADEC and need to settle down on this to see the best approach which is compatible with our common goal: Unity as well as GNOME Shell always had/have the desire to reduce the number of icons there. Our solutions needs to be compatible with future GNOME development and vision.

Finally, look-wise, most of people reported that the shell top panel doesn't look really good with maximized applications using our Ambiance theme, and we agree! The good news is that some members of the desktop team and myself are going to London on Thursday to work with the community on those topics at a [Fit and Finish Sprint](https://insights.ubuntu.com/2017/08/08/ubuntu-artful-desktop-fit-and-finish-sprint/). So, more news on this very very soon!

As every day, if you are eager to experiment with these changes before they migrate to the artful release pocket, you can head over to [our official Ubuntu desktop team transitions ppa](https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/transitions) to get a taste of what's cooking!

I wanted to say thank you to everyone of you commenting on the past days on all those blog posts. It has been a fun ride. We'll keep you posted with further development here. :)

Ubuntu Artful is going to be an exciting release for the desktop!
