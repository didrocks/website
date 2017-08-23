---
title: "Ubuntu GNOME Shell in Artful: Day 7"
date: 2017-08-23T09:24:25+02:00
tags: [ "pu", "ubuntu" ]
banner: "images/artful-shell-transition/keylock-indicator.png"
type: "post"
---


Today's change will be about one of our last transformation (non visual but in term of feature) on our journey on transforming the default session in Ubuntu Artful. For more background on this, you can refer back to our decisions regarding our default session experience as [discussed in my blog post](/2017/08/03/ubuntu--guadec-2017-and-plans-for-gnome-shell-migration/).

# Day 7: Application indicator support in the ubuntu session

As mentioned in day 6, we discussed about indicators. Indeed, we are thinking about providing a solution working for us while not exposing the whole systray content for some specific cases like Instant Messaging applications (empathy, skype…), emails (thunderbird, evolution…) or sync (dropbox…) applications. Indeed, we think it's an important workflow for most of our users base, especially in light with the [removal of systray support](https://git.gnome.org/browse/gnome-shell/commit/?id=f1ee6c69d74884e294dd5872c73691d5fd2ba09a) in the incoming Shell 3.26. We discussed that heavily (a long couple of hours discussion) during GUADEC and need to settle down on this to see the best approach which is compatible with our common goal: Unity as well as GNOME Shell always had/have the desire to reduce the number of icons there.

This is something that we rather achieved well with our indicator support. It sounds that the user experience and thanks to our user testing results at the time, got quite some traction and adoption in various upstream projects. We should bring that back in the new ubuntu session for our user's journey. We thus reviewed existing extensions and found the [KStatusNotifierItem/AppIndicator Support](https://extensions.gnome.org/extension/615/appindicator-support/) one. KStatusNotifierItem? Remember that the application indicator specification was mostly taken by KDE as well, hence the name. This kind of experience is particularly important for some use cases as we explained before. However, we identified that some applications (like dropbox or Skype) don’t work yet. Part of the issue for electron apps is that the [chromium framework is harcoding](https://chromium.googlesource.com/chromium/src/+/master/chrome/browser/ui/libgtkui/app_indicator_icon.cc#98) the `Unity` session name in `XDG_CURRENT_DESKTOP`. However, this is even invalid as of today in the unity session as its value is `Unity:Unity7` (this environment variable is a list in the specification) since Ubuntu 16.10. We are thus working to push a proper fix upstream, and will reach to those application developers to rebuild their applications to get the indicator showing up in the incoming artful release.

After installing [latest](https://launchpad.net/ubuntu/+source/ubuntu-meta/1.397) [set of](https://launchpad.net/ubuntu/+source/gnome-shell-extension-appindicator) [updates](https://launchpad.net/ubuntu/+source/gnome-shell/3.24.3-0ubuntu7) and login out/in, you should have indicator support in your shell:

![An example of a keylock indicator](/images/artful-shell-transition/keylock-indicator.png)

In parallel to this analysis, we reached out to the extension upstream to get a feeling of operating the same light fork procedure we did for Ubuntu Dock. As it doesn't have any undesired set of settings, we just needed to change the extension ID and add packaging. More background on why we need to change those are available in the [Ubuntu Dock introduction blog post](https://didrocks.fr/2017/08/18/ubuntu-gnome-shell-in-artful-day-5/). After getting a swift answer from [Jan Niklas](https://github.com/rgcjonas/gnome-shell-extension-appindicator), he even proposed that we transfer the main repository organization to the [ubuntu](https://github.com/ubuntu/) main github organization. We still wanted to ask if that was ok to the previous extension maintainer, [Jonas](https://github.com/rgcjonas), and wrote that down on a [bug report to initiate the transfer](https://github.com/rgcjonas/gnome-shell-extension-appindicator/issues/82). All of this is now done!

In summary:

* The new official extension repository is under the [ubuntu github organization](https://github.com/ubuntu/gnome-shell-extension-appindicator). As it's a transfer opened bugs and PR and redirection works.
* Previous and existing contributors still retain the same rights on that repository.
* The [ubuntu branch](https://github.com/ubuntu/gnome-shell-extension-appindicator/tree/ubuntu) only contains an ID change + packaging.
* The master branch will still move as it did in the past, via common contributions. We are going to implement more of the protocol (like scrolling on the indicator) directly to that master branch so that everyone can benefit.
* Jan Niklas will continue doing releases on the [GNOME extensions website](https://extensions.gnome.org/).
* We are going to push some fixes in various apps and upstream to support another session than just unity to get dropbox, skype and other apps working as expected.

As every day, if you are eager to experiment with these changes before they migrate to the artful release pocket, you can head over to [our official Ubuntu desktop team transitions ppa](https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/transitions) to get a taste of what's cooking!

There is still more to come (but at a slower pace): tomorrow and Friday, I'm heading over to London for our [Fit and Finish Sprint](https://insights.ubuntu.com/2017/08/08/ubuntu-artful-desktop-fit-and-finish-sprint/) on visual Shell and gtk theming. Also, we think that the distraction-free mode (despite being a very noble goal) from GNOME design may be a hard transition for some of our users. Thus, noticing new emails, new IM messaging and such can be difficult. However, the good news is that [we have a Dock visible by default](https://didrocks.fr/2017/08/18/ubuntu-gnome-shell-in-artful-day-5/), and so, showing back some of the badges and progress bar directly from application is something that we have an API for thanks to the Unity work. That support could be upstreamed to Dash to Dock, and consequently then, our own Dock! This would be a good answer to this problem, taking into account that we are definitively not reimplementing the messaging menu (as told, we don't want to reimplement the whole Unity in G-S, but only trying to improve what we think our user-base will need). We are currently working all of this and will update you as soon as we have more news. :)


