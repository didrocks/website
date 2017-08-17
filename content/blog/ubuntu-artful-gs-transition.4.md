---
title: "Ubuntu GNOME Shell in Artful: Day 4"
date: 2017-08-17T10:35:25+02:00
tags: [ "pu", "ubuntu" ]
banner: "images/artful-shell-transition/thursday-17-august-unity-session.png"
type: "post"
---

Let's continue on our journey on transforming the current default session in Ubuntu Artful with a small change today. For more background on this, you can refer back to our decisions regarding our default session experience as [discussed in my blog post](/2017/08/03/ubuntu--guadec-2017-and-plans-for-gnome-shell-migration/).

# Day 4: Status icons and multiple sessions

Let's focus today on a very small detail, but that still show up we are looking at those nitpicks before delivering ubuntu 17.10.

As I teased yesterday, can you spot what's wrong in that picture?

![Today's ubuntu session, before "ZE" change](/images/artful-shell-transition/thursday-17-august-ubuntu-before.png)

Sure, there is some french involved, but I wasn't talking about that. :) What's happening is that the battery icon is squished on a non HiDPI display. This is coming from the fact that the icon isn't square, and GNOME-Shell is expecting square icons, contrary to our Unity indicator power implementation. Consequently, our battery icons from our ubuntu-mono icon theme aren't compatible in their current form with the Shell.

As we can't separate easily the icon theme, and this one is containing a bunch of other monochrome icons we want to use by default in our ubuntu session for various applications, the "simple" fix was to prepend [those special icons with *unity-*](http://bazaar.launchpad.net/~ubuntu-art-pkg/ubuntu-themes/trunk/revision/554) and having only [indicator-power reading](http://bazaar.launchpad.net/~indicator-applet-developers/indicator-power/trunk.16.10/revision/313) them, still being backward compatible as Ubuntu MATE is using this indicator with their own theme. (Actually, I had to quickly write [a small script](https://gist.github.com/didrocks/1fdbe995b2694c78d1ee37f6ce77d85e) to rename everything in batch, as there is a bunch of symlinks in between of all those files).

The result is that now both the ubuntu and GNOME vanilla session shows up the upstream battery icon, alongside other default status icons:

![Vanilla upstream GNOME session with correct battery icon](/images/artful-shell-transition/thursday-17-august-after.png)

And our unity session (available in universe) will display our desired set of icons:
![Unity session in artful](/images/artful-shell-transition/thursday-17-august-unity-session.png)

Even if this seems a trivial issue, attention to details is always important and a crucial value we always have. Especially if we can, easily, prevent breakage on other part of the distribution and archive.

As every day, if you are eager to experiment with these changes before they migrate to the artful release pocket, you can head over to [our official Ubuntu desktop team transitions ppa](https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/transitions) to get a taste of what's cooking!

And we'll attack tomorrow the big piece: introducing the new Ubuntu Dock!
