---
title: "Ubuntu GNOME Shell in Artful: Day 13"
date: 2017-09-20T12:25:25+01:00
tags: [ "pu", "ubuntu", "gnome" ]
banner: "images/artful-shell-transition/dock-dynamic-transparency.png"
type: "post"
---

Now that GNOME 3.26 is released, available in Ubuntu artful, and final GNOME Shell UI is confirmed, it's time to adapt our default user experience to it. Let's discuss how we worked with dash to dock upstream on the transparency feature. For more background on our current transition to GNOME Shell in artful, you can refer back to our decisions regarding our default session experience as [discussed in my blog post](/2017/08/03/ubuntu--guadec-2017-and-plans-for-gnome-shell-migration/).

# Day 13: Adaptive transparency for Ubuntu Dock

GNOME Shell 3.26 excellent new release ships thus some dynamic panel transparency by default. If no window is next to the top panel, the bar is itself is translucent. If any windows is next to it, the panel becomes opaque. This feature is highlighted on the [GNOME 3.26 release note](https://help.gnome.org/misc/release-notes/3.26/). As we already discussed [this on a previous blog post](https://didrocks.fr/2017/09/04/ubuntu-gnome-shell-in-artful-day-9/), it means that the Ubuntu Dock default opacity level doesn't fit very well with the transparent top panel on an empty desktop.

![Previous default Ubuntu Dock transparency](/images/artful-shell-transition/new-theme-main-view.png)

Even if there were [some discussions](https://bugzilla.gnome.org/show_bug.cgi?id=787446) within GNOME about keeping or reverting this dynamic transparency feature, we reached out the Dash to Dock guys during the 3.25.9x period to be prepared. Started then some excellent discussions on the [pull request](https://github.com/micheleg/dash-to-dock/pull/513) which was already rolling full speed ahead.

The first idea was to have dynamic transparency. Having one status for the top panel, and another one for the Dock itself. However, this gives some weirds user experience after playing with it a little bit:

{{< youtube HTaF1zMuGGk >}}

We can feel there are too much flickering, having both parts of the UI behaving independently. The idea I raised upstream was thus to consider all Shell UI (which is, in the Ubuntu session the top panel and Ubuntu Dock) as a single entity. Their opacity status is thus linked, as one UI element. [François](https://github.com/franglais125) agreed and had the same idea on his mind, before implementing it. The results is way more natural:

{{< youtube 0MpTnqj6mqs >}}

Those options are implemented as options in Dash to Dock settings panel, and we just set this last behavior as the default in Ubuntu Dock.

We made sure that this option is working well with the various dock settings we expose in the Settings application:

{{< youtube eEMSrQCAteg >}}

In particular, you can see that intelli-hide is working as expected: dock opacity changing while the Dock is vanishing and when forcing it to show up again, it's at the maximum opacity that we set.

The default with no application next to the panel or dock is now giving some good outlook:

![Default empty Ubuntu artful desktop](/images/artful-shell-transition/dock-dynamic-transparency.png)

The best part is the following: as we are getting closer to release and there is still a little bit of work upstream to get everything merged in Dash to Dock itself for options and settings UI which doesn't impact Ubuntu Dock, [Michele has prepared](https://github.com/micheleg/dash-to-dock/pull/513#issuecomment-330391097) a [cleaned up branch](https://github.com/micheleg/dash-to-dock/commits/development) that we can cherry-pick from directly in our ubuntu-dock branch that they will keep compatible with master for us! Now that the [Feature Freeze and UI Freeze exceptions](https://bugs.launchpad.net/ubuntu/+source/gnome-shell-extension-ubuntu-dock/+bug/1717509) have been approved, the [Ubuntu Dock package](https://launchpad.net/ubuntu/+source/gnome-shell-extension-ubuntu-dock/0.5) is currently building in the artful repository alongside other fixes and some shortcuts improvements.

As usual, if you are eager to experiment with these changes before they migrate to the artful release pocket, you can head over to [our official Ubuntu desktop team transitions ppa](https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/transitions) to get a taste of what's cooking!

It's really a pleasure to work with Dash to Dock upstream, I'm using this blog opportunity to thank them again for everything they do and the cooperation they ease out for our use case.
