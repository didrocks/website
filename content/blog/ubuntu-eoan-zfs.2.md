---
title: "Ubuntu ZFS support in 19.10: ZFS on root"
date: 2019-10-11T09:36:19+02:00
tags: [ "pu", "ubuntu", ]
banner: "/images/zsys/eoan-installer-choice.png"
type: "post"
draft: true
manualdiscourse: "https://discourse.ubuntu.com/t/enhancing-our-zfs-support-on-ubuntu-19-10-an-introduction/12130"
---

# ZFS on root

This is part 2 of our blog post series on our current and future work around ZFS on root support in ubuntu. If you didn't read yet the [introductionary post](/2019/08/06/ubuntu-zfs-support-in-19.10-introduction/), I strongly recommend you to do this first!

We are going to discuss here what landed by default ubuntu 19.10.

## Upstream Zfs On Linux

We are shipping [ZFS On Linux version 0.8.1](https://launchpad.net/ubuntu/eoan/+source/zfs-linux), with features like native encryption, trimming support, checkpoints, raw encrypted zfs transmissions, project accounting and quota and a lot of performance enhancements. You can see more about 0.8 and 0.8.1 released on the [ZOL project release page directly](https://github.com/zfsonlinux/zfs/releases). 0.8.2 didn't make it on time for a good integration and tests in eoan. So, we backported some post-release upstream fixes as they fit, like newer kernel compatibility, to provide the best user experience and reliability. Some small upstream fixes and feedbacks were already [contributed by our team](https://github.com/zfsonlinux/zfs/commit/8ae8b2a1445bcccee1bb8ee7d4886f30050f6f53) to upstream ZFS On Linux project.

Any existing ZFS on root user will automatically get those benefits as soon as they update to Ubuntu 19.10.

## Installer integration

The ubiquity installer is providing now an **experimental** option for setting up ZFS on root on your system. While ZFS is quite a mature product for a long time, the installer ZFS support option is is alpha and people opting in should be conscious about it. It's not advised to run this on production system or system you have critical data on (apart if you have regular and verified backups, which we all do, correct?). To be fully clear, there may be breaking changes in the design as long as the feature is experimental, and we may, or may not, provide transition path to the next layout.

With that being said, what does ZFS on root means? It means that most of your system will run on ZFS. Basically even your "/" directory, is installed on ZFS.

Ready to jump in, despite all those disclaimers? If so, you download an ubuntu 19.10 ISO, you will see that the partionner screen in ubiquity has an additional option (please read the **Warning**!):

![ZFS option at the format screen](/images/zsys/eoan-installer-choice.png)

Yes, the current **experimental** support is limited right now to a whole disk installation. If you have multiple disks, the next screen will propose you to pick which one:

![if more than one disk is available, choose which one you will pick](/images/zsys/eoan-installer-disk-choice.png)


You will then get the "please confirm we'll reformat your whole disk" screen.

![still some quirks to iron out, like formatting confirmation page](/images/zsys/eoan-warning-erase-disk.png)

… and finally the installation proceed as usual:

![installation in progress](/images/zsys/eoan-installer-installing.png)

In case you didn't notice yet, this is **experimental** (what? ;)) and we have some known quirks, like the [confirmation screen](https://bugs.launchpad.net/ubuntu/+source/ubiquity/+bug/1847719) showing that it's going to format and create an ext4 partition. This is difficult to fix for ubuntu 19.10 (for the technical users interested in details, what we are actually doing is creating multiple partitions to let partman handling the ESP, and then, overwrite the ext4 partition with ZFS, so it’s technically not lying ;)). It’s something we will fix before getting out of the **experimental** phase, hopefully next cycle.

## Partitions layout

We'll create the following partitions:

### rpool

One ZFS partition for the "rpool" (as root pool), which will contain your main installation and user data. This is basically your main course and the one we'll detail the dataset layout in the next article as we have a lot to say about it.

### bpool

Another ZFS partition for your boot pool named "bpool", which has contains kernels and initramfs (basically your `/boot` without the EFI and bootloader). We have to separate this from your main pool as grub can't support all ZFS features that we want to enable on the root pool, and so, your pool would be otherwise unreadable by your boot loader, which will sadly result in unbootable system! Consequently, this pool runs a different version of ZFS pool version (right now, **version 28**, but we are looking for [next cycle to upgrade to version 5000](https://github.com/orgs/ubuntu/projects/1#card-27647903), with some features disabled). Note that due to that, even if `zpool status` is proposing you to upgrade your bpool, you should never do that your you won't be able to reboot. We will work on a [patch to prevent this](https://bugs.launchpad.net/ubuntu/+source/zfs-linux/+bug/1847389) to happen.

### ESP partition

There is the ESP partition (mounted as `/boot/efi`). Right now, it's only created if you have an UEFI system, but we might get it created in ubiquity [systematically in the future](https://bugs.launchpad.net/ubuntu/+source/ubiquity/+bug/1847721), so that people who disabled secure boot and enable it later on can have a smooth transition.

### grub partition

A **grub** partition (mounted as `/boot/grub`), which is formatted as **ext4**. This partition isn't a ZFS one because it's global to your machine, so the content and state should be shared between multiple installations on the same system. In addition, we don't want to reference a grub menu which can be snapshotted and roll backed, as it means the grub menu won't give access to "future system state" after a particular revert. If we succeed in having an ESP partition systematically created in the future, we can move grub itself to it unconditionally next cycle.

## Continuing work on pure ZFS system

We are planning to continue reporting feedback upstream (probably post 19.10 release, once we have more room for giving detailed information and various use-case scenarios) as our default dataset layout is quite advanced (more on that later) and current upstream mount ordering generator doesn't really cope with it. This is the reason why we took the decision to disable our revert GRUB feature for pure ZFS installation (but not [Zsys](https://github.com/ubuntu/zsys)!) in 19.10, as some use case could lead to unbootable systems. This is a very alpha experiment, but we didn't want to risk on purpose user's data.

But this is far from being the end of our road to our enhance ZFS support in ubuntu! Actually, the most interesting and exciting part (from a user's perspective) will come with Zsys.

## Zsys, ZFS System handler

[Zsys](https://github.com/ubuntu/zsys) is our work in progress to an enhanced support of ZFS systems. It allows running multiple ZFS installations in parallel on the same machine and managing complex ZFS dataset layouts, separating user data from system and persistent data. It will soon provide automated snapshots, backups, system managements.

However, as we first wanted to have feedback in Ubuntu 19.10 about pure ZFS systems, we didn't seed it by default. It's available though an `apt install zsys` away for those adventurous audience, and some ubuntu flavors even already jumped into the bang wagon where it will be installed by default! Even if you won't immediately see differences, this will unleash some of our grub, adduser and initramfs integration that are baked right in 19.10.

The excellent [Ars Technica](https://arstechnica.com/information-technology/2019/10/a-detailed-look-at-ubuntus-new-experimental-zfs-installer/) review by [Jim Salter](https://twitter.com/jrssnet) was wondering about the quite complex dataset layout we are setting up. We'll shed some lights on this on the next blog post which will explain what Zsys is really, what it does bring to the table and what our future plans are.

The future of ZFS on root on ubuntu is bright, I'm personally really excited about what this is going to bring in a short future to both server and desktop users! (and yes, we can cook some very nice features for our desktop user using ZFS)!

If you want to join the discussion, feel free to hop in your [ubuntu discourse dedicated topic](https://discourse.ubuntu.com/t/enhancing-our-zfs-support-on-ubuntu-19-10-an-introduction/12130).
