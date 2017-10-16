---
title: "Ubuntu GNOME Shell in Artful: Day 15"
date: 2017-10-16T11:15:25-05:00
tags: [ "pu", "ubuntu", "gnome" ]
banner: "images/artful-shell-transition/final_freeze_ubuntu_17.04_desktop.png"
type: "post"
discourse: true
---

Since the [Ubuntu Rally](https://insights.ubuntu.com/2017/09/01/ubuntu-rally-in-nyc/) in New York, the Ubuntu desktop team is full speed ahead on the latest improvements we can make to our 17.10 Ubuntu release, Artful Aardvark. Last Thursday was our [Final Freeze](https://wiki.ubuntu.com/ArtfulAardvark/ReleaseSchedule) and I think it's a good time to reflect some of the changes and fixes that happened during the rally and the following weeks. This list isn't exhaustive at all, of course, and only cover partially changes in our default desktop session, featuring GNOME Shell by default. For more background on our current transition to GNOME Shell in artful, you can refer back to our decisions regarding our default session experience as [discussed in my blog post](/2017/08/03/ubuntu--guadec-2017-and-plans-for-gnome-shell-migration/).

# Day 15: Final desktop polish before 17.10 is out

## GNOME 3.26.1

Most of you would have noticed already, but most of GNOME modules have been updated to their [3.26.1 release](https://mail.gnome.org/archives/gnome-announce-list/2017-October/msg00008.html). This means that Ubuntu 17.10 users will be able to enjoy the latest and greatest from the GNOME project. It's been fun to follow again the latest development release, report bugs, catch up regressions and following new features.

GNOME 3.26.1 introduces in addition to many bug fixes, improvements, documentation and translation, updates resizeable tiling support, which is a great feature that many people will surely take advantage of! Here is the video that [Georges](https://feaneron.com/) has done and [blogged about](https://feaneron.com/2017/06/13/smarter-half-tiling-in-gnome-shellmutter/) while developing the feature for those who didn't have a look yet:

{{< youtube vfj5KjAm-LE >}}

## A quick Ubuntu Dock fix rampage

I've already praised here many times the excellent Dash to Dock upstream for their responsiveness and friendliness. A nice illustration of this occurred during the Rally. [Nathan](http://www.nhaines.com/) grabbed me in the Desktop room and asked if a particular dock behavior was desired (scrolling on the edge switching between workspaces). First time I was hearing that feature and finding the behavior being possibly confusing, I pointed him to the upstream bug tracker where [he filed a bug report](https://github.com/micheleg/dash-to-dock/issues/605). Even before I pinged upstream about it, they noticed the report and engaged the discussion. We came to the conclusion the behavior is unpredictable for most users and the fix was quickly in, which we [backported in our own Ubuntu Dock](https://launchpad.net/ubuntu/+source/gnome-shell-extension-ubuntu-dock/0.7) as well with some other glitch fixes.

The funny part is that [Chris](https://twitter.com/chrislas) witnessed this, and reported that particular awesome cooperation effort in a recent [Linux Unplugged](http://www.jupiterbroadcasting.com/118741/that-one-time-at-ubuntu-camp-lup-217/) show.

## Theme fixes and suggested actions

With our transition to GNOME Shell, we are following thus more closely GNOME upstream philosophy and dropped our headerbar patches. Indeed, as we previously, for Unity vertical space optimizations paradigm with stripping the title bar and menus for maximized applications, distro-patched a lot of GNOME apps to revert the large headerbar. This isn't the case anymore. However, it created a different class of issues: action buttons are generally now on the top and not noticeable with our Ambiance/Radiance themes.

![Enabled suggested action button (can't really notice it)](/images/artful-shell-transition/suggested-action-before-change.png)

We thus introduced some styling for the suggested action, which will consequently makes other buttons noticeable on the top button (this is how upstream Adwaita theme implements it as well). After a lot of discussions on what color to use (we tied of course different shades of orange, aubergine…), working with [Daniel](http://danielfore.com/) from Elementary ([proof!](https://plus.google.com/+MartinWimpress/posts/JQJ3s25ZkW1)), [Matthew](https://twitter.com/mpt) suggested to use the green color from the retired Ubuntu Touch color palette, which is the best fit we could ever came up with ourself. After some gradient work to make it match our theme, and some post-upload fixes for various states (thanks to [Amr](https://launchpad.net/~amribrahim1987) for reporting [some bugs on them so quickly](https://bugs.launchpad.net/ubuntu/+source/ubuntu-themes/+bug/1720570) which forced me to fix them during my flight back home :p). We hope that this change will help users getting into the habit to look for actions in the GNOME headerbars.

![Enabled suggested action button](/images/artful-shell-transition/suggested-action-enabled.png)

![Disabled suggested action button](/images/artful-shell-transition/suggested-action-disabled.png)

But That's not all on the theme front! A lot of people were complaining about the double gradient between the shell and the title bar. We just uploaded for the final freeze [some small changes](https://launchpad.net/ubuntu/+source/ubuntu-themes/16.10+17.10.20171012.1-0ubuntu1) by [Marco](http://www.3v1n0.net/) making them looking a little bit better for both titlebars, headerbars and gtk2 applications, when focused on unfocused, having one or no menus. [Another change](https://launchpad.net/ubuntu/+source/gnome-shell/3.26.1-0ubuntu1) was made in GNOME Shell css to make our Ubuntu font appear a little bit less blurry than it was under Wayland. A long-term fix is [under investigation](https://bugs.launchpad.net/ubuntu/+source/gnome-shell/+bug/1714459) by [Daniel](https://launchpad.net/~vanvugt).

![Headerbar on focused application before theme change](/images/artful-shell-transition/headerbar-focused-before.png)

![Headerbar on focused application with new theme fix](/images/artful-shell-transition/headerbar-focused.png)

![Title bar on focused application before theme change](/images/artful-shell-transition/titlebar-focused-before.png)

![Title bar on focused application with new theme fix](/images/artful-shell-transition/titlebar-focused.png)

![Title bar on unfocused application with new theme fix](/images/artful-shell-transition/titlebar-backdrop.png)


## Settings fixes

The Dock settings panel evolved quite a lot since its first inception.

![First shot at Dock settings panel](/images/artful-shell-transition/dock_settings_finale.png)

[Bastien](http://www.hadess.net/), who had worked a lot on GNOME Control Center upstream, was kindly enough to give a bunch of feedbacks. While some of them were too intrusive so late in the cycle, we implemented most of his suggestions. Of course, even if we live less than 3 kms away from each other, we collaborated as proper geeks over IRC ;)

Here is the result:

![Dock settings after suggestions](/images/artful-shell-transition/dock_settings_refreshed.png)

One of the best advice was to move the background for list to white (we worked on that with [Sébastien](https://blogs.gnome.org/seb128)), making them way more readable:

![Settings universal access panel before changes](/images/artful-shell-transition/settings-universal-access-before.png)

![Settings universal access panel after changes](/images/artful-shell-transition/settings-universal-access.png)

![Settings search Shell provider before changes](/images/artful-shell-transition/settings-search-provider-before.png)

![Settings search Shell provider after changes](/images/artful-shell-transition/settings-search-provider.png)


## i18n fixes in the Dock and GNOME Shell

Some (but not all!) items accessible via right clicking on applications in the Ubuntu Dock, or even in the upstream Dash in the vanilla session [weren't translated](https://bugs.launchpad.net/ubuntu/+source/glib2.0/+bug/1711752).

![Untranslated desktop actions](/images/artful-shell-transition/quicklist-untranslated.png)

After a little bit of poking, it appeared that only the Desktop Actions were impacted (what we called "static quicklist" in the Unity world). Those [were standardized](https://standards.freedesktop.org/desktop-entry-spec/latest/ar01s10.html) some years after  we introduced it in Unity in [Freedesktop spec revision 1.1](https://standards.freedesktop.org/desktop-entry-spec/1.1/apfs02.html).

Debian, like Ubuntu is extracting translations from desktop files to include them in langpacks. Glib is thus [distro-patched](https://anonscm.debian.org/viewvc/pkg-gnome/desktop/unstable/glib2.0/debian/patches/01_gettext-desktopfiles.patch?view=markup) to load correctly those translations. However, the patch was never updated to ensure action names were returning localized strings, as few people are using those actions. After a little bit of debugging, [I fixed the patch in Ubuntu](https://launchpad.net/ubuntu/+source/glib2.0/2.54.1-1ubuntu1) and [proposed back](https://bugs.debian.org/cgi-bin/bugreport.cgi?att=1;bug=877761;filename=fix_get_action_name_translated.debdiff;msg=10) in the [Debian Bug Tracking System](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=877761). This is now merged for the next glib release there (as the bug impacts both Ubuntu, Debian and all its derivatives).

We weren't impacted by this bug previously as when we introduced this in Unity, the actions weren't standardized yet and glib wasn't supporting it. Unity was thus directly loading the actions itself. Nice now to have fixed that bug so that other people can benefit from it, using Debian and vanilla GNOME Shell on Ubuntu or any other combinations!

![Translated desktop actions](/images/artful-shell-transition/quicklist-translated.png)

## Community HUB

[Alan](https://popey.com/) [announced recently](https://popey.com/blog/posts/new-ubuntu-community-hub-launched.html) the [Ubuntu community hub](https://community.ubuntu.com/) when we can exchange between developers, users and new contributors.

When looking at this at the sprint, I decided that it could be a nice place for the community to comment on those blog posts rather than creating another silo here. Indeed, the current series of blog post have more than 600 comments, I tried to be responsive on most of them requiring some answers, but I can't obviously scale. Thanks to some of the community who already took the time to reply to already answered questions there! However, I think our community hub is a better place for those kind of interactions and you should see below, an automated created topic on the [Desktop section of the hub](https://community.ubuntu.com/c/desktop) corresponding to this blog post (if all goes well. Of course… it worked when we tested it ;)). This is read-only, embedded version and clicking on it should direct you to the corresponding topic on the discourse instance where you can contribute and exchange. I really hope that can foster even more participation inside the community and motivate new contributors!

## Other highlights

We got some multi-monitor fixes, HiDPI enhancements, indicator extension improvements and many others… Part of the team worked with [Jonas](https://github.com/jadahl) from Red Hat on mutter and Wayland on scaling factor. It was a real pleasure to meet him and to have him tagging along during the evenings and our numerous walks throughout Manhattan as well! It was an excellent sprint followed by nice follow-up weeks.

If you want to get a little bit of taste of what happened during the Ubuntu Rally, Chris from [Jupiter Broadcasting](http://www.jupiterbroadcasting.com/) recorded some vlogs from his trip to getting there, one of them being on the event itself:

{{< youtube Z1laxZ2F-Zg >}}

As usual, if you are eager to experiment with these changes before they migrate to the artful release pocket, you can head over to [our official Ubuntu desktop team transitions ppa](https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/transitions) to get a taste of what's cooking!

Now, it's almost time to release 17.10 (few days ahead!), but I will probably blog about the upgrade experience in my next and last - for this cycle - report on our Ubuntu GNOME Shell transition!
