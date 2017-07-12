---
title: "Ubuntu Make as a classic snap: intro"
date: 2017-07-05T10:26:24+02:00
tags: [ "snaps", "ubuntu-make", "uld", "ubuntulovesdevs", "pu" ]
banner: "public/ubuntu/uld.svg"
type: "post"
slug: "ubuntu-make-as-classic-snap-intro"
---

As [Ubuntu Make](https://wiki.ubuntu.com/ubuntu-make) is now in the community’s hands and we got some excellent regular contributors, one of those even became a core committer (good work again [Galileo](https://github.com/LyzardKing))! To be fair, they did most of the work in the past year. However, I was still a bottleneck for one thing: doing releases and pushing the debian package in Ubuntu. I thus recently took some time to turn the popular tool Ubuntu Make into a snap to see what the experience of switching a complex project to a snap could be like. That way, the community will be able to directly deliver from upstream to their users!
 
To install it, it’s as simple as:
```sh
$ snap install --classic ubuntu-make
```

# What is Ubuntu Make?
[Ubuntu Make](https://github.com/ubuntu/ubuntu-make) is a command line tool which allows you to download the latest version of popular developer tools on your system, installing it alongside all of the required dependencies (which will only ask for root access if you don't have them installed already), enable multi-arch on your system if you are on a 64 bit machine and integrate them with your desktop environment. Basically, one command to get your system ready to develop with!
As of today, we support more than 50 developer environments, from IntelliJ IDEs to Visual Studio Code, from Android Studio to Golang, Rust, Scala or Swift developer runtimes.
 
All of this is cooked by [a vibrant community on GitHub](https://github.com/ubuntu/ubuntu-make/network/members) tremendously helping us. A rigorous and extensive testsuite ensures that we don’t push broken changes and that upstream changes don’t break us, or inform us when they do. I did write more about our static, small, medium and large tests [here](/post/Ubuntu-Developer-Tools-Center:-how-do-we-run-tests/). For more general information about Ubuntu Make, please head over to [my blog](https://didrocks.fr/tags/ubuntulovesdevs) with various articles about this.

# Why a snap?
As we really rely on upstreams which can (and do!) change their download assets regularly, we need to be reactive in changing our code. This isn’t easy in the traditional distribution process, and SRUing to previous releases can take some time. We do use an always [updated PPA](https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/ubuntu-make) and cut regular releases, but this is more or less a workaround. Also, we note that most users don’t really know how to use that PPA.
 
That’s where a snap is a particularly good answer for us:

 * Always get an updated version of our code, easily and freshly delivered, compatible with latest developer environment changes.
 * Possibly set up per-commit releases directly from our master branch which is always working by policy.
 * One single snap compatible with all supported Ubuntu releases.
 
# What makes it special?
However, until recently, it wasn’t really possible to snap Ubuntu Make itself. Indeed, Ubuntu Make interacts with the apt database and other 3rd party tooling to setup your developer environment. This makes it hard, both confinement-wise and in terms of internal dependencies working across the board in a single snap. But that was until recently.
 
The whole process only took few hours of iterating and testing, which is almost nothing compared to the great benefits we got from it.
 
Want to try it right now? Just issue: `snap install --classic ubuntu-make`.
All commands are now available with `ubuntu-make.umake`!

You will get the latest and greatest directly from our master branch! :)
 
In the next post, we are going to deep dive into the technical details and implications of what was required in the journey to snapping it!

# Published parts:

 * [Ubuntu Make as a classic snap: intro](https://didrocks.fr/2017/07/05/ubuntu-make-as-a-classic-snap-intro/)
 * [Ubuntu Make as a classic snap: getting a 16.04 snap](https://didrocks.fr/2017/07/12/ubuntu-make-as-a-classic-snap-getting-a-16.04-snap/)
