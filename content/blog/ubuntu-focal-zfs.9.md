---
title: "ZFS focus on Ubuntu 20.04 LTS: ZSys properties on ZFS datasets"
date: 2020-05-01T09:36:19+02:00
tags: [ "pu", "ubuntu", "zfs" ]
banner: "/images/focal/zfs_user_properties.png"
type: "post"
manualdiscourse: "https://discourse.ubuntu.com/t/XXXXX"
draft: True
---

# ZFS focus on Ubuntu 20.04 LTS: ZSys properties on ZFS datasets

We are almost done in our long journey [presenting our ZFS work](https://didrocks.fr/TODO) on Ubuntu 20.04 LTS. The last piece to highlight is how we annotate datasets with some user properties to store metadata needed on boot and on state revert. As we stated on our [ZSys presentation article](https://didrocks.fr/TODO), one of the main principles is to avoid using a dedicated database which can quickly go out of sync with the real system: we store - and thus, rely - only on ZFS properties that are set on the datasets themselves. Taking your pool and moving it to another machine is sufficient.

This will probably give you the necessary information (alongside with the post on [partition](https://didrocks.fr/TODO) and [dataset](https://didrocks.fr/TODO) layouts) if you want to turn your existing ZFS system to one compatible with ZSys.

Without further ado, it’s time to directly check all that on details!


## ZFS properties

Of course, ZFS datasets properties are the main source of information when we build up a representation of the system when starting ZSys. We are using in particular `canmount` and `mountpoint` ZFS properties. While the usage of `mountpoint` is unequivocal, `canmount` has 3 meaningful states for us:

* **off**: we ignore the dataset (apart when reverting) in those cases. This dataset will not be mounted (but still snapshotted if part of system datasets). This is used by default on "container" filesystem dataset, like `rpool/ROOT/ubuntu_123456/var/lib` in the server layout we demonstrated in our previous post which can then host `rpool/ROOT/ubuntu_123456/var/lib/apt` for instance.
* **on**: those are the current (or previous boot at least) datasets that were mounted. We turn every datasets we want to mount and act on to that value in our `zsysd boot-prepare` call in the generator to prepare the system booting on the correct set of datasets.
* **noauto**: we set that value for any clones (previous system or user states) when reverting. This is another task that `zsysd boot-prepare` is doing (and thus, as you can reckon, we made sure that `zsysd boot-prepare` is idempotent): turning previous **canmount=on** datasets to **noauto** when reverting and either creating new clones (boot from a saved state made of snapshots) or turning all clones associated to a state your already booted on from **canmount=noauto** to **on**.

This is what allows preserving clones and switching throughout them, allowing system bisection (reverting the revert!).

## ZFS user properties for ZSys

On top of that, we are using a bunch of user properties to achieve our additional functionalities on top of pure ZFS one. All ZSys related properties starts with `com.ubuntu.zsys:` prefix.

### ZFS user properties on system datasets

Let’s look at those on a filesystem dataset state (current or part of history):

```sh
$ zfs get com.ubuntu.zsys:last-booted-kernel,com.ubuntu.zsys:last-used,com.ubuntu.zsys:bootfs rpool/ROOT/ubuntu_e2wti1
NAME                      PROPERTY                            VALUE                               SOURCE
rpool/ROOT/ubuntu_e2wti1  com.ubuntu.zsys:last-booted-kernel  vmlinuz-5.4.0-29-generic            local
rpool/ROOT/ubuntu_e2wti1  com.ubuntu.zsys:last-used           1589438938                          local
rpool/ROOT/ubuntu_e2wti1  com.ubuntu.zsys:bootfs              yes                                 local
```

You can see 3 properties on those root pool:

#### Last Booted Kernel

This property is storing the last successful kernel with booted with. This is used when building the GRUB menu, in particular on state saves, to ensure the "Revert" entry boot with the exact same kernel version, even if not last available.

#### Last used

This is a timestamp of last successful boot. This is used for filesystem datasets related state to help the garbage collector assessing what to collect (on snapshots dataset, we only take their creation time of course) and printing an accurate time (related to your system timezone) in the GRUB menu.

#### Bootfs

This is a marker on the root and any datasets you want to mount in the initramfs. It identifies as well ZSys machines, so that the daemon avoid treating non ZSys installation.

Contrary to upstream ZFS initramfs support (very early in the machine boot), when reverting on a state associated to snapshot datasets, we only clone and mount datasets having `com.ubuntu.zsys:bootfs` property set to "yes" (local or inherited) if this property is set (we are thus still compatible with manually crafted ZFS installation). The remaining datasets will be cloned on early boot by the `zsysd boot-prepare` called in the ZFS mount generator.

Why do we do that? Remember complex layout like the server one? As a reminder, we end up with:

```sh
$ zsysctl show --full
[…]
System Datasets:
 - rpool/ROOT/ubuntu_e2wti1
 […]
 - rpool/ROOT/ubuntu_e2wti1/var
 - rpool/ROOT/ubuntu_e2wti1/var/lib/AccountsService
 - rpool/ROOT/ubuntu_e2wti1/var/lib/NetworkManager
 - rpool/ROOT/ubuntu_e2wti1/var/lib/apt
 - rpool/ROOT/ubuntu_e2wti1/var/lib/dpkg
User Datasets:
[…]
Persistent Datasets:
 - rpool/var/lib
 […]
 ```

Reminder: `rpool/ROOT/ubuntu_e2wti1/var/lib` has **canmount=off**.

Imagine now that on boot, we clone (for reverting) and mount dataset all children on `rpool/ROOT/ubuntu_e2wti1@…`. This would mount in the initramfs:

* `/` from `rpool/ROOT/ubuntu_e2wti1@…`
* `/var` from `rpool/ROOT/ubuntu_e2wti1/var@…`
* `/var/lib/AccountsService` from `rpool/ROOT/ubuntu_e2wti1/var/lib/AccountsService@…`
* `/var/lib/NetworkManager` from `rpool/ROOT/ubuntu_e2wti1/var/lib/NetworkManager@…`
* `/var/lib/apt` from `rpool/ROOT/ubuntu_e2wti1/var/lib/apt@…`
* `/var/lib/dpkg` from `rpool/ROOT/ubuntu_e2wti1/var/lib/dpkg@…`

Then, the machine starts booting and when reaching the ZFS mount generator, or `zfs-mount.service`, it will try to mount `/var/lib` (from `rpool/var/lib`) which will fail as we already have some `/var/lib/*` subdirectories created and mounted.

The same idea applies when you create any persistent dataset with **canmount=on** (like `rpool/var/lib`) and when some children (in term of mountpoint hierarchy) of this persistent dataset is a system dataset (like `rpool/var/lib/apt`).

This is why we set on `rpool/ROOT/ubuntu_e2wti1/var` **bootfs=no** (and in any direct children of `rpool/ROOT/ubuntu_e2wti1`) so that you can create any persistent datasets without any headache. This property avoids hardcoding "only mount the root dataset and no child" in the code and is more flexible for the advanced user.

```sh
$ zfs get com.ubuntu.zsys:bootfs rpool/ROOT/ubuntu_e2wti1/var
NAME                          PROPERTY                VALUE                   SOURCE
rpool/ROOT/ubuntu_e2wti1/var  com.ubuntu.zsys:bootfs  no                      local
```

### ZFS user properties on system datasets for snapshot datasets

We store slightly more property on snapshots and the syntax is different:

```sh
$ zfs get all rpool/ROOT/ubuntu_e2wti1@autozsys_08865s
[…]
rpool/ROOT/ubuntu_e2wti1@autozsys_08865s  com.ubuntu.zsys:bootfs              yes:local                           local
rpool/ROOT/ubuntu_e2wti1@autozsys_08865s  com.ubuntu.zsys:canmount            on:                                 local
rpool/ROOT/ubuntu_e2wti1@autozsys_08865s  com.ubuntu.zsys:last-booted-kernel  vmlinuz-5.4.0-26-generic:local      local
rpool/ROOT/ubuntu_e2wti1@autozsys_08865s  com.ubuntu.zsys:mountpoint          /:local                             local
rpool/ROOT/ubuntu_e2wti1@autozsys_08865s  com.ubuntu.zsys:last-used           1589438938                          inherited from rpool/ROOT/ubuntu_e2wti1
```

In addition to the previous listed user properties, you can see that we are storing `canmount` and `mountpoint` as user properties. We store them to save their value of both those properties when we are taking a state save. For instance, if we change the mountpoint property on `rpool/ROOT/ubuntu_e2wti1`, this would have impacted all of its snapshots like `rpool/ROOT/ubuntu_e2wti1@autozsys_08865s`. It means that we wouldn’t have a way to know what was the original value when we saved a particular state, which really corresponds to the given state. Similarly, we "set in stone" the last booted kernel property which would have changed over time otherwise, as inherited from `rpool/ROOT/ubuntu_e2wti1` last-booted-kernel value.

You can also see above that we append on the first 4 properties **:local** (local source) or just **:** (default property). This is used to know and recreate the exact source of inheritance chain for any property. Indeed, in ZFS:

* `rpool/ROOT/ubuntu_e2wti1@autozsys_08865s` inherits from `rpool/ROOT/ubuntu_e2wti1` for all its properties that can be inherited
* `rpool/ROOT/ubuntu_e2wti1/var` inherits from `rpool/ROOT/ubuntu_e2wti1`.
* However `rpool/ROOT/ubuntu_e2wti1/var@autozsys_08865s` inherits from … `rpool/ROOT/ubuntu_e2wti1/var` and not from `rpool/ROOT/ubuntu_e2wti1@autozsys_08865s`!

This inheritance chain of ZFS is logical when you reason dataset per dataset. However, we are creating machines with ZSys and it means that some properties that should logically inherit (if not overridden) from the root snapshots are instead inheriting from the immediate parent in term of ZFS logic.

To recreate a "machine" state inheritance logic, we append thus the source property value to the value for datasets. If we look at `rpool/ROOT/ubuntu_e2wti1/var@autozsys_08865s`:

```sh
$ zfs get all rpool/ROOT/ubuntu_e2wti1/var@autozsys_08865s
[…]
rpool/ROOT/ubuntu_e2wti1/var@autozsys_08865s  com.ubuntu.zsys:mountpoint          /var:inherited                      local
rpool/ROOT/ubuntu_e2wti1/var@autozsys_08865s  com.ubuntu.zsys:last-booted-kernel  vmlinuz-5.4.0-26-generic:inherited  local
rpool/ROOT/ubuntu_e2wti1/var@autozsys_08865s  com.ubuntu.zsys:bootfs              no:local                            local
rpool/ROOT/ubuntu_e2wti1/var@autozsys_08865s  com.ubuntu.zsys:canmount            off:local                           local
rpool/ROOT/ubuntu_e2wti1/var@autozsys_08865s  com.ubuntu.zsys:last-used           1589438938                          inherited from rpool/ROOT/ubuntu_e2wti1
```

Here, we can see that **canmount=off** was stored from the value on `rpool/ROOT/ubuntu_e2wti1/var` when the state was saved and it was a local property on this one: **off:local**. Same thing for the **bootfs** property. However, we can see that both the **mountpoint** or the **last-booted-kernel** were inherited properties (not set explicitly) from its parent, `rpool/ROOT/ubuntu_e2wti1/var` from `rpool/ROOT/ubuntu_e2wti1` and as such, `rpool/ROOT/ubuntu_e2wti1/var@autozsys_08865s` from `rpool/ROOT/ubuntu_e2wti1@autozsys_08865s`.

When doing a revert on a state associated to snapshot datasets, we restore any **local** values (suffix **:local**) by resetting the values, and ignore any inherited or default ones, as the normal new clone datasets inheritance will thus restore regular ZFS inheritance scheme. This is how we can recreate high fidelity revert, without being impacted by future changes you have made in ZFS datasets after the state save was taken. This is also one way we track changes in datasets layouts while still reverting to the previous available layout.

You can see that we don’t set any **last-used** properties on snapshot datasets and let them inherited from the parent. It doesn’t even have any **:** suffix! This is a value that we just ignore and use, as previously told, the snapshot creation time for those states made of snapshot datasets (they only apply to states made of filesystem datasets, like clones).

### ZFS user properties on user datasets

More simple than system datasets, user datasets have a couple of user properties:

```sh
$ zfs get all rpool/USERDATA/didrocks_wbdgr3
[…]
rpool/USERDATA/didrocks_wbdgr3  com.ubuntu.zsys:bootfs-datasets  rpool/ROOT/ubuntu_e2wti1         local
rpool/USERDATA/didrocks_wbdgr3  com.ubuntu.zsys:last-used        1589438938                       local
```

The second one is straightforward: this is the last used time for this dataset to help the garbage collector. Similarly to system states, if the state is made of snapshot datasets, the creation time is used.

`com.ubuntu.zsys:bootfs-datasets` is the mechanism which associates this user dataset with the `rpool/ROOT/ubuntu_e2wti1` state.
This is how user state made of filesystem datasets can be linked to system states and `zsysctl show --full` confirms this:

```sh
$ zsysctl show --full
Name:               rpool/ROOT/ubuntu_e2wti1
[…]
System Datasets:
 - bpool/BOOT/ubuntu_e2wti1
 - rpool/ROOT/ubuntu_e2wti1
[…]
User Datasets:
 User: didrocks
 - rpool/USERDATA/didrocks_wbdgr3
```

User state made of snapshots datasets are way easier: they are linked by the snapshot name, this is why this user state is linked to this system state:

```sh
$ zsysctl show --full
History:
  - Name:               rpool/ROOT/ubuntu_e2wti1@autozsys_8cjrb0
    System Datasets:
     - bpool/BOOT/ubuntu_e2wti1@autozsys_8cjrb0
     - rpool/ROOT/ubuntu_e2wti1@autozsys_8cjrb0
     […]
    User Datasets:
      User: didrocks
        - rpool/USERDATA/didrocks_wbdgr3@autozsys_8cjrb0
```

In the previous case, `@autozsys_8cjrb0` is the link. When we revert to that state and create clone filesystem datasets for a new read-write state, we thus associate the user dataset to that state by tagging the user dataset with `com.ubuntu.zsys:bootfs-datasets` matching the state name (this is done in the `zsysd boot-prepare` call from the ZFS mount generator).

#### Shared user states

What happens though if you revert your system state without reverting the user data? This is where things are becoming interesting: for each users, we take the previous active user state and retag it by appending the newly created system state.

You thus have an user state with multiple values, separated by **,**, like `com.ubuntu.zsys:bootfs-datasets=rpool/ROOT/ubuntu_e2wti1,rpool/ROOT/ubuntu_fa2wz74`. This is the mechanism that allows you to revert the revert, with the exact same user data! The user state is then shared.

However, each user state can be different, for instance you may want to add `rpool/USERDATA/didrocks_wbdgr3/tools` to only one system state. The annotation allows that by tagging `com.ubuntu.zsys:bootfs-datasets=rpool/ROOT/ubuntu_fa2wz74` on this one for instance.

We would thus end up with:
```sh
$ zsysctl show --full
Name:               rpool/ROOT/ubuntu_fa2wz74
[…]
User Datasets:
 User: didrocks
 - rpool/USERDATA/didrocks_wbdgr3
 - rpool/USERDATA/didrocks_wbdgr3/tools

[…]
History:
  - Name:               rpool/ROOT/ubuntu_e2wti1
    […]
    User Datasets:
      User: didrocks
        - rpool/USERDATA/didrocks_wbdgr3
```

The user state associated to `rpool/ROOT/ubuntu_fa2wz74` is now named `rpool/USERDATA/didrocks_wbdgr3.rpool.ROOT.ubuntu-fa2wz74` and his compounded of 2 filesystem datasets: `rpool/USERDATA/didrocks_wbdgr3` and `rpool/USERDATA/didrocks_wbdgr3/tools`.

The user state associated to `rpool/ROOT/ubuntu_e2wti1` is now named `rpool/USERDATA/didrocks_wbdgr3.rpool.ROOT.ubuntu-e2wti1` and only has one filesystem dataset: `rpool/USERDATA/didrocks_wbdgr3`.

Those 2 user states sharing some common user datasets are now logically separated and can be referenced directly, without any confusion, but the user. If you revert or remove a system state, we just untag the system state that needs to be detached: the user state can still be useful for other states and only the needed datasets (like the `/tools` one) will be deleted.

#### ZFS user properties on user datasets made of snapshot datasets

We already discussed that the snapshot name is the association between system and user states. Similarly to system state snapshots, we store `canmount` and `mountpoint` to freeze those properties in time when saving the state. We append the source after **:** to track parent and children association.

```sh
$ zfs get all rpool/USERDATA/didrocks_wbdgr3@autozsys_8cjrb0
[…]
rpool/USERDATA/didrocks_wbdgr3@autozsys_8cjrb0  com.ubuntu.zsys:canmount         on:local                         local
rpool/USERDATA/didrocks_wbdgr3@autozsys_8cjrb0  com.ubuntu.zsys:mountpoint       /home/didrocks:local             local
rpool/USERDATA/didrocks_wbdgr3@autozsys_8cjrb0  com.ubuntu.zsys:bootfs-datasets  rpool/ROOT/ubuntu_e2wti1         inherited from rpool/USERDATA/didrocks_wbdgr3
rpool/USERDATA/didrocks_wbdgr3@autozsys_8cjrb0  com.ubuntu.zsys:last-used        1589438938                       inherited from rpool/USERDATA/didrocks_wbdgr3
```

As you can see, we let the normal ZFS inheritance value on `com.ubuntu.zsys:bootfs-datasets` and `com.ubuntu.zsys:last-used` but those are ignored when restoring a state as the state association is made via snapshot name match and last used is taken from the snapshot dataset creation time.

You now have a way (until we grow a command for doing this) to manually enroll existing user datasets to a system once you understand this concept!

# Final thoughts

Phew! We hope that now that you have a complete pictures on all those concepts, you can really appreciate what we try to bring as our ubuntu ZFS on root experience and how. ZSys provides a way to manage and monitor the system, while doing automated state saving to offer you reliable revert capability. We are paving the first steps to provide a strong and bullet-proof desktop system to our audience. The main idea is that most of the features are managed automatically for you, and we will provide more and more facilities in the future.

If you are interested in reading further ahead, you can check our [in progress specification](https://docs.google.com/document/d/1oV5-ef-fqzML4MGd2LAHRcLdR0USKkOmrJW-AP0CmC4/edit) and [follow our github project](https://github.com/orgs/ubuntu/projects/1). This is a good place, as [the upstream ZSys repo is](https://github.com/ubuntu/zsys), to see how you can contribute to this effort and help shaping the future on ZFS on root on ubuntu! This is in addition to all the enhancements, maintenance and fixes that are heading Ubuntu 20.04 LTS right now.

I hope this read was pleasing to you and that understanding the internals and challenges has highlighted why built this tool, how much thoughts and great care we took in crafting the whole system. Of course, there would be a lot more to say, like how we built our extensive testsuite on ZSys itself with the internal in-memory mock and real ZFS system stack, how we have hundreds of tests on our grub menu generation, how we handled some optimizations over the [go-libzfs package](https://github.com/bicomsystems/go-libzfs) and much more! But we have to stop at point and this is the good place for it. We can always continue the conversation via the [dedicated Ubuntu discourse thread](https://discourse.ubuntu.com/t/).
