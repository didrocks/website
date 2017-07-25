---
title: "Ubuntu Make as a classic snap: other distro versions and continuous delivery"
date: 2017-07-25T09:46:24+02:00
tags: [ "snaps", "ubuntu-make", "uld", "ubuntulovesdevs", "pu" ]
banner: "images/build.snapcraft.hero.svg"
type: "post"
slug: "ubuntu-make-as-classic-snap-other-distros-and-CD"
---

This is a suite of blog posts explaining how we snapped Ubuntu Make which is a complex software study case with deep interactions with the system. For more background on this, please refer to our [first blog post](/2017/07/05/ubuntu-make-as-a-classic-snap-intro). For how we got here, have a look at the [last blog post](/2017/07/12/ubuntu-make-as-classic-snap-getting-16.04).
 
# Testing on 14.04 LTS
Ok, we now have a working classic snap on 16.04 LTS. However, we saw that a word of caution needs to be taken care of due to the way classic snaps enable us to access the system.
 
Starting Ubuntu Make on a 14.04 LTS vanilla virtual machine, triggering a framework installing package got us to a segfault. I was expecting that one and didn’t sweat at least. :) Indeed, between 14.04 and 16.04, a gcc ABI transition occurred for C++ libraries. The apt python binding is using the libapt C++ binary, so using the compiled 16.04 version on 14.04 couldn’t possibly work.

The fix was thus just to add libstdc++6 to our [stage-packages list](https://github.com/ubuntu/ubuntu-make/commit/592afb0db74700f413a2ba3a43662ccc5117c945).
 
 
Something should be puzzling though: this library is a dependency of *libapt-pkg5.0* which we got from *python3-apt*, why isn’t that one staged by `snapcraft` as other depdencies for us thus? Actually, snapcraft is filtering the base dependencies like *libc* and standard c++ debian packages which are expected to be installed on any linux distribution to avoid that binary duplication.
 
This is really similar to default **golang** code compilation when some cgo is used: the static binary doesn’t include the base library like libc:
 
```sh
$ ldd go-binary 
	linux-vdso.so.1 =>  (0x00007ffd0d3d7000)
	libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007fa22e594000)
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007fa22e1cb000)
	/lib64/ld-linux-x86-64.so.2 (0x000055b6be100000)
```

And so, we have to statically link them explicitly in Golang, at the price of binary size:
```sh
$ go build --ldflags '-linkmode external -extldflags "-static"'
$ ldd go-binary 
	not a dynamic executable
```

 
Most of the time, it’s working well for strictly or devmode confined snaps as they are reusing those base binaries from the *core* snap, which has the compatible versions. However, classic is once again a different world: you need to care about that yourself and don’t have access to that core snap. This is why, by explicitly declaring it in our yaml file, we do inform snapcraft that we really want to include those packages. Then, our wrapper we discussed on the previous blog post, via the *LD_LIBRARY_PATH* override, will pick them over.
 
That was the only change needed for 14.04.

# Testing on 17.04
David tested on 17.04, and due to ABI transitions, we needed to add some more libraries to ship [as those ones](https://github.com/ubuntu/ubuntu-make/commit/86c777792228297151af3afb317ece41b47f5fce).
For the exact same reason than for 14.04, as classic snap don’t rely on the core snap, we needed to ship more libraries that will be picked up by the wrapper. But that was mostly it.
 
Of course, we did a quick test run back on 16.04 LTS, and all was still working as expected.

# Continuous delivery
Now that we have a classic snap working on every ubuntu versions, it was high time to fire up a continuous delivery machine! We already have travis-ci setup for [our integration tests](https://travis-ci.org/ubuntu/ubuntu-make/branches). However, we do want to produce a snap for every architectures and not only *amd64* as Ubuntu Make is used on those.
 
[build.snapcraft.io](https://build.snapcraft.io/) is the perfect match: pick your github repo and branch and get a snap build on multiple architectures, uploaded to the store on your behalf.
 
It was just a question of clicking and setting things up in a few minutes, and here we go! From commit in master to having the snap delivered in the edge channel is just a question of minutes! We can then ask for the bug reporter to test the fix or the new feature in less than half an hour in some case! Small and efficient tight loop delivery pipeline for the win.

We even got [a badge](https://github.com/ubuntu/ubuntu-make) added to our `README.md` to link users to our snap being the latest master. Asking people to test latest and greatest few minutes after committing a fix has never being easier!

# More polish

A handy feature to triage bugs is to know which version of the code the user currently running on. I thus did change slightely the [upstream code logic](https://github.com/ubuntu/ubuntu-make/commit/19a91473e4c6a00a57c931e50bab1cbe316b7228) for detecting snap version and print out as part of `umake --version`, *SNAP_REVISION* coming from snap run. The tests have been, of course, updated accordingly.

That way, we can know that people run from a snap and which exact revision they landed on when they file an issue.

# Last gotcha

The snap is functionally equivalent to the deb, just faster to deliver! The only gotcha is the lack of shell completion support (reported as [bug #1689578](https://bugs.launchpad.net/snappy/+bug/1689578) in snapcraft). One way to workaround it would probably be to have an `umake enable-completion` command which will install that files in the user’s directory or just wait for the incoming feature to be delivered (soon).

# In a nutshell
Our Ubuntu Make classic snap enables us to deliver an up to date version, a single binary snap compatible with any ubuntu version supporting snapd, always freshly delivered from our master upstream branch. It’s a great way for us to quickly deliver fixes and get people to test some experimental changes in the edge snapd channel. No release management needed, nobody preventing quick and easy delivery to our users.
 
Also, even if we ship a bunch of dependencies in our snap, diff update mitigate those issues, and when you remove the Ubuntu Make snap from your system (but why would you do that?), all of them are purged as part of the snap. No leftover!

I’m really glad we made this move to a classic Ubuntu Make snap, and you can see it yourself by just issuing `snap install --classic ubuntu-make` and using the `ubuntu-make.umake` command!

# Published parts:

 * [Ubuntu Make as a classic snap: intro](https://didrocks.fr/2017/07/05/ubuntu-make-as-a-classic-snap-intro/)
 * [Ubuntu Make as a classic snap: getting a 16.04 snap](https://didrocks.fr/2017/07/12/ubuntu-make-as-a-classic-snap-getting-a-16.04-snap/)
 * ["Ubuntu Make as a classic snap: other distro versions and continuous delivery](https://didrocks.fr/2017/07/25/ubuntu-make-as-classic-snap-other-distros-and-CD/)
