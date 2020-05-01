---
title: "ZFS focus on Ubuntu 20.04 LTS: ZSys state management"
date: 2020-05-01T09:36:19+02:00
tags: [ "pu", "ubuntu", "zfs" ]
banner: "/images/focal/time_machine.png"
type: "post"
manualdiscourse: "https://discourse.ubuntu.com/t/XXXXX"
draft: True
---

# ZFS focus on Ubuntu 20.04 LTS: ZSys sate management

After [our previous general presentation](https://didrocks.fr/TODO) of [ZSys](https://github.com/ubuntu/zsys), it’s """time""" to deep dive to one of its main predominant feature: state management!


## Why calling that state and not simply ZFS snapshots?

A little technical detour first, as this question will necesserally arise, especially from those familiar with ZFS concepts.

### What is a state, and what’s the difference with snapshots?

We have purposively chosen the "state" terminology to prevent system administrator and in general, all those familiar with ZFS to confuse if with snapshot datasets.

Basically a state is a set of datasets, all frozen in time (apart from the current state), which regrouped together forms a system "state" that you can chose to reboot on.

Those group of datasets can be either made of snapshot datasets (read only) (which is what most of advanced ZFS users will expect), but it can also be filesystem datasets (read write) (datasets clone of the current state datasets). You can boot to any of those.

### Why not only relying on snapshots?

This choice was made to go beyond what a simple `zfs rollback` can do. The limitations of rollbacking is that the current filesystem itself is revert (remember, it’s read write while snapshots are read only), and any intermediate snapshot datasets are destroyed. So, current and all intermediate states wouldn’t have been able to recover after a revert. We wanted to avoid being blocked by those limitations and this is why navigating between ZSys states includes using ZFS cloning (creating filesystem datasets (read write) from all snapshot datasets (read only) compounding a particular state) and then navigating between those ZFS filesystem datasets. We promote any correctly booted reverted state to migrate snapshots over it.

Creating this non destructive revert action allows crazy dreams: you can imagine reverting a revert! And this also unlock system bisection!

We separate as well system states to user states: system states have matching user states (one for each available user on the system). However, we can have finer grain user states (this is where most of your interesting work is going, afterwards!), while system states are limited to have reliable boot and working machine.

In addition to this, we have properties associated to each system state (via dataset user properties). This goes from simply storing mountpoint properties when the state save was requested, even if you changed it afterwards on the parent dataset. As we stored this as user properties, we are able to then reapply original base filesystem dataset properties, to ensure better fidelity. Similarly, we will reboot with the **exact same kernel** you booted with when you saved the state, even if this wasn’t the latest available version and if it’s been purged on the current state as well.

This is for all those reasons that we deliberately used the term "state" instead of snapshots. A state can be based on a collection (and not only one) dataset snapshots with the same name. However, not all states are (after a revert for instance). We hope this sheds some lights to advanced ZFS users on those particular decisions, and what ZSys provides on top of ZFS awesome core capabilities.

## Reverting to a previous system state

And here the meat! Managing states can be fun, but when managing is only done in the purpose of managing, there is no real point! Let’s exercise the whole goal of this state concept now: reverting!

Our grub bootloader (available via [Escape] or [Shift] press on boot) will allow you to revert the system (and optionally user) states on demand! An history entry will propose you to boot on an older state of your system.

![History option available](/images/focal/zfs_grub_history_entry.png)

Entering it will propose you the different system states you can revert to:

![History with ZSys](/images/focal/zsys_grub_back_to_the_future.png)

You can see there multiple times and also naming! What are those?
- Automated system state saves (see below on when we take automated system snapshots) only have their creation time used (for instance **Revert to 29/04/20 @ 13:37**). Their name is indeed meaningless.
- We display the user selected name on manual system state save, like **Revert to Manual Snapshot on …**
- If previous state save had a different release than the current one (and everytime the release is different from the state listed above, we are listing the release name), we display it as well: **Revert to Ubuntu 19.10 on …**
- If a manual clone is used as state save, we are using its state basename to indicate that: **Revert to ubuntu_myid on …**
- And of course, we can have a mix of the all of this, with name and different release like in the fictive **Revert to Distant past, Ubuntu 0.1885 on …**

Once you select to which state you can revert to, you have multiple options offered to you:
![History with ZSys](/images/focal/zsys_grub_revert_options.png)

Reverting the system will, as we explained previously, revert (if it’s a ZSys state save) with the exact metadata on every datasets constituting the state. In addition to this, we will boot with the exact same kernel that was last successfully booted with.

You can only revert the system dataset, in that case, only the whole system is reverted, but not non persistent datasets (we have none by default on the desktop, but we will detail that in a later post) and the user data are kept as current. It means though that you are taking the risk of reverting to older version of some software not being forward-compatible with your user data. It means though that your user data content will be the very last available data you created. (A technical note: if you select that option on filesystem dataset associate states (basically no snapshot), then user data will be considered as "current").

The user state will be considered then as shared between 2 system states, which has some implications in case of removal as you will see below.

The second main option "Revert system and user data" is to revert the whole system, including user data. It means that the home directory of *all users* on the system will be reverted to when this system state was last saved. You will have ways to access manually the newly created content (see just below), but the default will ensure that you have a very robust and compatible user data with the system softwares that are running.

Finally, there are the recovery option which are similar to the ubuntu "Advanced options" for recovery mode for each of this state.

And as we illustrated previously, this work out of the box, including reverting to **previous ubuntu release** with a high fidelity, and all this without loosing newer states (as not being a destructive action)!

### Having access to previous user states

We have plan for future cycles to integrate this more deeply with the system (login manager, file manager). For now, outside of reverting the whole system to a previous state (alongside users), there are other ways to access to previous revision of files.
In your datasets mountpoint, you can navigate to the `.zfs` hidden directory (which is really hidden: even showing all hidden directories won’t display it or `ls -a` either).

Typically, you can press "[Ctrl]+l" in Files, and append `/.zfs` to it. You will see a `snapshots` directory available to you, with one folder for each snapshots.

![Snapshot .zfs directory](/images/focal/zsys_manually_access_user_snapshots.png)

You can then navigate and restore any file by copying/pasting here (those directories are read only).

![Restore user file from .zfs directory](/images/focal/zsys_manually_restore_user_file.png)

Note that you can only find here history user states that are made of snapshot datasets which migrated on current state with this method. Having access to filesystem-based states or snapshots on those filesystem-based states involve manually mounting the datasets and navigating there. This is why we have plans to make all that easier in the future, when you can navigate based on time. :)

