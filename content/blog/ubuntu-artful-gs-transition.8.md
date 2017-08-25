---
title: "Ubuntu GNOME Shell in Artful: Day 8"
date: 2017-08-25T15:45:25+01:00
tags: [ "pu", "ubuntu" ]
banner: "images/artful-shell-transition/new_ubuntu_theme_preview.png"
type: "post"
---


A short blog post today to tease a little bit about our future theme changes that are going to land in Ubuntu Artful as we are transforming our default session. For more background on this, you can refer back to our decisions regarding our default session experience as [discussed in my blog post](/2017/08/03/ubuntu--guadec-2017-and-plans-for-gnome-shell-migration/).

# Day 8: Fit and Finish sprint (early) report

As I'm in the Eurostar[^1] heading back home from the [Fit and Finish Sprint](https://insights.ubuntu.com/2017/08/08/ubuntu-artful-desktop-fit-and-finish-sprint/), let me quickly write up how excellent this session has been!

Once we got over some physical dimmed screen display issue and Will encouraging me to fix this using manual and antic technology, we fought over some cables dilemma to display our default GNOME Shell theme. We then have been able to project it and incrementing all together over it.

![Which cable to use? Too many choices](/images/artful-shell-transition/cables-of-hell.jpg)

We thus made great progress on theming: from the GDM user log in, to the lock screen UI and in the main Shell theme as well, debating which options are the best to match our ubuntu values and default theme.

Here is a little teaser:

![Maybe the artful ubuntu GNOME Shell theme?](/images/artful-shell-transition/new_ubuntu_theme_preview.png)

Note we are not totally decided on it yet, as there is a counter proposal while we were iterate from a certain [Alan P.](https://twitter.com/popey) who prefers to remain anonymous:

![We should have done a poll to compare the two proposals popularity](/images/artful-shell-transition/another_proposal_for_ubuntu_theme.png)

Those themes are only compatible for Ambiance right now, and are based on GNOME Shell 2.24. We will rebase it on 2.26 once uploaded to the distribution (probably on 3.25.91).

We also got a bunch of other [bug fixes around the GTK theme itself and many design discussions](https://trello.com/c/Cpt4wwRw/204-london-fit-and-finish-hackfest) around various Shell desired behaviors. As you can see, the list was quite large!

You will notice that for once, not all those changes are available right away in artful. Indeed, the initial preparation work is done. However, we'll need a little bit more time to make a sanitized and maintainable over time across Shell versions support. Also, some rest after this recently very busy time (as you probably have followed over the course of those past weeks via those blog posts) is definitively needed! Consequently, please don't expect the change to show up before a good week, probably on the following Monday. That let as well other desktop team members to update most components to the new shiny 3.25.91 version of GNOME, continuing our polishing work, so you are not alone and will have many changes to test and play with in current Ubuntu Artful. ;)

[^1]: I must say there is something mindblowing publishing a blog post when being in the chunnel
