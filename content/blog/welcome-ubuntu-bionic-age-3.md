---
title: "Welcome To The (Ubuntu) Bionic Age: New WIP ubuntu theme as a snap"
date: 2018-04-10T10:49:19+01:00
tags: [ "pu", "ubuntu", "gnome" ]
banner: "/images/bionic-age/ubuntu-communitheme-desktop.png"
type: "post"
manualdiscourse: "https://community.ubuntu.com/t/call-for-participation-an-ubuntu-default-theme-lead-by-the-community/1545"
---

# Communitheme is now available for Ubuntu 18.04 LTS as a snap!

Before the release of Ubuntu 18.04 LTS, I wanted to write a few words to update you since our [first call for a theme crafted by the community](/2017/11/09/welcome-to-the-ubuntu-bionic-age-a-new-ubuntu-default-theme-call-for-participation/).

## Progress from 0 to nowdays communitheme's state

After our call for contribution in November last year over the [ubuntu community HUB](https://community.ubuntu.com/), we saw a lot of people stepping up and being interested into helping crafting this theme, which will be the default ubuntu theme in a near future. After creating a [subforum](https://community.ubuntu.com/c/desktop/theme-refresh) dedicated to that topic and adding people who volunteered, work was able to start! Communitheme was chosen as the Work In Progress codename for this effort.

We all know how design by commitee can be: everyone has different opinions, and we can get easily lost in endless discussions. This is where this subsection, set as read-only for non active contributors, helped us: we want to have focused conversation within the theme working actively on the theme, limiting chit-chatting. However, we want this work to be public, hence the fact that it is available for reading by the entire community.
There is the other threads to get inputs from the community at large, and most of people from this "theme team" [kept an eye on it](https://community.ubuntu.com/t/call-for-participation-an-ubuntu-default-theme-lead-by-the-community/1545) and answered. In retrospect, I'm really happy with the result of this setup: we didn't stop larger conversations with the community, while, during the first weeks in particular, that helped creating a lot of quick focused discussions against the main theme principles.

December and January were really going on fire, the team really shined quickly syncing up, creating base git projects for the [Shell](https://github.com/ubuntu/gnome-shell-communitheme), [GTK](https://github.com/ubuntu/gtk-communitheme) (2 and 3), [Suru](https://github.com/ubuntu/suru-icon-theme), and associated build systems, automating [a daily build ppa](https://launchpad.net/~communitheme/+archive/ubuntu/ppa). We helped on some little part to bootstrap in the setup (build system, session creation, CI, initial packaging…), but for most of them, the community worked on their own and I wasn't expecting to see that much progress so quickly!

Then, after the initial decisions taken design-wise, work continued on the implementation itself. A lot of people identified issues and reported them via bugs on the different github projects or pull requests, and active forth and back (and surely passionated!) exchanges happened! It's hard to mention everyone involved here, seeing the diversity of feedbacks both on github and the forum itself, but the core Communitheme is always listening, and larger community, testing via the ppa, are able to spot corner cases. Thanks to everyone contributing here, this is really where free software interaction shines. :)

## Expanding the project

While all this was under hard work and as it wasn't enough, [sound](https://community.ubuntu.com/t/new-system-sounds-in-18-04/1557) and [cursor](https://community.ubuntu.com/t/cursor-theme-discussion/3870) theme renewal were proposed! The Communitheme projects continued thus to expand, within those 2 new areas. It's decided, those two [new](https://github.com/ubuntu/cursor-communitheme) [repositories](https://github.com/ubuntu/communitheme-sounds) are now officially part of Communitheme project and will be deliver altogether!

It's really a delight to see that level of passion and efforts being put going way further than anything you would envision in your best dream!

## 18.04 LTS and default theme.

Some of the obligatory questions that came up was "when is it going to be shipped?". My [first blog post](/2017/11/09/welcome-to-the-ubuntu-bionic-age-a-new-ubuntu-default-theme-call-for-participation/) and the [FAQ written in November](https://community.ubuntu.com/t/faq-ubuntu-new-theme/1930) was really clear about that. Pasting from it:
> Simple answer: when it's ready! Creating a theme, with correct icon assets, is quite long. Especially as we have multiple variants of it (GTK2, GTK3, Shell, Qt…). We don't want to rush and we have a great theme currently.
>
> It means that if the theme is ready for 18.04, it will be the default. If it still needs work, we keep the current Ambiance theme as default for the next LTS. We'll make a ppa with instructions on how for people to try and follow this work in progress and it will be made default on a later ubuntu release. We mainly just want to release a quality theme and complete icon set, from day 1!

We were slowly, approaching [UI Freeze](https://wiki.ubuntu.com/BionicBeaver/ReleaseSchedule) for ubuntu bionic, and a lot of things had to be taken into account:

 * There are already some big changes during ubuntu 16.04 LTS (xenial) to ubuntu 18.04 LTS (bionic) upgrade. The most noticeable one being the change from Unity to GNOME Shell as a default desktop environment. This means that we need to ensure people have some kind of familiarity and that they can find themselves at home after the upgrade. We need to ease the transition so that those changes aren't seen as too drastic. The look and feel of the Shell and applications are obviously a great part of recognizable landmarks.
 * There were (and still!) a lot of changes coming up to Communitheme (which is a good thing!). If you look at the number of [commits](https://github.com/ubuntu/gtk-communitheme/commits/master), worked on [issues](https://github.com/ubuntu/gtk-communitheme/issues?utf8=%E2%9C%93&q=is%3Aissue+), [pull requests](https://github.com/ubuntu/gtk-communitheme/issues?utf8=%E2%9C%93&q=is%3Aissue+) alone on the GTK theme project (which is the most active one), you will see a lot of positive exchanges on them. It's even hard for me to keep the pace only lurking at them! However, once [UI Freeze](https://wiki.ubuntu.com/UserInterfaceFreeze) is set, it means the theme is set in stone, and no visible changes can easily enter the distribution. It means in general, no more refinement, no visible element or color changes for the next 5 years of support in 18.04 LTS. Basically, setting Communitheme as a default for bionic would have frozen the pace of changes going into it. At the time, a lot of heated discussions were still happening on the [main thread](https://community.ubuntu.com/t/call-for-participation-an-ubuntu-default-theme-lead-by-the-community/1545) and it didn't seem reasonable to freeze main concepts in the theme. If you are interesting, those were at discussion ~#600 (over the +#800 there currently is ;)).
 * The GTK2 and Qt theming isn't on par with the GTK3 one yet, leaving some applications looking drastically different than others.
 * As some active Communitheme members were telling, there is still a lot to be done and important bug reports to be dealt with. Also, we need more applications to be tested. For instance, at the time around UI Freeze, we just went through and understood completely the GTK2 issues that some people had. We [had fixed recently](https://github.com/Ubuntu/gnome-shell-communitheme/issues/61) why some people got unstyled GTK2 applications. A similar story for stylable Ubuntu Dock/Dash to dock also happened. This is to totally normal, and those bugs (with new ones discovered everyday) are part of creating a theme. Applications are using the toolkits and styles in very subtle ways, and you need to try to not make the whole theme unmaintainable on the long term with too many application-specific code.

For all those reasons, is wasn't reasonable to decide to ship Communitheme as a default theme for 18.04 LTS and thus the decision was taken. To ensure we didn't get a stalled theme, we provided some Ambiance (current default theme) [enhancements and fixes](https://community.ubuntu.com/t/ambiance-gnome-theme-bugbears-and-what-can-be-done-about-them/1637), some of them coming by popular request from [Ambiance-rw](https://www.gnome-look.org/p/1197991/) itself where we worked with the upstream contributor!

However, we still need to ensure that Communitheme gets a lot of testing in the widest variety of configuration and situations (japanese/chinese users, color-blind people?) against many applications. Also, some active supporters for the new theme really wanted on their installation to have latest and greatest on a stable LTS.

We thus wanted to offer the new theme to those enthusiasts so that we collect continued feedbacks, refine the theme before shipping it by default in a stable ubuntu release. However, as stated above, we can't introduce the Communitheme package as part of the distribution itself or it will be ruled by the UI Freeze, which won't allow drastic evolutions.

The existing PPA was an option but may be a little bit too technical and only provide one version of every components… Maybe we can do better! (spoiler alert: yes, we can).

## A snap for Communitheme

![Communitheme snap session](/images/bionic-age/snap-communitheme-session-selection.png)

The snap approach brings up many benefits to us:

### Discoverability & installation

Contrary to a PPA, the snap can be found directly through the Ubuntu Software application, as well as via the `snap search` command. Only one command to install it: `snap install communitheme`.

Basically, the workflow to install Communitheme is:

1. Install Communitheme snap (via GNOME Software or using the command line)
1. Reboot your computer and select the "Communitheme snap session" while logging in.

### Stability and risk approach

As mentioned previously, the PPA only delivers one version of every components to their latest master content. It means that you get the last commit of everything, including potential regressions, which isn't great for a stable system if you rely on a always-working environment. Here, the snap gives us multiple [channels](https://docs.snapcraft.io/reference/channels) and users can subscribe to different level of risks:

 * For enthusiasts who wants to live on the edge, every new commits in any master of one project part of Communitheme (Shell, GTK, sound, cursor, icon…) is automatically built and uploaded to the snap store, via a [Travis CI integration hook](https://docs.snapcraft.io/build-snaps/ci-integration). Note that we couldn't use unfortunatly https://build.snapcraft.io/, as we have a complex setup (multiple git projects being delivered from the same snap) that isn't currently supported. The downside is that the snap is only available for amd64 machines, which is the kind of hardware most of desktop users are using anyway. However, any commits on the edge aren't manually tested, so your session could be suddenly broken! If you are adventurous and want to always follow the latest of latest, just install the snap via: `snap install communitheme --edge`. It's ideal if you want to open issues, fixes and contributes to the Communitheme project!
 * For users who are a little bit more risk-adverse, but still want to use and follow this theme, just track the stable channel: `snap install communitheme` (`--stable` is implied). The promotion of a snap from **edge** to **stable** is done after some manual testing giving a seal of quality for stable users. Those release rights are shared between Carlo, Merlijn and myself. We hope to get regular refresh on the stable channel so that you can enjoy something close enough to latest and greatest, while being on the safe side. ;)
 
 You can of course switch between channels to track: `snap refresh communitheme --edge` or `snap refresh communitheme --stable` for instance.

### Reverting on bad changes

Even with this, if something goes terribly bad, snaps allow us to [revert](https://docs.snapcraft.io/reference/snap-command#revert) to a previous channel. That can be done globally for everyone with the release permissions in the snap store (so the same mechanism than promoting **edge** to **stable**) or you can do it yourself locally using `snap revert communitheme`. You won't get updated anymore to the version you reverted from, as being marked as broken, but will be able to get any newer version though. If you have to revert, please ensure you quickly open a bug or signal it on the HUB!

### Auto refreshing

Another interesting thing that the snap brings to us is auto-refreshing of the theme. Basically, you don't need to do anything to get latest from the **edge** or **stable** channels. Once installed, the snap will auto-refresh itself (even multiple times a day)! Just install and track the desired channel and forget about it! :)

### Testing changes before they are in the master version

While working on this CI build, thanks to the branch support in snap store, it is now really easy to test incoming changes before they are even merged into master!

For every pull request on any Communitheme related project, a comment is automatically generated [like in that example](https://github.com/ubuntu/cursor-communitheme/pull/1). You will be able to read this once the snap is created:

> A new test snap version is available using: `snap refresh communitheme --channel=edge/gnome-shell-communitheme-pr42`.
> Further available updates will track that pull request then.
>
> Switch back to stable or edge snap with `snap refresh communitheme --stable` or `snap refresh communitheme --edge` once you are done with it!
>
> Replace `refresh` with `install` if you haven't installed the snap yet.

Follow those instructions « et voilà », you can now test directly the fixes from this pull request and help upstream spotting any regression before they enter the master branch!

Any further commit pushed to the same pull request will trigger again a build and release of an update of the snap. This update is then automatically (as in the **edge** or **stable**) case made available and downloaded on user's machine without any action required!

I really hope this functionality will really facilitate and emulate even quicker, richer and tighter feedback loop and interactions between the community and theme designers/developers. It has never been so easy to test, switch and revert incoming changes before they land!

## Support in distro for Communitheme snap

The multiple sessions support is similar to the one we implemented for the GNOME Vanilla session that [I presented here](/2017/08/15/ubuntu-gnome-shell-in-artful-day-2/). The session is set to `XDG_CURRENT_DESKTOP=communitheme:ubuntu:GNOME` inheriting the basic ubuntu and GNOME properties, and adding **communitheme** overrides for selecting the right theme under that session.

As for the vanilla session, this works for any default gsettings key. If you changed your theme in another session, you won't inherit from the default thus. That's the reason why the snap is shipping a confined `communitheme.reset` command allowing you to reset those impacted keys to the default, and ensuring you have the correct theme by default!

Those parts (session file and gsettings overrides) aren't supported in the snap yet to be exposed to the system itself. That's the reason why the support is added to the default ubuntu 18.04 LTS distribution itself both [for the session](https://launchpad.net/ubuntu/+source/gnome-session/3.28.0-0ubuntu2), and [gsettings override](https://launchpad.net/ubuntu/+source/ubuntu-settings/18.04) itself. In addition, we needed to add a [fix for GNOME Shell](https://launchpad.net/ubuntu/+source/gnome-shell/3.28.0-0ubuntu2) to support `XDG_DATA_DIRS` for themes (it only supported it for modes, which can refer themes) that we [proposed upstream](https://gitlab.gnome.org/GNOME/gnome-shell/merge_requests/70), but awaiting for feedbacks. Some other minor changes were needed for extensions theming and cursor theme under Xorg.

The session is thus installed by default on your 18.04 system, but only reveals (thanks to `TryExec`) once you install the snap! Due to all this, the snap will consequently only work on 18.04 LTS as we need those changes in the distribution and some of them are tailored to Communitheme. We hope to generalize those patches in the future.

Note that GTK2 applications aren't themed yet in the snap, we are working on this (you can follow [this bug](https://github.com/ubuntu/gtk-communitheme/issues/322) if this is of interest to you).

## Other technical details

The Travis CI integration is easy to grasp: all projects have the same [.travis.yml](https://github.com/ubuntu/communitheme-snap-helpers/blob/master/.travis.yml) file, which downloads during build [the CI script](https://github.com/ubuntu/communitheme-snap-helpers/blob/master/build/prepare-build-snap) to ease maintenance. This one is downloading the [snapcraft.yaml](https://github.com/ubuntu/communitheme-snap-helpers/blob/master/snap/snapcraft.yaml) file centralized as well for maintenance ease. It pulls changes for every source from every component part from latest master, but it modifies the source for the current component to point to the local branch (master or PR branch), and then build using the official snapcraft Docker container a snap. It then decides to push the built snap in master or PR case and release in the appropriate channel.

Finally, in the PR case, a github token permits us to print the relevant install instruction for our users, with the unique channel name.

Note that we don't use here the isolation and security features of the snap, as it doesn't mean much in the theming context. It would be the entire session which would be snapped, which isn't in our scope here. We are already clearly benefiting from the delivery mechanism with the various features described above and where we can benefit from this innovative approach.

## Some interviews coming up!

To conclude, I hope you will really enjoy the Communitheme snap, report feedbacks and help the whole ecosystem on github or on the [community HUB](https://community.ubuntu.com/t/call-for-participation-an-ubuntu-default-theme-lead-by-the-community/1545). Do not hesitate to post feedbacks as the future look of ubuntu is in your hands! I want to thank again for their tireless efforts the Communitheme design and development team, in particular c-lobrano, godlyranchdressing, madsrh, luxamann, Merlijn, frederik, from the "core" team without forgetting ya-d, yawzub, NusiNusi and nana-4 for their extensive testing and reports. :)

I will post short interviews in the coming days from the core Communitheme team, stay tuned!