### Boot success and failure

You will tell us that pressing [Escape] or [Shift] on boot to show up grub isn’t the most discoverable feature and you are right.

However, in case of boot failing, the next boot will show up grub by default and you will have those "History entries" available to you! We define successful boot by systemd having reached the `default.target` target. On desktop, default.target is a symlink to graphical.target:
```sh
$ systemctl status default.target
● graphical.target - Graphical Interface
     Loaded: loaded (/lib/systemd/system/graphical.target; static; vendor preset
     Active: active since Thu 2020-04-30 08:39:17 CEST; 9h ago
       Docs: man:systemd.special(7)

avril 30 08:39:17 casanier systemd[1]: Reached target Graphical Interface.
```

This is how we set LastUsed timestamp, record which kernel was used and optionally rebuild the bootloader menu if needed (basically, if you reverted and current state is different):
```sh
$ systemctl status zsys-commit.service 
● zsys-commit.service - Mark current ZSYS boot as successful
     Loaded: loaded (/lib/systemd/system/zsys-commit.service; enabled; vendor preset: enabled)
     Active: active (exited) since Thu 2020-04-30 08:39:23 CEST; 9h ago
    Process: 3249 ExecStart=/sbin/zsysctl boot commit (code=exited, status=0/SUCCESS)
   Main PID: 3249 (code=exited, status=0/SUCCESS)

avril 30 08:39:17 casanier systemd[1]: Starting Mark current ZSYS boot as successful...
avril 30 08:39:18 casanier zsysctl[3249]: INFO Updating GRUB menu
avril 30 08:39:23 casanier systemd[1]: Finished Mark current ZSYS boot as successful.
```

### Editable grub menu

We know advanced users like to press "e" to edit grub menu or tweak their settings on some boot at their convenience. 
We took great care so that it’s easily editable by them: our functions have a set of argument with readable names.
Pressing "e" when entering a history state line:

![Edit one history entry](/images/focal/zsys_grub_edit1_line.png)

Then, on each revert entry, you can edit it as well and replace the variable if you prefer directly editing them here:

![Edit one revert entry](/images/focal/zsys_grub_edit2_line.png)

