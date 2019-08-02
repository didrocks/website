---
title: "Ubuntu ZFS support in 19.10: introduction"
date: 2019-08-06T09:36:19+02:00
tags: [ "pu", "ubuntu" ]
banner: "/images/zsys/zol-logo.png"
type: "post"
draft: true
manualdiscourse: "TODO"
---

# Enhancing our ZFS support on ubuntu 19.10, an introduction

It's been quote some time that Ubuntu is supporting ZFS as an option, we started with a [file-based ZFS pool on Ubuntu 15.10](https://ubuntu.com/blog/using-lxd-with-a-file-based-zfs-pool-on-ubuntu-wily),
then delivered it as a [FS containers in 16.04](https://ubuntu.com/blog/zfs-is-the-fs-for-containers-in-ubuntu-16-04),
and recommended it for the [fastest and more reliable container experience on LXD](https://ubuntu.com/blog/lxd-2-0-installing-and-configuring-lxd-212).

We even have some dedicated tutorials for people who want to become more familiar with the ZFS concepts, like on
[basics layouts]https://tutorials.ubuntu.com/tutorial/setup-zfs-storage-pool and [taking snapshots](https://tutorials.ubuntu.com/tutorial/using-zfs-snapshots-clones).

To do all this, we are using the excellent [ZFS On Linux](https://zfsonlinux.org/) implementation, which has a vibrant
and [active upstream community](https://github.com/zfsonlinux/zfs). This one is built as a kernel module (no DKMS involved). We know that some people (as we read that as well recently in comments) are still questioning the licensing, however, we did draw [our conclusion on that topic 3 years ago](https://ubuntu.com/blog/zfs-licensing-and-linux) linked here as a reference.

## ZFS & Ubuntu 19.10

The next step was to start supporting ZFS on root, meaning an entire ZFS system. We announced 6 months ago the support for
[deploying Ubuntu root on ZFS with MAAS](https://ubuntu.com/blog/deploying-ubuntu-root-on-zfs-with-maas) as an experimental feature.

So, what's new around here for [Ubuntu Eoan Ermine](https://wiki.ubuntu.com/EoanErmine/ReleaseSchedule) (which will become ubuntu 19.10)? As the tech press already reported, spotted from [our weekly team report on ubuntu discourse](https://discourse.ubuntu.com/c/desktop/team-updates), we are going to enhance ZFS on root support in the coming cycles, and this
one is a first round towards that goal.

We want to support ZFS on Root for both desktop and servers as an installer **experimental** option. The desktop will
be the first beneficiary in Ubuntu 19.10. Note the bold case around experimental though! As we want to have the dataset layout
right and as we know a file system is a very crucial part as it's responsible of your data, we don't want to encourage people using it on production system yet, or at least, not without very regular backups. The option will be thus highlighted as such,
you are now warned! However, feel free to play and break it as much as possible and give us (hopefully positive) feedback. :)

## What's already in Eoan?

The worked started several weeks ago and Eoan already has some nice improvements concerning ZFS:

* We are shipping [ZFS On Linux version 0.8.1](https://launchpad.net/ubuntu/eoan/+source/zfs-linux), with features like native encryption, trimming support, checkpoints, raw encrypted zfs transmissions, project accounting and quota and a lot of performance
enhancements. You can see more about 0.8 and 0.8.1 released on the [ZOL project release page directly](https://github.com/zfsonlinux/zfs/releases).
* We backported (and will continue to backport) some post-releases upstream fixes as they may fit, to provide the best
user experience and reliability.
* We added a whole new support in GRUB menu. A small preview is available below and a more detailed blog post will be
presented on a separate blog post later on.

<video controls src="/images/zsys/grub-zfs.mp4"></video>

Any existing ZFS on root user will automatically get those benefits as soon as they update to Ubuntu 19.10.

## Incoming work with zsys

The goal here is to make some of the ZFS basic and advanced concepts easily accessible and transparent to anyone, like providing
automated snapshots, easy way to rollback, offline instant updates, easy backup support… This is to make a very solid and robust system, configured correctly by default. This work is thus focusing on not requiring any deep understanding on ZFS, but be able to still profit from advanced features.

However, we are really aware that some system administrators are very passionate about file systems and wants to be under
control, this is why we designed our system in such a way that it can cope with manual tweaking, is easy to understand
for anyone having some know-how on ZFS, and still remains very flexible.

Finally, we want to provide some best practices in term of zfs dataset layout for various needs: a daily desktop user will
just need a very reliable and easy to revert to a stable state, while a system administrator will want to optimize, tweak
heavily and have persistent datasets, even when rollbacking his/her server operating system.

For this, we are developing a new user space daemon, named [zsys](https://github.com/ubuntu/zsys). This one cooperate
with grub (but is not limited to it) and ZFS on Linux initramfs to give advanced features we'll describe later on. Our goal is to upstream to grub and zol project maintainers as much as possible when things are proved to be solid enough...

The whole current progress and what's up next is accessible via our public card project against the ubuntu github organization
[via this link](https://github.com/orgs/ubuntu/projects/1).

## We didn't do it alone

Thanks to the early press coverage, we could get in touch with Richard Laager, who maintains
[the upstream HOWTO on root on ZFS for Ubuntu](https://github.com/zfsonlinux/zfs/wiki/Ubuntu-18.04-Root-on-ZFS).
After some forth and back on the draft specification and long discussions over it, we came to some good output and
he modified slightly the HOWTO to be more compatible with our plans.

Similarly, Marcin Skadetailrbek got in touch as well and wants to bring ZSYS to Fedora (but some adjustments will be needed,
making this is a longer-term project).

Both have been commenting and contributing on a specification (Work In Progress) that we wrote and a copy is
[accessible here](https://docs.google.com/document/d/1m9VWOjfmdbujV4AQWzWruiXYgb0W4Hu0D8uQC_BWKjk) for those who want
to see what's cooking up for the longer term plan. Thanks to them for their very useful inputs!

## More work ahead

As you can see, the future of ZFS as root on ubuntu is bright. We still have a lot to tackle and 19.10 will be only the
beginning of the journey. However, the path forward is exciting and we hope to be able to bring something exciting and
unique to ZFS users!

More blog posts in september after summer break will shed more lights on parts of those enhancements, and report where
we stand at.

You can discuss about all this [on our dedicated ubuntu discourse thread](TODO).
