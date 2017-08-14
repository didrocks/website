---
title: "Ubuntu GNOME Shell in Artful: Day 1"
date: 2017-08-14T09:36:25+02:00
tags: [ "pu", "ubuntu" ]
banner: "images/artful-shell-transition/monday-14-august-default.png"
draft: true
type: "post"
---

And here we go! I'm going to report here day by day the progress we are making in ubuntu artful after taking our decisions regarding our default session experience as [discussed in my last blog post](/2017/08/03/ubuntu--guadec-2017-and-plans-for-gnome-shell-migration/).

# Day 1: differentiating ubuntu and GNOME vanilla session

Let's start by the ground work and differentiate our two "ubuntu" and "gnome" sessions. Until today, they were pixel by pixel identical.

Now are migrating in ubuntu artful some [GNOME Session changes](https://launchpad.net/ubuntu/+source/gnome-session/3.24.1-0ubuntu18) as well as some [GNOME Shell ones](https://launchpad.net/ubuntu/+source/gnome-shell/3.24.3-0ubuntu2).

How did we implement that? A little bit before GUADEC, while doing our regular running pratice with [Mathieu](https://mathieu.daitauha.fr)[^1], he mentionned to me the notion of GNOME Shell modes that were used to create the classic session.

After a little bit of digging, this sounded the perfect mechanism we should use to customize our default session, while still shipping an upstream vanilla GNOME session as well.

So, after creating the corresponding environment variables and files to differente the sessions: we now have [an "ubuntu" mode](http://bazaar.launchpad.net/~ubuntu-desktop/gnome-shell/ubuntu/view/head:/debian/ubuntu-session-mods/ubuntu.json), which is enabled in [our default session](http://bazaar.launchpad.net/~ubuntu-desktop/gnome-session/ubuntu/view/head:/debian/patches/50_ubuntu_sessions.patch#L97).

This one refer to an "ubuntu.css" file. To keep it in sync with potential changes in GNOME Shell, this latter is [created at build-time](http://bazaar.launchpad.net/~ubuntu-desktop/gnome-shell/ubuntu/view/head:/debian/rules#L34), from the "gnome-shell.css" vanilla one, based on [some regular expression rules](http://bazaar.launchpad.net/~ubuntu-desktop/gnome-shell/ubuntu/view/head:/debian/ubuntu-session-mods/theme-script.regexp).

So, what changes can you expect with those? Not that much for now in our default session, it should almost be a no-change operation. However, this is the base work to be able to do further changes we'll highlight tomorrow.

After upgrading, the default session will look like this:

![Can you spot the only difference in that screenshot compared to today's session?](/images/artful-shell-transition/monday-14-august-default.png)


Looks very like the current ubuntu session from today's iso, doesn't it? However, if you switch to the GNOME session, after `apt install gnome-session`, you will now see some subtiles differences:
 * the Cantarell font is installed and used inside that GNOME session instead of the ubuntu one. The per-session mode enabled us to drop our distro-wide [patch to enforce the ubuntu font](http://bazaar.launchpad.net/~ubuntu-desktop/gnome-shell/ubuntu/revision/40) in GNOME Shell.
 * symbolic icons are only used in the GNOME vanilla session. Indeed, we decided that consistency was important for our default experience. Our ubuntu mono icons don't have symbolic flavors for most of them. Default ones are looking like the adwaita icon themes and are quite different visually from the ubuntu ones, which can puzzle our users ("Did I launch the application I wanted to? Does this application menu corresponds to the currently focused application?"). But not having symbolic icons for our default theme doesn't justify it by itself, the most important reasons was that most of third-party applications, like firefox, thunderbird, any IDE, and a lot of traditional applications don't ship either a symbolic icon for branding concerns. Consequently, they appear fully colorized. This inconsistency from one application to another isn't visually pleasant, and so, we decided, for 17.10, to disable symbolic icons in the ubuntu session.
 
 The vanilla session is thus showing up those 2 differences for now, as you can see here:

![The new GNOME Vanilla session, looking still quite like the ubuntu one today](/images/artful-shell-transition/monday-14-august-gnome.png)

Finally, we needed to [enforce gdm to use our ubuntu.css theme](http://bazaar.launchpad.net/~ubuntu-desktop/gnome-shell/ubuntu/revision/43) as this is a distribution-wide component, and we'll thus make for all those the "ubuntu look and feel experience".

If you are eager to experiment those changes before they migrate to the artful release pocket, you can head over to [our official ubuntu desktop team transitions ppa](https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/transitions) to get a taste of what's cooking!

> Some people might notice that we didn't use the GNOME Shell per mode overrides (for some mutter behavior like button placement) but we will come with a more scalable and upstream solution that we'll present tomorrow.

This is just the beginning! The differences as quite subtile right now. Tomorrow, the vanilla GNOME session and ubuntu ones will look drastically different. I heard even that some window buttons may change their position…


[^1]: Indeed, GNOME and Ubuntu contributors can have fun, even sometimes having drinks and dinners together. ;)




