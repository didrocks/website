---
title: "Welcome To The (Ubuntu) Bionic Age: Nautilus, a LTS and desktop icons"
date: 2018-01-23T10:36:19+01:00
tags: [ "pu", "ubuntu", "gnome" ]
banner: "/images/bionic-age/no-desktop-icons.png"
type: "post"
draft: true
manualdiscourse: "https://community.ubuntu.com/t/files-nautilus-v3-28-will-lose-the-desktop-icons-capability/3115"
---

# Nautilus, Ubuntu 18.04 LTS and desktop icons: upstream and downstream views.

If you are following closely the news of various tech websites, one of the latest hot topic in the community was about [Nautilus removing desktop icons](https://csorianognome.wordpress.com/2017/12/21/nautilus-desktop-plans/). Let's try to clarify some points to ensure the various discussions around it have enough background information and not reacting on emotions only as it could be seen lately. You will have both downstream (mine) and upstream (Carlos) perspectives here.

## Why upstream Nautilus developers are removing the desktop icons

First, I wasn't personally really surprised by the announce. Let's be clear: GNOME, since [its 3.0 release](https://help.gnome.org/misc/release-notes/3.0/), doesn't have any icons on the desktop by default. There was an option in Tweaks to turn it back on, but let's be honest: this wasn't really supported.

![The GNOME 3.0 original desktop: see, no icons!](https://help.gnome.org/misc/release-notes/3.0/figures/gnome-3-0.png.fr)

The proof is that this code was never really maintained for 7 years and didn't transition to newer view technologies like the ones Nautilus is migrating to. Having patched myself this code many years ago for Unity (moving desktop icons to the right depending on the icon size, in intellihide mode which thus doesn't workarea STRUT), I can testify that this code was getting old. Consequently, it became old and rotten for something not even used on default upstream GNOME experience! It would be some irony to keep it that way.

I'm reading a lot of comments about "just keep it as option, the answer is easy". Let me disagree with this. As already stated previously during my [artful blog post series](https://didrocks.fr/2017/08/18/ubuntu-gnome-shell-in-artful-day-5/), and for the same reason that we keep Ubuntu Dock with a very few set of supported options, any added option has a cost:

* It's another code path to test (manually, most of the time, unfortunately), and the exploding combination of options which can interact badly between each other just produce an unfinished projects, where you have to be careful to not enable this and that option together, or it crashes or cause side-effects… People having played enough with [Compiz Config Settings Manager](http://wiki.compiz.org/CCSM) should know what I'm talking about.
* Not only that, but more code means more bugs, and if you have to transition to a newer technology, you have to modify that code as well. And working on that is  de-tremendous of other bug fixes, features, tests or documentations that could benefit the project. So, this piece of code that you keep and don't use, has a very negative impact on your whole project. Worse, it impacts indirectly even users who are using the defaults as they are not benefiting of planned enhancements from other part of the project, due to maintainer's time constraints.

So, yeah, there is never "**just** an option".

In addition to that argument that I took to defend upstream's position (even in front of the [French ubuntu community](https://forum.ubuntu-fr.org/viewtopic.php?pid=21849646#p21849646)), I want also to hilight that the plan to remove desktop icons was really well executed in term of communication. However, seeing the feedback the upstream developers got when following this communication plan, which takes time, doesn't motivate to do it again, where in my opinion, it should be the standard for any important (or considered as this) decisions:

* Carlos [blogged about it](https://csorianognome.wordpress.com/2017/12/21/nautilus-desktop-plans/) on planet GNOME by the end of December. He didn't only explain the context, why this change, who are impacted by this, possible solutions, but he also presented some proposals. So, there is complete what/why/who/how!
* In addition to this, there is a very good abstract, more technically oriented, in [an upstream bug report](https://gitlab.gnome.org/GNOME/nautilus/issues/158).
* A GNOME Shell [proof of concept extension](https://gitlab.gnome.org/csoriano/org.gnome.desktop-icons) was even built to show that the long term solution for users who wants to support icons on the desktop is feasible. It wasn't just a throwaway experience as [clear goals and targets](https://gitlab.gnome.org/csoriano/org.gnome.desktop-icons/issues/1) were defined on what's need to be done to move from a PoC to a working extension. And yes, by the **exact same group** who are removing desktop icons from Nautilus, I guess that means a lot!

Consequently, those are the detailed information for users to understand why this change is happening and what will be the long-term consequence of it. That is the foundation for good comments and exchanges on the various striking news blog posts. Very well done Carlos! I hope that more and more of those decisions on any free software project will be presented and explained as well as this one. ;)

## A word from Nautilus upstream maintainer

Now that I've said what I wanted to tell about my view on the upstream changes, and before detailing what we are going to do for Ubuntu 18.04 LTS, let me introduce you Nautilus upstream maintainer already many times mentioned: [Carlos Soriano](https://csorianognome.wordpress.com).

Hello Ubuntu and GNOME community,

Thanks Didier for the detailed explanation and giving me a place in your blog!

I'm writting here because I wanted to clarify some details in the interaction with downstreams, in this case Ubuntu and Canonical developers. When I wrote the blog post with all the details I explained only the part that purely refers to upstream Nautilus. And that was actually quite well received. However, two weeks after my blog post some websites explained the change in a not very good way (the clickbait magic).

Usually that is not very important, those who want factual information know that we are in IRC all the time and that we usually write blog posts about upcoming changes in our blog agregation at [Planet GNOME](https://planet.gnome.org) or here in Didier's blog for Ubuntu. This time though, some people in our closer communities (both GNOME and Ubuntu) got splashed with missconceptions, and I wanted to address that, and what better than to do it with Didier :)

One missconception was that Ubuntu and Canonical were 'yet again' using older version of software just because. Well maybe you are surprised now, **my recommendation for Ubuntu and Canonical was to actually stay in Nautilus 3.26**. Seriously, with a LTS version coming that is by far the most reasonable option. While for a regular user the upstream recommendation is to try out nemo-desktop (btw another missconception, we said nemo-desktop, not Nemo the app, for a user those are in practice two different things), for a distribution that needs to support and maintain all kind of random requests and stability promises for years, staying with a single code that they already worked with is the best option.

Another missconception I saw these days is that seems we take decisions in a rush. In short, I became Nautilus maintainer 3 years and 4 months ago. **Exactly 3 years and one month ago I realized that we need to remove that part from Nautilus**. It has been quite hard to reason within myself during these 3 years that an option that upstream was not considered the experience we wanted to provide was holding most of the major works on Nautilus, including making away contributions from new contributors given the poor state that part impacted the whole code. In all this time, dowstream like Ubuntu were a major part of me holding this code.

And the last missconception was that it looks like GNOME devs and Ubuntu devs are in completely separate nichos where noone communicates with each other. While we are usually focused on our personal tasks, when a change is going to happen we communicate. In this case, **I reached out to the desktop team at Canonical before taking the final step** providing a draft of the blog post to check out the impact given the possible options for the LTS release of Ubuntu.

In summary, the take out from here is that while we might have slighly different visions, at the end of the day we just want to provide the best experience to the users, and for that believe me we do the best we can.

In case you have any question you can always reach out to us Nautilus upstream in #nautilus IRC channel at irc.gnome.org or in our [mailing list](https://mail.gnome.org/mailman/listinfo/nautilus-list).

Hope you enjoy this read, and hopefully we will have the benefits of this work to show soon. Thanks again Didier!

## What does this mean for Ubuntu?

We thought about this as the Ubuntu Desktop team. Our next release is a LTS (Long-Term Support) version, meaning that Ubuntu 18.04 (currently named Bionic Beaver during its development) will have 5 years of support in term of bug fixes and security updates.

It also mean that most of our user audience will upgrade from our last Ubuntu 16.04 LTS to Ubuntu 18.04 LTS (or even 14.04 -> 16.04 -> 18.04!). The changes are quite large in those last 2 years in term of software updates and new features. On top of this, those users will experience for the first time [the Unity -> GNOME Shell transition](https://didrocks.fr/2017/08/03/ubuntu--guadec-2017-and-plans-for-gnome-shell-migration/), and we want to give them a feeling of comfort and familiar landmarks in our default experience, despite the huge changes underneath.

On Ubuntu desktop, we are shipping a Dock, visible by default. Consequently, the desktop view itself, without any application on top, is more important in our user experience than it is for upstream GNOME Shell default one. We think that shipping icons on the desktop is still relevant for our user bases.

Where does this leave us regarding those changes? Thinking about the problem, we came to approximately the same conclusions that upstream Nautilus developers have:

* Staying for the LTS release, on Nautilus 3.26: the pros is that it's a battle tested code, that we already know we can support (shipped on 17.10). This matches the fact that the LTS is a very important and strong commitment to us. The cons is that it won't be by release date the latest and greatest upstream Nautilus release, and maybe some integrations with other parts of GNOME 3.28 code would require more downstream work from us.
* Using an alternative file manager for the desktop, like [Nemo](https://github.com/linuxmint/nemo). Shipping on a LTS entirely new code, having to support 2 file managers (Nautilus for normal file browsing and Nemo for the desktop) and ensuring the integration between those two and all other applications works well quickly ruled out that solution.
* Upgrading to Nautilus 3.28 and shipping the PoC GNOME-Shell extension, contributing to it as much as possible before release. The issue (despite being the long-term solution) is that we'll ship as in the previous solution entirely new code and that the extension needs new APIs from Nautilus which aren't fully shaped yet (and maybe won't be ready for GNOME 3.28 even). Also, we did plan a long time in advance (end of September for 18.04 LTS) on the features and work needed to be done for the next release and we [still have a lot to do](https://trello.com/b/lsBmkzPY/ubuntu-desktop-1804-cycle) for Ubuntu 18.04 LTS, some of them being GNOME upstream code. Consequently, rushing into this extension coding, looking to our [approaching Feature Freeze deadline](https://wiki.ubuntu.com/BionicBeaver/ReleaseSchedule) on March 1st, would mean that we either drop some initially planned features, or fix less bugs, less polish, which will be detrimental to our overall release quality.

As in every release, we decide on a component by component bases what to do (upgrade to latest or not), weighing the pros and cons and trying to take the best decision for our end user audience. We think that most of GNOME components will be upgraded to 3.28. However, in that particular instance, we decided to keep Nautilus to version 3.26 on Ubuntu 18.04 LTS. You can read [the discussion that took place](https://irclogs.ubuntu.com/2018/01/09/%23ubuntu-desktop.html?_ga=2.198019628.2006842383.1516351034-90769605.1516351034#t14:46) during our weekly Ubuntu Desktop meeting on IRC, leading to that decision.

Another pro to that decision is that it gives flavors shipping Nautilus by default, like [Ubuntu Budgie](https://ubuntubudgie.org/) and [Edubuntu](https://www.edubuntu.org/), a little bit more time to find a solution matching their need, as they don't run GNOME Shell, and so, can't use that extension.

The experience will thus be: desktop icons (with Nautilus 3.26) on the default ubuntu session. The [vanilla GNOME session I talked many times about](https://didrocks.fr/2017/08/15/ubuntu-gnome-shell-in-artful-day-2/) will still be running Nautilus 3.26 (as we can only have one version of a software in the archive and installed on user's machine with traditional deb packages), but with icons on the desktop disabled, as we did on Ubuntu Artful. I think some motivated users will build Nautilus 3.28 in a ppa, but it won't receive official security and bug fixes support of course.

![Default ubuntu desktop](/images/artful-shell-transition/final_freeze_ubuntu_17.04_desktop.png)

![GNOME vanilla session desktop](/images/artful-shell-transition/vanilla-gnome-17.10.png)

Meanwhile, we will start contributing for a more long term plan on this new GNOME Shell extension with the Nautilus developers to shape a proper API, have good Drag and Drop support and so on, progressively… This will give better long term code and we hope that the following ubuntu releases will be able to move to it once we reach the minimal set of features we want from it (and consequently, update to latest Nautilus version!).

I hope that sheds some lights on both GNOME upstream and our ubuntu decisions, seeing the two perspectives and why those actions were taken, as well as the long term plan. Hopefully, those posts explaining a little bit the context will lead to informed and constructive comments as well!