The use of functions with readable names should be of help for this audience. In addition to this, we didn’t use that function for normal boots or advanced options to not confuse them who will have entries without any variable.

As a reminder, those are the changes which allowed us to reduce `grub.cfg` from 7329 lines for 100 state saves that grub didn’t really like to 728, each new history entry being 3 additional lines instead of 50, and reducing from 80s to load the history entry to being instantaneous.

## zsysctl commands related to states

Most of commands related to states are grouped under the `zsysctl machine` and `zsysctl state`.

The first one is `zsysctl show`  (alias of `zsysctl machine show`). It will show any system and user states for the targeted machine ID, blank being the current machine. `zsysctl machine list` will display any available machine ID you can use.

```sh
$ zsysctl show
Name:           rpool/ROOT/ubuntu_qiq15o
ZSys:           true
Last Used:      current
History:        
  - Name:       rpool/ROOT/ubuntu_qiq15o@autozsys_iynia9
    Created on: 2020-04-30 14:21:47
  - Name:       rpool/ROOT/ubuntu_qiq15o@mySavedState
    Created on: 2020-04-30 13:22:04
  - Name:       rpool/ROOT/ubuntu_idjvq9
    Created on: 2020-04-29 15:19:41
  - Name:       rpool/ROOT/ubuntu_idjvq9@autozsys_cwfpcd
    Created on: 2020-04-29 11:30:39
  - Name:       rpool/ROOT/ubuntu_qiq15o@autozsys_fgw65h
    Created on: 2020-04-29 10:15:48
  - Name:       rpool/ROOT/ubuntu_qiq15o@autozsys_wjdzc4
    Created on: 2020-04-29 10:10:26
  - Name:       rpool/ROOT/ubuntu_zums7p
    Created on: 2020-04-29 09:41:28
Users:
  - Name:    didrocks
    History: 
     - rpool/USERDATA/didrocks_e2jj0s@autozsys_iynia9 (2020-04-30 14:21:48)
     - rpool/USERDATA/didrocks_e2jj0s@mySavedState (2020-04-30 13:22:04)
     - rpool/USERDATA/didrocks_e2jj0s@autozsys_nc0t07 (2020-04-30 14:20:53)
     - rpool/USERDATA/didrocks_e2jj0s@autozsys_wul29z (2020-04-30 13:20:01)
     - rpool/USERDATA/didrocks_e2jj0s@autozsys_7urchl (2020-04-30 12:20:01)
     - rpool/USERDATA/didrocks_e2jj0s@autozsys_okd3jd (2020-04-30 11:20:01)
     - rpool/USERDATA/didrocks_e2jj0s@autozsys_nvy9n8 (2020-04-30 10:20:01)
     - rpool/USERDATA/didrocks_e2jj0s@autozsys_ltvyzi (2020-04-30 09:20:05)
     - rpool/USERDATA/didrocks_e2jj0s@autozsys_nr4qgi (2020-04-29 18:21:03)
     - rpool/USERDATA/didrocks_e2jj0s@autozsys_tznx3z (2020-04-29 17:21:05)
     - rpool/USERDATA/didrocks_e2jj0s@autozsys_ha8ydo (2020-04-29 16:21:08)
     - rpool/USERDATA/didrocks_e2jj0s-rpool.ROOT.ubuntu-idjvq9 (2020-04-29 15:21:01)
     - rpool/USERDATA/didrocks_fsmk5b@autozsys_f94bcr (2020-04-29 15:18:20)
     - rpool/USERDATA/didrocks_fsmk5b@autozsys_k1lrce (2020-04-29 15:15:01)
     - rpool/USERDATA/didrocks_fsmk5b@autozsys_voi74a (2020-04-29 14:15:01)
     - rpool/USERDATA/didrocks_fsmk5b@autozsys_xx5xxz (2020-04-29 13:15:01)
     - rpool/USERDATA/didrocks_fsmk5b@autozsys_lww7fh (2020-04-29 12:15:01)
     - rpool/USERDATA/didrocks_e2jj0s@autozsys_cwfpcd (2020-04-29 11:30:39)
     - rpool/USERDATA/didrocks_fsmk5b@autozsys_w2929w (2020-04-29 11:15:06)
     - rpool/USERDATA/didrocks_e2jj0s@autozsys_fgw65h (2020-04-29 10:15:48)
     - rpool/USERDATA/didrocks_e2jj0s@autozsys_wjdzc4 (2020-04-29 10:10:26)
     - rpool/USERDATA/didrocks_e2jj0s@autozsys_2y0bud (2020-04-29 09:41:56)
     - rpool/USERDATA/didrocks_fsmk5b (2020-04-29 09:41:28)
  - Name:    root
    History: 
     - rpool/USERDATA/root_e2jj0s@autozsys_iynia9 (2020-04-30 14:21:48)
     - rpool/USERDATA/root_e2jj0s@mySavedState (2020-04-30 13:22:04)
     - rpool/USERDATA/root_e2jj0s-rpool.ROOT.ubuntu-idjvq9 (2020-04-29 15:21:01)
     - rpool/USERDATA/root_e2jj0s@autozsys_cwfpcd (2020-04-29 11:30:39)
     - rpool/USERDATA/root_e2jj0s@autozsys_fgw65h (2020-04-29 10:15:48)
     - rpool/USERDATA/root_e2jj0s@autozsys_wjdzc4 (2020-04-29 10:10:26)
     - rpool/USERDATA/root_fsmk5b (2020-04-29 09:41:28)
```

