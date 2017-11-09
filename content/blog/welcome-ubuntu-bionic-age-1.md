---
title: "Welcome To The (Ubuntu) Bionic Age: A new ubuntu default theme, call for participation!"
date: 2017-11-09T15:36:19+01:00
tags: [ "pu", "ubuntu", "gnome" ]
banner: "/images/artful-shell-transition/final_freeze_ubuntu_17.04_desktop.png"
type: "post"
manualdiscourse: "https://community.ubuntu.com/t/call-for-participation-an-ubuntu-default-theme-lead-by-the-community/1545"
---

# Call for participation: an ubuntu default theme lead by the community?

As part of our Unity 7 to GNOME Shell transition in 17.10, we had last August a [Fit and Finish Sprint](https://insights.ubuntu.com/2017/08/08/ubuntu-artful-desktop-fit-and-finish-sprint/?_ga=2.124355530.1963937614.1509953008-1187013618.1399019970) at the London office to get the Shell feeling more like Ubuntu and we added some tweaks to our default GTK theme.

The outcome can be seen in the following posts:

* https://didrocks.fr/2017/08/25/ubuntu-gnome-shell-in-artful-day-8
* https://didrocks.fr/2017/09/04/ubuntu-gnome-shell-in-artful-day-9
* https://didrocks.fr/2017/10/16/ubuntu-gnome-shell-in-artful-day-15

Some more refinements came in afterward and finale 17.10 has a slightly modified look, but the general feedback we got from the community is that the ubuntu GNOME Shell session really looks and feels like ubuntu.

So I guess, objective completed (next level, attribute your earned point to person’s skills… :p).

![Default ubuntu 17.10 desktop](/images/artful-shell-transition/final_freeze_ubuntu_17.04_desktop.png)

## All done?

However, as in any good RPG, this isn’t the real end of the story (there is the next boss!): we heard as well some people (on [the community hub](https://community.ubuntu.com), in blog post comments) asking for a more drastic refresh of our theme and we generally agree with this. That would be a good idea to rebase and refresh our desktop with the help of the community!

For any themes, there are multiple parts:

* The Shell theme itself (css).
* The GTK3 and GTK 2 theme. The first one is using css, the second is some C code.
* An icon theme.

Here is thus a call for participation if you are interesting into getting into that journey with us. The idea is to have few people (I think, 2-3 people + [Alan Pope](https://popey.com/) and I), having contributed to popular shell or GTK themes already, leading this project. That way, we can define what changes to the theme feels like “ubuntu” or not. We will coordinate all the work on the [community hub](https://community.ubuntu.com) to ensure that every decision is public and explain why.

We will sync regularly with the Canonical design theme (we have one meeting at the end of the month already) to check progress and get advice. Once the theme for the Shell, and GTK bindings are ready, we’ll switch the default ubuntu to it. That may or may not happen for the LTS, depending on the advancement. If it’s not ready to be switched by default, we will thus give instructions for our advanced users to get a taste of what’s currently cooking up :)

## How do we get that going?

The idea is to restart from scratch, basing on upstream (GNOME) work. Indeed, the current ubuntu theme is in pure css, upstream is using sass. The Shell theme itself didn’t deviate much, but the general idea is:

for GTK3 apps, starting from Adwaita (default GNOME theme), and modify constants and slight behavior modifications in the sass files. That way, we don’t deviate too much and it will be easy to rebase with the numerous theme changes every new GTK release gets.
for the Shell, using their sass files and tweaking for the same reasons.
for the icon theme, we might start from our unity8 Suru icon set?
Also, even if we hope that contributors from popular united and other themes will come along, the fact to start back from upstream’s playground provide a nice neutral and clean ground, we didn’t prevent from cherry-picking from what exists already. :)

## Come with us!

Anyone can contribute (preferably via pull request on the projects we will created), however, in design, it’s always easier to have ideas than coming with concrete technical help ;). This is why there is this call to get some ideas of the number of people willing to spend some time on this project.
The needs skills are either CSS (we’ll use SASS, more on that later) or C GTK theming. Also, Icon designers are more than welcome. :)

Excited? Join us! If you are interested (in either leading or just contributing), please post on [this community hub topic](https://community.ubuntu.com/t/call-for-participation-an-ubuntu-default-theme-lead-by-the-community/1545) your intents and ideas. Also, please reference your technical skills so that we can get an idea on the amount of awesome help we’ll get!

We’ll of course post more info on the same desktop section once we are ready to kick in the project! Hope, you are as thrilled by this project as we all are. ;)
