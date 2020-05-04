---
title: "ZFS focus on Ubuntu 20.04 LTS: ZSys commands for state management"
date: 2020-05-01T09:36:19+02:00
tags: [ "pu", "ubuntu", "zfs" ]
banner: "/images/focal/time_machine_driver.jpg"
type: "post"
manualdiscourse: "https://discourse.ubuntu.com/t/XXXXX"
draft: True
---

# ZFS focus on Ubuntu 20.04 LTS: ZSys commands for state management

In the [previous article](https://didrocks.fr/TODO), we presented the core state concept of [ZSys](https://github.com/ubuntu/zsys). This one will focus more on how you can manage states, when we are taking them automatically or manually and way more general considerations. A lot to cover, so let's go!

## zsysctl commands related to states

Most of commands related to states are grouped under the `zsysctl machine` and `zsysctl state` subcommands.

The first one is `zsysctl show`  (alias of `zsysctl machine show`). It will detail any system and user states for the targeted machine ID, blank being the current machine. `zsysctl machine list` will display any available machine ID you can use.

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

For those interested, we are going to see all this more in details in the advanced final posts, but what can we already see here?

* We have system states and states for each users.
* The history for each history state have time associated when they were taken or last booted on.
* We have more states saved from our connected user (didrocks) than other users or system states.
* A lot of state saves contains **@autozsys_** which are automated user or system state saves.
* People can save states manually, which is exactly what we are going to cover just now!

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

System states can be saved by any administrator or anyone allowed by our polkit scheme to write system state. With our default policy, non administrators but sudoers will be presented a polkit authorization window:
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

Note that you don’t necessarily need to set a name even when you save a state manually. In case you don’t specify one, we will generate one for you with the **@autozsys_** prefix, which will qualify it for garbage collection.

You will have some manual state saves as you request them, but most of those that are see are going to be generated automatically.

### When do we automatically save states?

This naturally triggers this question. How can we try to meaningfully decide when we should save state?

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

Users have thus some states save created when a system state save is requested. However, we are saving regularly more states as this is where most of your important data are.

A systemd timer unit is responsible of taking hourly state save for any connected users:
```sh
$ systemctl --user list-timers
NEXT                         LEFT       LAST                         PASSED    UNIT                      ACTIVATES                  
Thu 2020-04-30 15:44:33 CEST 45min left Thu 2020-04-30 14:44:33 CEST 14min ago zsys-user-savestate.timer zsys-user-savestate.service

1 timers listed.
```
We can see there next time the user state save will be taken and which systemd unit is responsible for this.


### Removing state

We will see in the next blog post the garbage collection policy to remove any automated snapshots (those starting with **@autozsys_**), but you may want, for disk usage matters or needs to immediately remove some state, there is of course a command for that!

We have `zsysctl state save STATE_NAME` to create a new state and unsurprisingly, we have… `zsysctl state remove STATE_NAME` to delete one! However, removing a state can be a little bit more complex as they can have dependencies.

#### User State without any dependency

Removing a user state (simple snapshot without any dependency) is easy:
```sh
$ zsysctl state remove nc0t07
```

-> This is a simple case which removed for the current user the state `rpool/USERDATA/ubuntu_e2jj0s@autozsys_nc0t07` and all its associated ZFS datasets.

Similarly, we can remove any manual user state with its name:
```sh
$ zsysctl state remove MyUserSave
```

Removing other user’s state (with `--user othername`) will trigger a polkit prompt for access permission on other user state write.

Note that we have many ways to name a state that is removable. You can use:

* its full qualifying name (here `rpool/USERDATA/didrocks_e2jj0s@autozsys_nc0t07`)
* for snapshot-based state save, anything after **@** (`MyUserSnapshot` for `rpool/USERDATA/didrocks_e2jj0s@MyUserSnapshot`) if it's unique in this namespace (for a given system or for the whole system)
* for automated state save. anything after **autozsys_** (`nc0t07` for `rpool/USERDATA/didrocks_e2jj0s@autozsys_nc0t07`) with the same uniqueness principle
* for filesystem-based state save, anything after **_** (`e2jj0s` for `rpool/USERDATA/didrocks_e2jj0s`) with the same uniqueness principle
* for filesystem-based state save, its basename (`didrocks_e2jj0s` for `rpool/USERDATA/didrocks_e2jj0s`) with the same uniqueness principle
* for shared user states between multiple system states, only the full path is a valid state, like in `rpool/USERDATA/didrocks_e2jj0s-rpool.ROOT.ubuntu-idjvq9`.

If multiple states are matching a short form, you will be informed:
```sh
$ zsysctl state remove MyUserSave
ERROR couldn't remove user state MyUserSave: Couldn't find state: multiple states are matching MyUserSave:
  - rpool/USERDATA/didrocks_e2jj0s@MyUserSave (2020-04-30 09:20:05)
  - rpool/USERDATA/didrocks_a3bbe3@MyUserSave (2020-04-30 05:20:05)
  Please use full state path.
```

In that case, only the full qualifying name can be used.

#### System State without any dependency

We can remove a manual snapshot for the whole system adding `--system` to the call. This triggers an access permission polkit request for writing system state. Note that the bootloader menu is also refreshed.

```sh
$ zsysctl state remove --system mySavedState
INFO Updating GRUB menu
```

Any associated user state associated to "mySavedState" are also removed.

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

Same prompt but different effect when user states are associated to multiple system states:

```sh
$ zsysctl state remove rpool/USERDATA/didrocks_e2jj0s-rpool.ROOT.ubuntu-idjvq9
rpool/USERDATA/didrocks_e2jj0s will be detached from system state rpool/ROOT/ubuntu_idjvq9

Would you like to proceed [y/N]? y
```

In this last cases, the data themselves won’t be deleted. Indeed, the user state was associated to multiple system states (case of reverting the system without reverting user data. More on that later on.). As there is still at least one system state associated to it, the data are still needed. If the user state is now associated to only one system state, it will be renamed to the simpler `rpool/USERDATA/didrocks_e2jj0` naming scheme for its full qualifying name.

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

Any confirmation (if you are sure about what you are doing) can be bypassed by the force - `-f` - flag.

As you can see, there are lot of cases and complex handling of states for removal! We spent hours and hours to streamline and ensure that removing manually a state is done properly, taking into account dependencies and simplifying as much as possible the user experience. All this is backed up with a very extensive test suite.

We are creating a huge number of state saves automatically for you. but we don’t want our users having to remove them manually. This is why we had to draft a garbage collection strategy so that your disk doesn’t end up being full quickly. This is an interesting topic which will be, coincidentally, the next one! See you there :)

Meanwhile, join the discussion via the [dedicated Ubuntu discourse thread](https://discourse.ubuntu.com/t/).