For those interesting, later in this post, we are going to see things more in details, but what can we already see here?
* We have system states and states for each users.
* The history for each history state have time associated when they were taken or last booted on.
* We have more states saved from our connected user (didrocks) than other users or system states.
* A lot of state saves contains "@autozsys_" which are automated user or system state saves.
* People can save states manually.

### Saving states manually

Any user can save its user state at any time via the `zsysctl save` command which is an alias for `zsysctl state save`:
```sh
$ zsysctl save mySavedState
Successfully saved as "mySavedState"
$ zsysctl show
[…]
Users:
  - Name:    didrocks
    History: 
     - rpool/USERDATA/didrocks_e2jj0s@mySavedState (2020-04-30 13:22:04)
```
This won’t ask for any additional permissions and will save current user’s state.

System states can be saved by any administrator or anyone allowed by our polkit scheme to write system state. With our default policy, not administrators but sudoers will be presented a polkit authorization window:
![Polkit request for performing system write operation](/images/focal/zsys_polkit.png)

```sh
$ zsysctl save --system MySystemSave
INFO Updating GRUB menu                           
Successfully saved as "MySystemSave"
$ zsysctl show
Name:           rpool/ROOT/ubuntu_qiq15o
ZSys:           true
Last Used:      current
History:        
  - Name:       rpool/ROOT/ubuntu_qiq15o@MySystemSave
    Created on: 2020-04-30 14:04:31
  […]
Users:
  - Name:    didrocks
    History: 
     - rpool/USERDATA/didrocks_e2jj0s@MySystemSave (2020-04-30 14:04:32)
     […]
  - Name:    root
    History: 
     - rpool/USERDATA/root_e2jj0s@MySystemSave (2020-04-30 14:04:32)
     […]
```

Note that you don’t necesserally need to set a name even when you save a state manually. In case you don’t specify one, we will generate one for you with the **@autozsys_** prefix, which will qualify it for garbage collection.

You will have manual state save, but most of those that are see are going to be generated automatically.

### When do we automatically save states?

This naturally triggers this question. How can we try to meaningfully select at what occurrence we should save state?

#### System states

Each time you install, remove or upgrade your packages, a state save is automatically taken by the system. This is done by apt transactions. Note that we have some specificities in ubuntu with background updates (via `unattended-upgrades`) which splits up upgrades in multiple apt transactions. We were able to group that in only one system saving. We split it up in two parts (saving state and rebuilding bootloader menu) as you can see when running the apt command manually:
```sh
$ apt install foo
[…]
INFO Requesting to save current system state      
Successfully saved as "autozsys_iynia9"
[installing/remove/upgrading package(s)]
INFO Updating GRUB menu
```

You can see the output of the automated state saving name. This creates a system state save, which compounds as well user state with the same name. Note that the bootloader menu is also refreshed.

#### User states

Users have thus some states save created when a system state save is requested. However, it has more states saved regularly by it.

A systemd timer unit is responsible of taking hourly state save for any connected users:
```sh
$ systemctl --user list-timers
NEXT                         LEFT       LAST                         PASSED    UNIT                      ACTIVATES                  
Thu 2020-04-30 15:44:33 CEST 45min left Thu 2020-04-30 14:44:33 CEST 14min ago zsys-user-savestate.timer zsys-user-savestate.service

1 timers listed.
```
We can see there next time the user state save will be taken and which systemd units have a role in this.


