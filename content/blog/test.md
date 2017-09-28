---
title: "Test blog post"
date: 2017-09-26T14:21:21-04:00
tags: []
banner: "images/artful-shell-transition/ubuntu-dock-count.png"
type: "post"
discourse: true
draft: true
---


This is a test blog post for discourse integration. Not listed on any tag on purpose.

The Ubuntu desktop team and a lot other people from the Ubuntu community are gathering for the week in New York for the Ubuntu Rally. It's time to get the final touch and bug fixes for Ubuntu artful which is turning itself soon into Ubuntu 17.10. As you probably know if you follow this blog series, it will feature GNOME Shell by default, with slight modifications to ease and adapt to our audience for this new user experience. For more background on our current transition to GNOME Shell in artful, you can refer back to our decisions regarding our default session experience as [discussed in my blog post](/2017/08/03/ubuntu--guadec-2017-and-plans-for-gnome-shell-migration/).

# Day 14: Badges and progress bar on Ubuntu Dock

One of the latest thing we wanted to work on as we highlighted on our previous posts is the notification for new emails or download experience on the Shell. We already do ship the [KStatusNotifier extension](/2017/08/23/ubuntu-gnome-shell-in-artful-day-7/) for application indicator, but need a way to signal the user (even if you are not looking at the screen when this happens) for new emails, IM or download/copy progress.

[Andrea](https://plus.google.com/u/0/+AndreaAzzarone) stepped up on this and [worked with Dash to Dock upstream](https://github.com/micheleg/dash-to-dock/pull/590) to implement the unity API for this. Working with them, as usual, was pleasing and we got the green flag that it's going to merge to master, with possibly some tweaks, which will make this work available to every Dash to Dock users! It means that after [this update](https://launchpad.net/ubuntu/+source/gnome-shell-extension-ubuntu-dock/0.6), Thunderbird is handily showing the number of unread emails you have in your inbox, thanks to thunderbird-gnome-support that [we seeded back](https://launchpad.net/ubuntu/+source/ubuntu-meta/1.401) with [Sébastien](https://blogs.gnome.org/seb128/).

![Ubuntu Dock with number of unread emails](/images/artful-shell-transition/ubuntu-dock-count.png)

Similarly, we now have progress bar support on Nautilus, Firefox downloads and every applications using that API to get updated on transactional actions.

![Ubuntu Dock with progress bar during a download with Firefox](/images/artful-shell-transition/ubuntu-dock-progress-bar.png)

