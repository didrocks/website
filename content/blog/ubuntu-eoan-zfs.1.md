---
title: "Ubuntu ZFS support in 19.10: introduction"
date: 2019-08-06T09:36:19+02:00
tags: [ "pu", "ubuntu" ]
banner: "/images/zsys/zol-logo.png"
type: "post"
manualdiscourse: "https://discourse.ubuntu.com/t/enhancing-our-zfs-support-on-ubuntu-19-10-an-introduction/12130"
---

# Enhancing our ZFS support on Ubuntu 19.10 - an introduction

Ubuntu has supported ZFS as an option for some time. We started with a [file-based ZFS pool on Ubuntu 15.10](https://ubuntu.com/blog/using-lxd-with-a-file-based-zfs-pool-on-ubuntu-wily), then delivered it as a [FS container in 16.04](https://ubuntu.com/blog/zfs-is-the-fs-for-containers-in-ubuntu-16-04), and  recommended it for the [fastest and most reliable container experience on LXD](https://ubuntu.com/blog/lxd-2-0-installing-and-configuring-lxd-212).

We have also created some dedicated tutorials for users who want to become more familiar with ZFS concepts, like on [basic layouts](https://tutorials.ubuntu.com/tutorial/setup-zfs-storage-pool) and [taking snapshots](https://tutorials.ubuntu.com/tutorial/using-zfs-snapshots-clones).

To do all this, we are using the excellent [ZFS On Linux](https://zfsonlinux.org/) implementation which has a vibrant and active [upstream community](https://github.com/zfsonlinux/zfs). This is built as a kernel module and therefore, no DKMS is involved.  

Three years ago we spent time looking at the licensing which applies to the Linux kernel and to ZFS.  Our [conclusions](https://ubuntu.com/blog/zfs-licensing-and-linux) are that we are acting within the rights granted and in compliance with the terms on both licenses.

By working towards adding support for ZFS as the root file system we will bring the benefits of ZFS to Ubuntu users through an easy to use interface and automated operations, abstracting some of the complexity while still allowing flexibility for power users.


## ZFS & Ubuntu 19.10

We announced 6 months ago that support for [deploying Ubuntu root on ZFS with MAAS](https://ubuntu.com/blog/deploying-ubuntu-root-on-zfs-with-maas) was available as an experimental feature.

So, what's new for [Ubuntu 19.10 (Eoan Ermine)](https://wiki.ubuntu.com/EoanErmine/ReleaseSchedule)?  As has already been reported, spotted in [our weekly team report on Ubuntu discourse](https://discourse.ubuntu.com/c/desktop/team-updates), we are going to enhance ZFS on root support in the coming cycles. Ubuntu 19.10 is a first round towards that goal.

We want to support ZFS on root as an experimental installer option, initially for desktop, but keeping the layout extensible for server later on. The desktop will be the first beneficiary in Ubuntu 19.10. Note the use of the term ‘experimental’ though! As we want to have the dataset layout right and we know a file system is crucial as it's responsible for all your data, we don't want to encourage people to use it on production systems yet, or at least, not without regular backups. The option will be highlighted as such - you are now warned! However, feel free to play with it and pass on feedback. 


## What's already in Eoan?

The worked started several weeks ago and Eoan already has some nice improvements concerning ZFS:



*   We are shipping [ZFS On Linux version 0.8.1](https://launchpad.net/ubuntu/eoan/+source/zfs-linux), with features like native encryption, trimming support, checkpoints, raw encrypted zfs transmissions, project accounting and quota and a lot of performance enhancements. You can see more about 0.8 and 0.8.1 released on the [ZOL project release page directly](https://github.com/zfsonlinux/zfs/releases).
*   We backported (and will continue to backport) some post-release upstream fixes as they fit, to provide the best user experience and reliability.
*   We added a new support in the GRUB menu. A small preview is available below and a more detailed blog post will be presented later on.

<video controls src="/images/zsys/grub-zfs.mp4"></video>

Any existing ZFS on root user will automatically get those benefits as soon as they update to Ubuntu 19.10.


## Incoming work with zsys

The goal here is to make some of the ZFS basic and advanced concepts easily accessible and transparent to anyone, like providing automated snapshots, an easy way to rollback, offline instant updates, easy backup support and so on. This is to make a solid and robust system which is configured correctly by default. This work therefore focuses on not requiring a deep understanding of ZFS, but still being to make use of the advanced features.

However, we are aware that some system administrators are very passionate about the file systems and want to be in control. This is why we designed our system in such a way that it can cope with manual tweaking, is easy to understand for people having some know-how on ZFS, and still remains very flexible.

Finally, we want to provide some best practices in terms of the ZFS dataset layout for various needs. For example, a daily desktop user will need reliability and for it to be easy to revert to a stable state, while a system administrator will want to optimise, tweak heavily and have persistent datasets, even when rolling back his/her server operating system.

For this, we are developing a new user space daemon, named [zsys](https://github.com/ubuntu/zsys). This will cooperate with GRUB (but is not limited to it) and ZFS on Linux initramfs to give advanced features we'll describe later on. Our goal is to upstream as much as possible to GRUB and zol project maintainers when things are solid enough.

For this, we are developing a new user space daemon, named [zsys](https://github.com/ubuntu/zsys). This one cooperate
with grub (but is not limited to it) and ZFS on Linux initramfs to give advanced features we'll describe later on. Our goal is to upstream to grub and ZOL project maintainers as much as possible when things are proved to be solid enough...

The whole current progress and what's up next is accessible via our public card project against the Ubuntu GitHub organisation [via this link](https://github.com/orgs/ubuntu/projects/1).

## We didn't do it alone

Thanks to early press coverage, we got in touch with Richard Laager who maintains [the upstream HOWTO on root on ZFS for Ubuntu](https://github.com/zfsonlinux/zfs/wiki/Ubuntu-18.04-Root-on-ZFS). After some back and forth on the draft specification, we came to some good conclusions and he slightly modified the HOWTO to be more compatible with our plans.

Similarly, [Marcin Skadetailrbek](https://github.com/mskarbek) got in touch as well and wants to bring ZSYS to Fedora (but some adjustments will be needed,
making this is a longer-term project).

## More work ahead

As you can see, the future of ZFS as root on Ubuntu is bright. We still have a lot to tackle and 19.10 will be only the beginning of the journey. However, the path forward is exciting and we hope to be able to bring something fresh and unique to ZFS users.

More blog posts will follow to shed more light on these enhancements, and report as to our status.

Join the discussion via the [dedicated Ubuntu discourse thread](https://discourse.ubuntu.com/t/enhancing-our-zfs-support-on-ubuntu-19-10-an-introduction/12130).