### Removing state

We will see in the next blog post the garbage collection policy to remove any automated snapshots (those starting with **@autozsys_**), but you may want, for disk usage or need purpose to immediately remove some state, there is of course a command for that!

We have `zsysctl state save STATE_NAME` to create a new state and unsurprinsingly, we have… `zsysctl state remove STATE_NAME` to delete one! However, states can be a little bit more complex as they can have dependencies.

#### User State without any dependency

Removing a user state (simple snapshot without any dependency) is easy:
```sh
$ zsysctl state remove nc0t07
```

-> This is a simple case which removed for the current user the state `rpool/USERDATA/ubuntu_e2jj0s@autozsys_nc0t07` and all its associated ZFS datasets.

Similarily, we can remove any manual user state with its name:
```sh
$ zsysctl state remove MyUserSave
```

Removing other user’s state (with `--user othername`) will trigger a polkit prompt for access permission on other user state write.

Note that we have many way to name a state that is removable. You can use:
* its full qualified name (here `rpool/USERDATA/didrocks_e2jj0s@autozsys_nc0t07`)
* for snapshot-based state save, anything after **@** (`MyUserSnapshot` for `rpool/USERDATA/didrocks_e2jj0s@MyUserSnapshot`)
* for automated state save. anything after **autozsys_** (`nc0t07` for `rpool/USERDATA/didrocks_e2jj0s@autozsys_nc0t07`)
* for filesystem-based state save, anything after **_** (`e2jj0s` for `rpool/USERDATA/didrocks_e2jj0s`)
* for filesystem-based state save, its basename (`didrocks_e2jj0s` for `rpool/USERDATA/didrocks_e2jj0s`)
* for shared user states between multiple system states, only the full path is a valid state, like in `rpool/USERDATA/didrocks_e2jj0s-rpool.ROOT.ubuntu-idjvq9`.

If multiple states are matching a short prefix one, you will be informed:
```sh
$ zsysctl state remove MyUserSave
ERROR couldn't remove user state MyUserSave: Couldn't find state: multiple states are matching MyUserSave:
  - rpool/USERDATA/didrocks_e2jj0s@MyUserSave (2020-04-30 09:20:05)
  - rpool/USERDATA/didrocks_a3bbe3@MyUserSave (2020-04-30 05:20:05)
  Please use full state path.
```

In that case, only the full qualifying name can be used.

#### System State without any dependency

We can similarly to the previous case take a manual snapshot for the whole system adding `--system` to the call. This triggers
an access permission polkit request for writing system state. Note that the bootloader menu is also refreshed.

```sh
$ zsysctl state remove --system mySavedState
INFO Updating GRUB menu
```

Any associated user state saves to "mySavedState" are also removed.

#### User State without any dependency but linked to a system state

A confirmation will be requested upon any suppression request if the user state is linked to a system state:

```sh
$ zsysctl state remove rpool/USERDATA/didrocks_e2jj0s@autozsys_iynia9
rpool/USERDATA/didrocks_e2jj0s@autozsys_iynia9 will be detached from system state rpool/ROOT/ubuntu_qiq15o@autozsys_iynia9

Would you like to proceed [y/N]? y
```

Warning: this system state may now be unbootable or unrevertable. In any case, you will not have this user data available anymore for this saved state after its deletion. It’s better to completely suppress the system state directly.

Same is available on filesystem dataset user states associated to a system state:
```sh
$ zsysctl state remove --user=root rpool/USERDATA/root_fsmk5b
rpool/USERDATA/root_fsmk5b will be detached from system state rpool/ROOT/ubuntu_zums7p

Would you like to proceed [y/N]? y
```

Same prompt but different effect when user states associated to multiple system states:

```sh
$ zsysctl state remove rpool/USERDATA/didrocks_e2jj0s-rpool.ROOT.ubuntu-idjvq9
rpool/USERDATA/didrocks_e2jj0s will be detached from system state rpool/ROOT/ubuntu_idjvq9

Would you like to proceed [y/N]? y
```

