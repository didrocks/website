---
title: "Ubuntu GNOME Shell in Artful: Day 2"
date: 2017-08-15T10:57:25+02:00
tags: [ "pu", "ubuntu", "gnome" ]
banner: "images/artful-shell-transition/tuesday-15-august-gnome.png"
type: "post"
---

Let's continue our journey and progress on transforming current Ubuntu Artful. For more background on this, you can refer back to our decisions regarding our default session experience as [discussed in my blog post](/2017/08/03/ubuntu--guadec-2017-and-plans-for-gnome-shell-migration/).

# Day 2: and the GNOME vanilla session rises…

Today is about differentiating our Ubuntu and GNOME vanilla session to provide different defaults per session.

Remember that the GNOME "vanilla" session yesterday was looking like this:
![GNOME Vanilla session, as per Monday 14th of August 2017](/images/artful-shell-transition/monday-14-august-gnome.png)

After upgrading today, there is a whole new layout, very close to a pure upstream GNOME default look:
![GNOME Vanilla session, as per Tuesday 15th of August 2017](/images/artful-shell-transition/tuesday-15-august-gnome.png)

![GNOME Vanilla session in overview mode, as per Tuesday 15th of August 2017](/images/artful-shell-transition/tuesday-15-august-gnome-overview.png)

What can we notice?
* The GNOME vanilla session have a different set of icons and GTK themes
* Icon default size are drastically bigger
* The desktop doesn't show any icons by default
* There is only one window button (the close icon), on the right
* What you can't see is that even the cursor icon is different
* As yesterday, this GNOME session still has different font and symbolic icons

A lot of other noticeable changes that we do in ubuntu (welcoming window disabled in GNOME software, File chooser dialog always opening in current directory instead of recommended, default set of plugins in some softwares like rhythmbox, power settings) are reset to their default in that particular session.

Remember that this session isn't installed by default but is available via a simple `apt install gnome-session`. It has never been easier in Ubuntu to taste something really close to upstream vision!

Let's now look at the Ubuntu default session:

![Ubuntu session after per desktop overrides](/images/artful-shell-transition/tuesday-15-august-default.png)

![Ubuntu session overview](/images/artful-shell-transition/tuesday-15-august-default-overview.png)

Not that many visible changes: we still have icons visible on the desktop in that session. However, the trash (or bin, take the flavor you prefer ;)) icon is available back on the desktop as our dock won't include. The more observers will notice that there are 3 window buttons (minimize, maximize and close, which makes more sense having this incoming dock in that session), on the right, as we announced some days ago! 

How did we implement that? All of this magic relies on a very recent glib feature [reported here](https://bugzilla.gnome.org/show_bug.cgi?id=746592). This one isn't available yet upstream, but we got the blessing by [Allison](https://blogs.gnome.org/desrt/) (upstream glib developer) to take it as is for now. We this [uploaded it as a distro-patch](https://launchpad.net/ubuntu/+source/glib2.0/2.53.4-3ubuntu1). However, this change going to be merged soon in glib master, once more regression tests are written. [Alberts](https://launchpad.net/~muktupavels) implemented a per session gsettings override, which enables having different default per desktop name. Allison cleaned it up a little bit while we were discussing about it at GUADEC and Alberts fixed some remaining bugs! Thanks to both of them for helping us proactively delivering this!

What remained to be done was to [add new desktop names](http://launchpadlibrarian.net/333296311/gnome-session_3.24.1-0ubuntu18_3.24.1-0ubuntu19.diff.gz): our default ubuntu session has `XDG_CURRENT_DESKTOP=ubuntu:GNOME` while the vanilla GNOME one has just `XDG_CURRENT_DESKTOP=GNOME`. Then, reorganizing the exiting override (after a lot of [cleanup](http://bazaar.launchpad.net/~ubuntu-desktop/+junk/ubuntu-settings/revision/98), removing redundancies [across packages](http://launchpadlibrarian.net/333296844/gtk+3.0_3.22.17-0ubuntu1_3.22.17-0ubuntu2.diff.gz)) to separate between "[global distribution-wide overrides](http://bazaar.launchpad.net/~ubuntu-desktop/+junk/ubuntu-settings/view/head:/debian/ubuntu-settings.gsettings-override#L1)" that we want globally available and [per-desktop ones](http://bazaar.launchpad.net/~ubuntu-desktop/+junk/ubuntu-settings/view/head:/debian/ubuntu-settings.gsettings-override#L46) was easy. The idea is really to only impact our sessions when we want to have different behavior than the default upstream one, without impacting the other session.

So, what about Unity you will say? Remember that people upgrading will still have the "unity" session available (it won't be the default session and people will be transitionned to our newdefault ubuntu one using GNOME Shell), and people installing from scratch can still `apt install unity` if they want to have this unity session available in their display manager.

![The unity session](/images/artful-shell-transition/tuesday-15-august-unity.png)

Well, buttons are on the left, as people might expect with the unity experience and the trash icon isn't displayed on the desktop but still on the Unity launcher. This is thanks to Unity now defining `XDG_CURRENT_DESKTOP=Unity:Unity7:ubuntu`, still having access to the *ubuntu* overrides, but having the [*Unity* ones](http://bazaar.launchpad.net/~ubuntu-desktop/+junk/ubuntu-settings/view/head:/debian/ubuntu-settings.gsettings-override#L129) taking priorities, like button positions, icons to display on the desktop, some shorcuts…

We had similarly warned the Ubuntu MATE team, as those changes was impacting them too, so that they can prepare for it.

Of course, all this is only overriding *defaults*. It means that if you change one of those settings (like chosen theme or settings in an application), this is an user change. As an user change is more important than defaults, it will impact all sessions. Indeed, it would feel weird to have some keys impacting only some sessions, and other like your social accounts, where you definitively want them to be system-wide. The more robust and understandable behavior is thus to say that any user action changing a setting will be available whatever desktop the user is on.

We think that this schema is an improvement over the current override system in GNOME Shell. Indeed, this one is only available on a [few hardcoded GNOME shell modes](https://git.gnome.org/browse/gnome-shell/tree/src/shell-global.c#n1489), and so, we would have needed to distro-patch to add our own "mode". Furthermore, it can [only impact some defined mutter keys](https://git.gnome.org/browse/gnome-shell-extensions/tree/data/org.gnome.shell.extensions.classic-overrides.gschema.xml), like button position, modal dialog behavior… Consequentely, it can be easily go out of sync with current mutter schema if this one changes. One of the keys that can't be overriden is, for instance, the default GTK theme.
That's why I'm proposing having a look at migrating the upstream "GNOME classic" (GNOME 2-like experience under GNOME Shell) session to this system, and replace thus the GNOME Shell current override system, in the near future, once the Glib change is merged in (probably in GNOME next cycle).

As yesterday, if you are eager to experiment with these changes before they migrate to the artful release pocket, you can head over to [our official ubuntu desktop team transitions ppa](https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/transitions) to get a taste of what's cooking! Happy testing and playing on those sessions!

I hadn't [lied to you yesterday](https://didrocks.fr/2017/08/14/ubuntu-gnome-shell-in-artful-day-1/), the vanilla GNOME session and Ubuntu ones are now looking and behaving drastically different! Tomorrow, I'll show how we implement back some desired behavior changes while still impacting only the ubuntu session, leaving the upstream behavior in the GNOME vanilla session.
