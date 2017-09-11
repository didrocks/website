---
title: "Ubuntu GNOME Shell in Artful: Day 11"
date: 2017-09-11T16:00:25+01:00
tags: [ "pu", "ubuntu", "gnome" ]
banner: "images/artful-shell-transition/gdm-upstream.png"
type: "post"
---

Let's talk today about collaboration (with [System76](https://system76.com) in this case) and how we give more benefits to both Ubuntu and the upcoming [Pop! OS](https://system76.com/pop) user base. For more background on our current transition to GNOME Shell in artful, you can refer back to our decisions regarding our default session experience as [discussed in my blog post](/2017/08/03/ubuntu--guadec-2017-and-plans-for-gnome-shell-migration/).


# Day 11: collaboration and GDM theming.

As you probably know by know reading this series of blog posts, Ubuntu artful [introduces a vanilla GNOME session](https://didrocks.fr/2017/08/15/ubuntu-gnome-shell-in-artful-day-2/) type in addition to the Ubuntu style and experience default session. This one can be installed as easily as typing (or using any UI enabling you to install technical packages): `sudo apt install gnome-session`. You can then easily switch, in GDM, between the different kind of sessions and choose the one fitting your personality the best.

However, as I mentioned when talking about [our new Ubuntu Shell theme](https://didrocks.fr/2017/09/04/ubuntu-gnome-shell-in-artful-day-9/), GDM, as being a system-wide component, will keep using our Ubuntu style with no easy way to change it. The theme name is indeed hardcoded in the Shell for good reasons (for instance, there is the fear that user themes, changing the css, may end up being outdated, and potentially can break the Shell and GDM, leaving the user with no UI at all). We were distro-patching this by changing `gnome-shell.css` by our `ubuntu.css` style. It would mean as well people switching to the vanilla session or GNOME classic had no way (apart from recompiling) to change the current GDM theme.

![Default Login screen (GDM) in Ubuntu Artful](/images/artful-shell-transition/gdm-welcome.png)

Enters then [Jeremy](https://twitter.com/jeremy_soller) from System76. As Pop! OS is an Ubuntu derivative, we didn't know they were impacted by those changes (their packages, even the theme ones, aren't in the Ubuntu archive), and we couldn't audit this. After a little bit of back and forth so that each party understands the other's perspective in term of maintenance cost and path forward, we agreed that the long term plan is to work upstream to give distributors and vendors this possibility (where vendors are responsible of what they ship by default, and should then ensure their themes are compatible with the Shell version they ship with, contrary to random user-installed theme on the Internet). Jeremy did thus file [a bug upstream and I detailed the need and perimeter of what we want to allow a little bit more](https://bugzilla.gnome.org/show_bug.cgi?id=787454).

However, System76 needed a solution for their upcoming release of their OS based on Ubuntu 17.10. We thus accepted and sponsored [their patch to use an alternative for now](https://launchpad.net/ubuntu/+source/gnome-shell/3.25.91-0ubuntu4). That enables them to avoid forking the GNOME Shell ubuntu package to set their GDM system theme. Nothing changes for our users (just a little bit of maintenance overhead for us, the ubuntu desktop team).

The next morning, I started to think back about all that and our vanilla session. I thus decided that it was a good opportunity to let advanced users, who wants to stick fully with the vanilla default GNOME experience, take advantage of this. Another alternative pointing to upstream css (with lower priority than our default one) [is thus now in artful](https://launchpad.net/ubuntu/+source/gnome-session/3.25.90-0ubuntu3).

It means that if you install the `gnome-session` package and want the upstream GDM look as well, you can switch to it by issuing:

```
sudo update-alternatives --config gdm3.css
There are 2 choices for the alternative gdm3.css (providing /usr/share/gnome-shell/theme/gdm3.css).

  Selection    Path                                          Priority   Status
------------------------------------------------------------
* 0            /usr/share/gnome-shell/theme/ubuntu.css        10        auto mode
  1            /usr/share/gnome-shell/theme/gnome-shell.css   5         manual mode
  2            /usr/share/gnome-shell/theme/ubuntu.css        10        manual mode

Press <enter> to keep the current choice[*], or type selection number:
```

Press 1 (in this case), and then [Enter]. The next reboot (actually, the next GDM restart) will show up the upstream GDM color scheme:

![Upstream GDM color scheme](/images/artful-shell-transition/gdm-upstream.png)


If you want to revert to the Ubuntu's defaults, just issue a `sudo update-alternatives --auto gdm3.css`. Of course, those commands are for advanced users. I'm sure that 3rd party tweaking tool will pick up this and make it available if people choose the whole GNOME Vanilla experience.

As every day, if you are eager to experiment with these changes before they migrate to the artful release pocket, you can head over to [our official Ubuntu desktop team transitions ppa](https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/transitions) to get a taste of what's cooking!

It's great to see that we can collaborate with other companies, still hoping we can push a better technological design for this directly upstream (and allow GNOME classic as well having its own theme that way), and deliver through that cooperation a better controlled experience for our advanced user base.
