---
title: "Open The Cosmic Gate: A beautiful theme gets a beautiful name"
date: 2018-07-24T10:10:19+01:00
tags: [ "pu", "ubuntu", "gnome" ]
banner: "/images/cosmic/yaru.svg"
type: "post"
manualdiscourse: "https://community.ubuntu.com/t/call-for-participation-an-ubuntu-default-theme-lead-by-the-community/1545"
---

# A beautiful theme gets a beautiful name

Communitheme has been a community effort from the start with an overwhelming amount of feedback from an even larger community. Surprisingly, the still ongoing discussion thread of more than [1500 messages](https://community.ubuntu.com/t/call-for-participation-an-ubuntu-default-theme-lead-by-the-community/1545) hasn’t (yet?) broken discourse!

However, the effort to refresh the look and feel of Ubuntu has gone way beyond just a theme. From the start, Sam Hewitt’s [beautiful Suru icons](https://snwh.org/suru) were included and over time, the effort brought new system sounds and new cursors under its wing. Some of the design discussions have gone even further than this, but the desire to stay as close to upstream GNOME as possible has put most of those in the freezer for now. So, in order to reflect the broad scope and in light of its upcoming inclusion in Ubuntu, a new name is in order.

After 8 months of intense labour, we are proud to announce the birth of **Yaru**!

![Yaru vs Suru](https://kawakawalearningstudio.com/wp-content/uploads/2017/08/img_023_YaruAndSuru-cover-e1505891170257-1024x580.jpg)

A fully community grown theme, ready to look good and be awesome. Yaru continues on the Japanese influences of Suru, and its meaning, "to do" or "to give" fits perfectly with this project: Yaru is here because we did it, we’re happy to give it to you to spread Ubuntu’s culture of sharing, and we hope it helps you do cool stuff on Ubuntu. Best of all, even the name was vetted in by the community! [A poll](https://community.ubuntu.com/t/new-communitheme-name-poll/6693) confirmed that this name is widely loved by the entire Communitheme-community. A longer explanation of Yaru vs Suru could be [found  here](https://kawakawalearningstudio.com/all/different-ways-of-using-%E3%82%84%E3%82%8B-yaru-and-%E3%81%99%E3%82%8B-suru/).

## We did not do it in a day..

Communitheme project got immediately big expectations from the community. Many people were eagerly awaiting this style refresh and wanted it as default theme in the 18.04 LTS (codename Bionic Beaver) release. However, we decided to [postpone its release](https://didrocks.fr/2018/04/10/welcome-to-the-ubuntu-bionic-age-new-wip-ubuntu-theme-as-a-snap/) to give us the freedom to keep changing the theme, since an LTS would mean that the theme’s look and feel is fixed for a few years. Two months after the Bionic release, looking at the [commit activity](https://github.com/ubuntu/gtk-communitheme/graphs/commit-activity) and at the list of [pull requests](https://github.com/ubuntu/gtk-communitheme/pulls?utf8=%E2%9C%93&q=is%3Apr+is%3Aclosed+is%3Amerged), it’s clear this was the right decision. We have gone through several iterations that affected also the very basic elements of style.

![GNOME Shell theme](/images/cosmic/gnome-shell-example.png)

 * The color & shape of our button sets changed to look bright, sharp and elegant.
 * The colors for the window and sidebar background are changed to a more warm and welcoming tone, like we did for the headerbars at the beginning.
 * We abandoned the strong orange for the text selection and changed it to a more discrete blue.
 * We changed the Color & shape of GNOME and GTK notifications so that they pop up nicer from the background.
 * Finally, many changes were made to the transparency, borders, shadows, colors and depth effects so GNOME Shell looks like something in between Unity7, Unity8 and the new design ideas.

The use of flat design was also discussed thoroughly, because it is very common nowadays. Flat UI is less distracting and gives an uncluttered and sharp look, but it can also be boring and decrease the UX. We decided to mix both styles: the contours, GNOME shell and the headerbars are flat and the applications themselves in the center have a gentle 3D effect to better highlight where the focus should be.

![GTK theme](/images/cosmic/gtk-theme-example.png)

Those themes are based on both upstream [GNOME Shell](https://gitlab.gnome.org/GNOME/gnome-shell/data/theme/) and [Adwaita](https://gitlab.gnome.org/GNOME/gtk/gtk/theme/Adwaita) themes sass files, making the whole maintenance way easier.

## We did not do it alone…

We sincerely want to thank for all the feedback, ideas, PRs and also testing and reports the whole community, just to name a few: ya-d, jaggers, yazub, NusiNusi, nana-4, CraigD, vinceliuce, Paz-it, mivoligo, taciturasa. Without their huge support we surely would not have gone this far in so little time, and of course we want to thank the Design and Development team [Stefan Eduard Krenn](/2018/04/16/welcome-to-the-ubuntu-bionic-age-behind-communitheme-interviewing-stefan-eduard/), [Carlo Lobrano](/2018/04/17/welcome-to-the-ubuntu-bionic-age-behind-communitheme-interviewing-carlo/), [Mads Rosendahl](/2018/04/18/welcome-to-the-ubuntu-bionic-age-behind-communitheme-interviewing-mads/), [Frederik Feichtmeier](/2018/04/19/welcome-to-the-ubuntu-bionic-age-behind-communitheme-interviewing-frederik/), [Merlijn Sebrechts](/2018/04/24/welcome-to-the-ubuntu-bionic-age-behind-communitheme-interviewing-merlijn/), [Aaron Papin](/2018/04/25/welcome-to-the-ubuntu-bionic-age-behind-communitheme-interviewing-aaron/), whose constant effort and professionalism shaped Yaru theme commit after commit and discussion after discussion since the very beginning of this awesome journey.

## What will happen in the coming days?

If you are one of the 19 000 people who downloaded the communitheme snap on ubuntu 18.04 LTS, basically nothing will change for you and you will still get the regular flow of daily (commitly? ;)) or weekly updates depending on which channel you have chosen ! We made a good deal in keeping backward compatibility for this user base. Snaps can’t be renamed yet, and consequently, we decided on keeping "communitheme" codename for this version. You still get latest of latest, and the [build system](https://github.com/ubuntu/communitheme/blob/master/snap/snapcraft.yaml#L23) has now some tweaks to ensure you get a compatible version with your system. You will still log into your communitheme dedicated session.

We are going to transition Cosmic (incoming 18.10 Ubuntu release) very soon to use a newly set of distribution packages under the Yaru new name. The new package will enter in the coming days to the ubuntu archive and the default ubuntu session will switch to it soon (once we get the package in main and makes some changes in various projects and default settings)! It won’t get as many refresh cycle as the snap based version, but we'll make regular snapshots. Please use the snap if you want to give continuous feedback on the [ubuntu hub](https://community.ubuntu.com) with its dedicated section  or or directly install from source.

Speaking of installing from source, we [merged last week](https://github.com/ubuntu/communitheme/pull/628) our different repositories (5 of them) [into a single one](https://community.ubuntu.com/t/merging-repositories-into-a-single-one/6769) to ease maintenance and releases. Now, we can get very easily the “Yaru” experience (GTK2, GTK3, GNOME Shell, icon, cursor and sound themes), cloning a single git repository and installing from it!

## Eager to help?

Note that screenshots are still Work In Progress, there is still some discussions about keeping the Ubuntu logo by default
on the launcher or not and other fundamentals changes that the community can decide until the [Cosmic Cuttlefish release](https://wiki.ubuntu.com/CosmicCuttlefish/ReleaseSchedule).

We still need some helps, in particular in the GTK2 world (which will be used to provide theming for Qt applications as well). It has never been easier to contribute to Yaru thanks to the recent repository reorganization: contributing to the projects is now simply heading to the [Yaru repository](https://github.com/ubuntu/communitheme) under the ubuntu organization, read the [README](https://github.com/ubuntu/communitheme/blob/master/README.md) and [contributing guidelines](https://github.com/ubuntu/communitheme/blob/master/CONTRIBUTING.md). All coordination still goes through the community ubuntu HUB and [its dedicated topic](https://community.ubuntu.com/t/call-for-participation-an-ubuntu-default-theme-lead-by-the-community/1545/923). Will you be the next one? :)

Didier - on behalf of the whole communitheme core contributor team who contributed to this announce.
