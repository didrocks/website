---
title: "ZFS focus on Ubuntu 20.04 LTS: ZSys dataset layout"
date: 2020-05-01T09:36:19+02:00
tags: [ "pu", "ubuntu", "zfs" ]
banner: "/images/focal/zfs_install_done.png"
type: "post"
manualdiscourse: "https://discourse.ubuntu.com/t/XXXXX"
draft: True
---

# ZFS focus on Ubuntu 20.04 LTS: ZSys dataset layout

After looking at the [global partition layout](https://didrocks.fr/TODO) when you select the ZFS option on ubuntu 20.04 LTS, let’s dive in details what are exactly inside those ZFS pools, I name `bpool` and `rpool`!

We are mainly focusing on two kinds of ZFS datasets: filesytem datasets and snapshot datasets. We already eluded to them multiple times in previous blog posts, but if you want to follow this section, I highly recommend following this couple of ZFS tutorials ([setup and basics](https://ubuntu.com/tutorials/setup-zfs-storage-pool) and [snapshot and clones](https://ubuntu.com/tutorials/using-zfs-snapshots-clones)) or watching this [introduction](https://www.youtube.com/watch?v=3oG-1U5AI9A). This will help guiding you on those concepts.

## Current states and its datasets

If you run `zsysctl show --full`, you will see exactly the datasets that our current state is made of:

```sh
$ zsysctl show --full
Name:               rpool/ROOT/ubuntu_e2wti1
ZSys:               true
Last Used:          current
Last Booted Kernel: vmlinuz-5.4.0-29-generic
System Datasets:
 - bpool/BOOT/ubuntu_e2wti1
 - rpool/ROOT/ubuntu_e2wti1
 - rpool/ROOT/ubuntu_e2wti1/srv
 - rpool/ROOT/ubuntu_e2wti1/usr
 - rpool/ROOT/ubuntu_e2wti1/var
 - rpool/ROOT/ubuntu_e2wti1/usr/local
 - rpool/ROOT/ubuntu_e2wti1/var/games
 - rpool/ROOT/ubuntu_e2wti1/var/lib
 - rpool/ROOT/ubuntu_e2wti1/var/log
 - rpool/ROOT/ubuntu_e2wti1/var/mail
 - rpool/ROOT/ubuntu_e2wti1/var/snap
 - rpool/ROOT/ubuntu_e2wti1/var/spool
 - rpool/ROOT/ubuntu_e2wti1/var/www
 - rpool/ROOT/ubuntu_e2wti1/var/lib/AccountsService
 - rpool/ROOT/ubuntu_e2wti1/var/lib/NetworkManager
 - rpool/ROOT/ubuntu_e2wti1/var/lib/apt
 - rpool/ROOT/ubuntu_e2wti1/var/lib/dpkg
User Datasets:
 User: didrocks
 - rpool/USERDATA/didrocks_wbdgr3
 User: root
 - rpool/USERDATA/root_wbdgr3
```

Note: if you have datasets directly under `rpool`, you will see a additional category named "Persistent datasets". For instance, if `rpool/var/lib/docker` exists, you will see:

```
Persistent Datasets:
 - rpool/var/lib/docker
```

You will see that the state name corresponds to the dataset mounting on `/` (`rpool/ROOT/ubuntu_e2wti1`). It’s listed under the "System Datasets" category. We also have 2 other "User Datasets" and "Persistent Datasets" categories. Let’s examine them one after another.

### System states and their datasets

System datasets are datasets which forms… your installed system! (except `/boot/efi` and `/boot/grub` which are on a separate partition as explained [in the previous article about partitioning](https://didrocks.fr/TODO)). You can see that kernel and initramfs are located on `bpool/BOOT/ubuntu_e2wti1` matching the `rpool/ROOT/ubuntu_e2wti1` base name. Any of those and descendant are considered as system datasets.

In general, every datasets (and its children) under **<pool_name>/ROOT/<DATASET_ROOT>** will form a single system state. If a matching **<DATASET_ROOT>** is found in any **<other_pool_name>/BOOT/<DATASET_ROOT>** with the same name, it will be considered as part of the same state. This is not mandatory though and if we have multiple candidates, we always prefer **<pool_name>/ROOT/<DATASET_ROOT>/boot** over **<pool_name>/BOOT/<DATASET_ROOT>** over **<other_pool_name>/BOOT/<DATASET_ROOT>** and over **<pool_name>/ROOT/<DATASET_ROOT>** `/boot` subdirectory on `/` dataset. Note that we have some optimization for our default layout with a **bpool/BOOT/<DATASET_ROOT>** when generating our grub menu and loading our system, but this isn’t mandatory.

When you save a system state, a snapshot is created on all of them in sync. If you look at the history part (if you already have one system state save) of the same command, you can see this:

```sh
$ zsysctl show --full
[…]
  - Name:               rpool/ROOT/ubuntu_e2wti1@autozsys_k8uf7o
    Created on:         2020-05-05 08:35:43
    Last Booted Kernel: vmlinuz-5.4.0-28-generic
    System Datasets:
     - bpool/BOOT/ubuntu_e2wti1@autozsys_k8uf7o
     - rpool/ROOT/ubuntu_e2wti1@autozsys_k8uf7o
     - rpool/ROOT/ubuntu_e2wti1/srv@autozsys_k8uf7o
     - rpool/ROOT/ubuntu_e2wti1/usr@autozsys_k8uf7o
     - rpool/ROOT/ubuntu_e2wti1/var@autozsys_k8uf7o
     - rpool/ROOT/ubuntu_e2wti1/usr/local@autozsys_k8uf7o
     - rpool/ROOT/ubuntu_e2wti1/var/games@autozsys_k8uf7o
     - rpool/ROOT/ubuntu_e2wti1/var/lib@autozsys_k8uf7o
     - rpool/ROOT/ubuntu_e2wti1/var/log@autozsys_k8uf7o
     - rpool/ROOT/ubuntu_e2wti1/var/mail@autozsys_k8uf7o
     - rpool/ROOT/ubuntu_e2wti1/var/snap@autozsys_k8uf7o
     - rpool/ROOT/ubuntu_e2wti1/var/spool@autozsys_k8uf7o
     - rpool/ROOT/ubuntu_e2wti1/var/www@autozsys_k8uf7o
     - rpool/ROOT/ubuntu_e2wti1/var/lib/AccountsService@autozsys_k8uf7o
     - rpool/ROOT/ubuntu_e2wti1/var/lib/NetworkManager@autozsys_k8uf7o
     - rpool/ROOT/ubuntu_e2wti1/var/lib/apt@autozsys_k8uf7o
     - rpool/ROOT/ubuntu_e2wti1/var/lib/dpkg@autozsys_k8uf7o
    User Datasets:
      User: didrocks
         - rpool/USERDATA/didrocks_wbdgr3@autozsys_k8uf7o
      User: root
         - rpool/USERDATA/root_wbdgr3@autozsys_k8uf7o
```

Here, all system datasets snapshot names match the generated `@autozsys_k8uf7o`. As a reminder from [a previous blog post](https://didrocks.fr/TODO), a history state save can also be formed of filesystem dataset clones instead of snapshots. Clones are like snapshots in our case, after a state revert (and thus, have a different suffix after `_`). However, this changes nothing to this concept.

If you are a ZFS expert, you are probably wondering why we have so many datasets in our default layout. Hold on your excitement, you have been able to wait for 8 blog posts to arrive to this point, I just ask you to wait for a couple of more paragraphs! :)

Before that, let’s jump on the second type of datasets: User Datasets.

### User states and their datasets

User state is formed by default of one dataset per user. In our example, the "didrocks" user has one dataset `rpool/USERDATA/didrocks_wbdgr3` for the current user state. Similarly, history system state has a linked user state `rpool/USERDATA/didrocks_wbdgr3@autozsys_k8uf7o`. The same snapshot suffix helps linking there, and we will come later (next blog post) on how we associate user state to system state for filesystem datasets.

As we have **/ROOT/** (and **/BOOT/**) to identify system states, **/USERDATA/** is where will live user state datasets, prefixed by user name. Those user datasets can have any children datasets as you need and those will be taken into account by ZSys.

Once linked to a system, we snapshots user datasets on system state saving (to allow reverting to a system **with** its userdata for all users). We also save hourly for connected users their own state, for finer-grain revert. This is also listed in the **Users:** section of the zsysctl show command:

```sh
$ zsysctl show --full
[…]
Users:
  - Name:    didrocks
    History: 
     - rpool/USERDATA/didrocks_wbdgr3@autozsys_stb57j (2020-05-13 15:53:20): rpool/USERDATA/didrocks_wbdgr3@autozsys_stb57j
     - rpool/USERDATA/didrocks_wbdgr3@autozsys_5ffi7s (2020-05-13 14:52:20): rpool/USERDATA/didrocks_wbdgr3@autozsys_5ffi7s
     - rpool/USERDATA/didrocks_wbdgr3@autozsys_hmolrv (2020-05-13 13:51:20): rpool/USERDATA/didrocks_wbdgr3@autozsys_hmolrv
     - rpool/USERDATA/didrocks_ptdr42 (2020-05-13 13:12:10): rpool/USERDATA/didrocks_ptdr42
```

Note that this is redundant above as we only have one dataset per user state associated. However, for shared user states, this makes sense:
```sh
$ zsysctl show --full
[…]
Users:
  - Name:    didrocks
    History: 
     - rpool/USERDATA/didrocks_e2jj0s-rpool.ROOT.ubuntu-idjvq9 (2020-05-01 14:43:20): rpool/USERDATA/didrocks_e2jj0s
```
The user states associating dataset `rpool/USERDATA/didrocks_e2jj0s` to system state named `rpool/ROOT/ubuntu_idjvq9` is named `rpool/USERDATA/didrocks_e2jj0s-rpool.ROOT.ubuntu-idjvq9`.

For both system and user states, you can add or remove children datasets at any time, and when reverting, each state will track exactly what datasets were available at that time and restore only those datasets (not more, not less). This is even true for shared user states between multiple states, but with the user states not having the same number of datasets on each system states! Basically, complex scenarios and changing system or user dataset layout is tracked on the history.

Similarly to **/BOOT/**, **/USERDATA/** can be on any pool. We have the same logic to prefer the current root pool than others, but if you create a **/USERDATA/** on another pool, any `useradd` or system calls will try to do the right thing and reuse what you have already setup.

What is not tracked by the history, and this is a good thing, are the well-named "Persistent Datasets" :)

### Persistent datasets

As you can infer from the name, those are datasets that are shared across all states. They are never automatically snapshotted nor reverted. The idea is to have those persistent disk space which are moving a lot in term of data content. The counterpart of this persistence is that you don’t really care about their content or can manually recover if the data are not compatible after a revert. Also, you may want to permanently persistent some data in various cases.

Datasets are considered persistent if you create directly any of them outside of any **/ROOT/**, **/BOOT/** and **/USERDATA/** namespace. Of course, persistent datasets are ignored by garbage collection as there is no history to collect and we ignore your manual snapshots over it.

For instance. the docker [ZFS storage driver](https://docs.docker.com/storage/storagedriver/zfs-driver/) has a tendency of creating a lot of datasets automatically on each `docker run` invocation, and keeping them on stopped containers. I don’t want to store and have any history on them. Consequently, I have created a `rpool/var/lib/docker` persistent datasets which is taken into account in the system:

```sh
$ zsysctl show --full
[…]
Persistent Datasets:
 - rpool/var/lib/docker
```

You will note that we don’t repeat them over each history system state entry for consistency, but they are indeed available and shared between any of them.

More practically, to do this, we create 3 datasets:

* `rpool/var` with `canmount=off` (`rpool/ROOT/ubuntu_e2wti1/var` is the real mountpoint for `/var`) with `zfs create -o canmount=off rpool/var`
* Similarly, `rpool/var/lib` with `canmount=off` (`rpool/ROOT/ubuntu_e2wti1/var/lib` is the real mountpoint for `/var/lib`) with `zfs create -o canmount=off rpool/var/lib`
* And finally our desired `rpool/var/lib/docker` which will be mounted over `/var/lib/docker` as our persistent dataset with `zfs create rpool/var/lib/docker`

This is the reason why `rpool` has a mountpoint of `/` with `canmount=off`, you can directly take advantage of ZFS inheritance by creating datasets under it without having to set mountpoints on each of them. You could also create directly `rpool/var-lib-docker` with a mountpoint set to `/var/lib/docker` if you prefer, but I find the other scheme to be more explicit, especially if you create more datasets under the same directory.

Once done, we get:

```sh
zfs list -r -o name,canmount,mountpoint rpool/var
NAME                          CANMOUNT  MOUNTPOINT
rpool/var                          off  /var
rpool/var/lib                      off  /var/lib
rpool/var/lib/docker               on   /var/lib/docker
```

Note that we are currently fixing the docker package in ubuntu to create this automatically for you. You can think of a similar scheme for any VM images that you don’t want to version or [LXD containers](https://linuxcontainers.org/fr/lxd/getting-started-cli/).

By default on the desktop, we don’t have any persistent datasets and people can tweak exceptional cases as listed above on their will. We focus heavily on getting a bullet proof ubuntu desktop, where reverting will **reliably** restore you to a good working state. However, you can forsee different needs for servers.

If you are a sysadmin, other use cases will come to your mind: you maybe want a persistent database, or webserver assets or any other server-related tasks you want to avoid rolling back in any case - but if you downgrade to an earlier version of your system, and for instance, your database tool, you will have to ensure that you can still load data created with a newer version of it. This is why we created such a layout scheme!

### Why so many System datasets?

Remember the number of System dataset we have on desktop:
```sh
$ zsysctl show --full
[…]
System Datasets:
 - bpool/BOOT/ubuntu_e2wti1
 - rpool/ROOT/ubuntu_e2wti1
 - rpool/ROOT/ubuntu_e2wti1/srv
 - rpool/ROOT/ubuntu_e2wti1/usr
 - rpool/ROOT/ubuntu_e2wti1/var
 - rpool/ROOT/ubuntu_e2wti1/usr/local
 - rpool/ROOT/ubuntu_e2wti1/var/games
 - rpool/ROOT/ubuntu_e2wti1/var/lib
 - rpool/ROOT/ubuntu_e2wti1/var/log
 - rpool/ROOT/ubuntu_e2wti1/var/mail
 - rpool/ROOT/ubuntu_e2wti1/var/snap
 - rpool/ROOT/ubuntu_e2wti1/var/spool
 - rpool/ROOT/ubuntu_e2wti1/var/www
 - rpool/ROOT/ubuntu_e2wti1/var/lib/AccountsService
 - rpool/ROOT/ubuntu_e2wti1/var/lib/NetworkManager
 - rpool/ROOT/ubuntu_e2wti1/var/lib/apt
 - rpool/ROOT/ubuntu_e2wti1/var/lib/dpkg
 ```

The whole idea of this separation is to offer bridges with the second kind of layout we want to offer, because, as you may reckon, ZSys and ZFS on root option will be available in a future releases as a server install option. We made great care to have a compatible layout with it with sensible different defaults, and we will support a way to switch from one layout to the other.

On a server, as said, you have way more occasions on wanting persistent datasets: databases, services, website contents… You will then of course have to deal with forward compatibility of your data with the various softwares in case of revert, but this layout is destinated to more advanced users who can handle those issues.

By default, the server layout would look something like that:
```sh
$ zsysctl show --full
[…]
System Datasets:
 - bpool/BOOT/ubuntu_e2wti1
 - rpool/ROOT/ubuntu_e2wti1
 - rpool/ROOT/ubuntu_e2wti1/usr
 - rpool/ROOT/ubuntu_e2wti1/var
 - rpool/ROOT/ubuntu_e2wti1/var/lib/AccountsService
 - rpool/ROOT/ubuntu_e2wti1/var/lib/NetworkManager
 - rpool/ROOT/ubuntu_e2wti1/var/lib/apt
 - rpool/ROOT/ubuntu_e2wti1/var/lib/dpkg
User Datasets:
[…]
Persistent Datasets:
 - rpool/srv
 - rpool/usr/local
 - rpool/var/lib
 - rpool/var/games
 - rpool/var/log
 - rpool/var/mail
 - rpool/var/snap
 - rpool/var/spool
 - rpool/var/www
 ```

 Looks familiar isn’t it? What we can note is that we now have way more persistent directories:

* logs files in `/var/log` and local mails (`/var/mail`), which is interesting to troubleshoot issues and have a continuity in auditing, even after a revert
* some server related webserver content files like `/srv` and `/var/www`
* locally compiled sofwares in `/usr/local`
* snaps (`/var/snap`) which handle their revisions themselves
* some other directories `/var/games` and printing related tasks `/var/spool`
* and more importantly, `/var/lib` becomes persistent, it means that database content and a lot of other programs data that you want to persist on the server will automatically be persistent (grafana, influxdb, jenkins, mysql, postgresql, just to name a few).

However, there are some system-related data in `/var/lib`, like your network configuration and primarily, your package manager state (**apt** and **dpkg**). Those are thus store and linked to System Datasets as the system revert will be inconsistent if the **apt or dpkg databases** for instance were kept persistent. From that scheme, you can infer there is a **canmount=off** set on `rpool/ROOT/ubuntu_e2wti1/var/lib/` so act as a dataset container for `rpool/ROOT/ubuntu_e2wti1/var/lib/dpkg` (dpkg dabatase) for instance.

This is how we combine having both a bullet-proof system, but still giving flexibility for sysadmins to perform their tasks and keep all logs, service data and general local content even if a system revert is needed.

We have thus a lot of dataset (which is basically no cost, just more code in how we have to handle it) on the desktop, but this brings this whole flexible where sysadmin can switch between types with a simple zfs rename.

# Final thoughts

Note that ZSys has no hardcoded layout knowledge. It only segregated datasets in different types, treating **/ROOT/**/**/BOOT/** as system datasets containers, **/USERDATA/** for user related dataset and the rest as persistent datasets. You can create any number of children datasets as you wish and need, knowing now exactly what the impacts will be in term of history saving, space disk and such. We hope this sheds some lights on the decisions we took and why we have all those default datasets created by our installer.

This being done, I think there is few but important domains remaining to be covered. As you saw in the previous command, we know exactly what kernel you booted with, we also associat user datasets to system datasets despite having different names, we keep track of the last successful boot time and last used. How is that all stored? That will be our next adventure in this blog post series. See you there :)

Meanwhile, join the discussion via the [dedicated Ubuntu discourse thread](https://discourse.ubuntu.com/t/).