In this last cases, the data themselves won’t be deleted. Indeed, the user state was associated to multiple system states (case of reverting the system without reverting user data. More on that later on.). As there is still at least one system state associated to it, the data are still needed. If the user state is now associated to only one system state, it will be renamed to the simpler `rpool/USERDATA/didrocks_e2jj0` naming scheme.

We will detail more information in a future blog post on the dataset layout and links between user and system datasets.


### User or system states with state dependencies

If a state has some state dependencies, confirmation and list of all the states that are going to be deleted are listed:
```sh
$ zsysctl state remove rpool/USERDATA/didrocks_fsmk5b
rpool/USERDATA/didrocks_fsmk5b has a dependency linked to some states:
  - rpool/USERDATA/didrocks_fsmk5b@autozsys_a45fdz (2020-04-30 16:53:51)

Would you like to proceed [y/N]? y
```
Deleting the main snapshots will delete any dependencies.


This can be even more complex if this is an association of states that have dependencies, and some of the dependencies are linked to system states:
```sh
$ zsysctl state remove rpool/USERDATA/didrocks_e2jj0s@autozsys_cwfpcd
rpool/USERDATA/didrocks_e2jj0s@autozsys_cwfpcd will be detached from system state rpool/ROOT/ubuntu_idjvq9@autozsys_cwfpcd
rpool/USERDATA/didrocks_e2jj0s@autozsys_cwfpcd has a dependency linked to some states:
  - rpool/USERDATA/didrocks_fsmk5b (2020-04-29 09:41:28) to remove. Currently linked to rpool/ROOT/ubuntu_zums7p
  - rpool/USERDATA/didrocks_fsmk5b@autozsys_f94bcr (2020-04-29 15:18:20)
  - rpool/USERDATA/didrocks_fsmk5b@autozsys_k1lrce (2020-04-29 15:15:01)
  - rpool/USERDATA/didrocks_fsmk5b@autozsys_lww7fh (2020-04-29 12:15:01)
  - rpool/USERDATA/didrocks_fsmk5b@autozsys_w2929w (2020-04-29 11:15:06)
  - rpool/USERDATA/didrocks_fsmk5b@autozsys_xx5xxz (2020-04-29 13:15:01)
  - rpool/USERDATA/didrocks_fsmk5b@autozsys_voi74a (2020-04-29 14:15:01)

Would you like to proceed [y/N]? y
```

Note that the same goes for system states. If a system states deletes some user states as well, all user states dependencies will be listed for any attached users! The list can be long, but we try to always list everything in an understandable and clear articulation for our users.

#### User or system states with manual ZFS clone dependencies

If the user has done a manual zfs clone via the `zfs` commands on any datasets composing any states (remember that a state is a set of datasets) this will be detailed to the user before confirming the deletion:
```sh
$ zsysctl state remove rpool/USERDATA/didrocks_fsmk5b@autozsys_a45fdz
rpool/USERDATA/didrocks_fsmk5b@autozsys_a45fdz has a dependency on some datasets:
  - rpool/manualclone

Would you like to proceed [y/N]? y
```

And finally, this goes as well when removing a state where one of the state dependencies has a dataset with some dependencies:
```sh
$ zsysctl state remove rpool/USERDATA/didrocks_fsmk5b
rpool/USERDATA/didrocks_fsmk5b will be detached from system state rpool/didrocks/ubuntu_zums7p
rpool/USERDATA/didrocks_fsmk5b has a dependency linked to some states:
  - rpool/USERDATA/didrocks_fsmk5b@autozsys_a45fdz (2020-04-30 16:53:51)
rpool/USERDATA/didrocks_fsmk5b has a dependency on some datasets:
  - rpool/manualclone

Would you like to proceed [y/N]? y
```

This will delete `rpool/manualclone` in addition to the depending and targeted states.

## Final note on states

Any confirmation (if you are sure about what you are doing) can be bypassed by the force `-f` flag.

As you can see, there are lot of cases and complex handling of states for removal! We spent hours and hours to streamline and ensure that removing manually a state is done properly, taking into account dependencies and simplifying as much as possible the user experience. All this is backed up with a very extensive test suite.

We are creating a huge number of state saves automatically for you. but we don’t want our users having to remove them manually. This is why we had to draft a garbage collection strategy so that your disk doesn’t end up being full quickly. This is an interesting topic which will be, coincidently, the next one! See you there :)

Meanwhile, join the discussion via the [dedicated Ubuntu discourse thread](https://discourse.ubuntu.com/t/).