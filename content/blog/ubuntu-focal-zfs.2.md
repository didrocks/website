---
title: "ZFS focus on Ubuntu 20.04 LTS: Zsys general presentation"
date: 2020-05-01T09:36:19+02:00
tags: [ "pu", "ubuntu", "zfs" ]
banner: "/images/focal/zsys_architecture.png"
type: "post"
manualdiscourse: "https://discourse.ubuntu.com/t/XXXXX"
draft: True
---

# ZFS focus on Ubuntu 20.04 LTS: Zsys general presentation

In [our previous blog post](https://didrocks.fr/TODO), we presented some enhancements and differences between Ubuntu 19.10 and Ubuntu 20.04 LTS in term of ZFS support. We only alluded to [Zsys](https://github.com/ubuntu/zsys), our ZFS system helper, which is now installed by default when selecting ZFS on root installation on the Ubuntu Desktop.

It’s now time to shed some lights on it and explain what exactly Zsys is bringing to you.

## What is Zsys?

We called Zsys a ZFS system (hence its name) helper. It can be first seen as a boot environment, which are popular in the OpenZFS community, to help you booting on previous revision of your system (basically snapshots) in a coherent manner. However, Zsys goes beyond than that by providing regular user snapshots, system garbage collection and much more that we will detail below!

## System state saving

We will go deeper in details about what is a state and how it behaves, but as we want to get exhaustive in term of Zsys features in this post, here is a little introduction.

Each time you install, remove or upgrade your packages, a state save is automatically taken by the system. This is done by apt transactions. Note that we have some specificities in ubuntu with background updates (via `unattended-upgrades`) which splits up upgrades in multiple apt transactions. We were able to group that in only one system saving. We split it up in two parts (saving state and rebuilding bootloader menu) as you can see when running the apt command manually:
```sh
$ apt install foo
[…]
INFO Requesting to save current system state      
Successfully saved as "autozsys_ip60to"
[installing/remove/upgrading package(s)]
INFO Updating GRUB menu
```

You can find them now stored on the system:
```sh
$ zsysctl show
Name:           rpool/ROOT/ubuntu_e2wti1
ZSys:           true
Last Used:      current
History:        
  - Name:       rpool/ROOT/ubuntu_e2wti1@autozsys_cgym7c
    Created on: 2020-04-28 12:23:12
  - Name:       rpool/ROOT/ubuntu_e2wti1@autozsys_kho1px
    Created on: 2020-04-28 12:16:34
  - Name:       rpool/ROOT/ubuntu_e2wti1@autozsys_w2kfiv
    Created on: 2020-04-28 11:52:46
  - Name:       rpool/ROOT/ubuntu_e2wti1@autozsys_ixfcpk
    Created on: 2020-04-28 11:52:24
  - Name:       rpool/ROOT/ubuntu_e2wti1@autozsys_ip60to
    Created on: 2020-04-28 11:50:06
  - Name:       rpool/ROOT/ubuntu_e2wti1@autozsys_08865s
    Created on: 2020-04-28 09:27:31
  - Name:       rpool/ROOT/ubuntu_e2wti1@autozsys_nqq08r
    Created on: 2020-04-28 09:07:42
  - Name:       rpool/ROOT/ubuntu_e2wti1@autozsys_258qec
    Created on: 2020-04-27 18:11:12
  - Name:       rpool/ROOT/ubuntu_e2wti1@autozsys_yldeob
    Created on: 2020-04-27 18:10:22
  - Name:       rpool/ROOT/ubuntu_e2wti1@autozsys_r66le8
    Created on: 2020-04-24 17:51:42
```

Our grub bootloader (available via [Escape] or [Shift] press on boot) will allow you to revert the system (and optionally user) states on demand! An history entry will propose you to boot on an older state of your system.

This goes further than OpenZFS rollbacks as we made revert a non destructive action: current and intermediate states aren’t destroyed, and you can imagine even reverting the revert (or doing system bisection!). Also, we store exactly some key dataset properties as to when the state saving was taken. The revert will then reapply original base filesystem dataset properties, to ensure better fidelity. For instance, changing a filesystem mountpoint won’t apply to the zsys snapshots inheriting from it, and we will remount it at the correct place. Similarly, we will reboot with the exact same kernel you booted with on the state that you saved, even if this wasn’t the latest available version.

Note: simply put if you are unfamiliar with ZFS technology and terminology, you can think of a dataset as a directory that you can control independently of the rest of your system: each dataset can be saved and restored separately, have their own history, properties, quotas…

![History with zsys](/images/focal/zsys_grub_back_to_the_future.png)

In a nutshell, we try to ensure high fidelity when you revert your system so that you can trustfully and safely boot on an older state of your system.


## Commit successful boot

You will tell us that pressing [Escape] or [Shift] on boot to show up grub isn’t the most discoverable feature and you are right.

However, in case of boot failing, the next boot will show up grub by default and you will have those "History entries" available to you!

Similarly, we save and commit every time you are able to successfully boot your system (we will define successful boot in the next blog post about states). This can trigger a grub menu update if needed (new states to add for instance). It means that if a boot is failing and you revert, just rebooting should keep the latest successful state as the default grub entry didn’t change!

This is just a small taste of what we do on our path of robust and bullet-proof Ubuntu desktop. We will explain all of this in greater details in the next blog post.

## User integration

Zsys is deeply integrating users to ZFS. Each non system user created manually or automatically (via `gnome-control-center`, `adduser` or `useradd`) will have its own separate space (dataset) created. We handle home directory renaming with `usermod` but still need to do more work on `userdel` as we shouldn’t delete the user immediately (what if you revert to a state that had this user for instance?).

This allows us to take automatic hourly user state save… However, we only do it if the user is connected (to a GUI or CLI session)!

You can see below those hourly automated user state save. Hourly ones are automated time-based user snapshots and you can see some more linked to system state changes.
```sh
$ zsysctl show
[…]
Users:
  - Name:    didrocks
    History: 
     - rpool/USERDATA/didrocks_wbdgr3@autozsys_setmsc (2020-04-28 15:36:25)
     - rpool/USERDATA/didrocks_wbdgr3@autozsys_8wdamc (2020-04-28 14:35:25)
     - rpool/USERDATA/didrocks_wbdgr3@autozsys_y5tsor (2020-04-28 13:34:25)
     - rpool/USERDATA/didrocks_wbdgr3@autozsys_yysp1w (2020-04-28 12:33:25)
     - rpool/USERDATA/didrocks_wbdgr3@autozsys_cgym7c (2020-04-28 12:23:12)
     - rpool/USERDATA/didrocks_wbdgr3@autozsys_kho1px (2020-04-28 12:16:34)
     - rpool/USERDATA/didrocks_wbdgr3@autozsys_w2kfiv (2020-04-28 11:52:46)
     - rpool/USERDATA/didrocks_wbdgr3@autozsys_ixfcpk (2020-04-28 11:52:24)
     - rpool/USERDATA/didrocks_wbdgr3@autozsys_ip60to (2020-04-28 11:50:06)
     - rpool/USERDATA/didrocks_wbdgr3@autozsys_891br7 (2020-04-28 11:32:25)
     - rpool/USERDATA/didrocks_wbdgr3@autozsys_yl9fuu (2020-04-28 10:31:25)
     - rpool/USERDATA/didrocks_wbdgr3@autozsys_vg70mw (2020-04-28 09:30:25)
     - rpool/USERDATA/didrocks_wbdgr3@autozsys_08865s (2020-04-28 09:27:31)
     - rpool/USERDATA/didrocks_wbdgr3@autozsys_nqq08r (2020-04-28 09:07:42)
     - rpool/USERDATA/didrocks_wbdgr3@autozsys_xgntka (2020-04-28 08:29:45)
     - rpool/USERDATA/didrocks_wbdgr3@autozsys_0zisgn (2020-04-27 19:41:36)
     - rpool/USERDATA/didrocks_wbdgr3@autozsys_44635k (2020-04-27 18:40:36)
```

User snapshots are basically free when you don’t change any files (you can think of them as if they were only storing the difference between current and previous states). It will allow us in the near future to offer easily restore of previous version of your user’s data. The "oh, I removed that file and don't have any backups" will be something of the past soon :)

Note that this isn’t a real backup as all your data are on the same physical place (your disk!), so this shouldn’t replace backups. We have plan to integrate that with some backup tools in the future.


## Garbage collection on state saving

As you can infer from the above, we will have a lot of state saves. While we allow the user to manually save and remove states, as most of them will be taken automatically, it would be complicated and counter-productive on a daily bases to handle them manually. Add to this that some states are dependent on other states to be purged (more on that in … you would have guess, the next blog post about state!), you can understand the complexity here.

The GC will have also its dedicated post, but in summary, we are trying to prune states as time passes, to ensure that you have a number of relevant states that you can revert to. The general idea is that as more time pass by, the less granularity you need. This will help saving disk space. You will have a very finer grain states to revert to for the previous day, a little bit less for the previous weeks, less for months… You get it I think. :)

## Multiple machines

Something that isn’t really well supported nowdays was multiple OpenZFS installations on the same machines. This isn’t fully complete yet (you can’t have 2 pools of the same name, like `rpool` or `bpool`), but if you install manually a secondary machine on the same pools or different ones (with different names), this is handled by Zsys and our grub menu!

![Multiple machine support with zsys](/images/focal/zsys_multiple_machines.png)

Both will have their own separate history of states and zsys will manage both! You can have shared or unshared user home data between the machines.

## Principles of Zsys

We built Zsys on multiple principles.

![Zsys architecture](/images/focal/zsys_architecture.png)

### Lightweight

The first principle of Zsys is to be as lightweight as possible: Zsys only runs on demand, it’s made of a command line (`zsysctl`) which connects to a service `zsysd`, which is socket activated. This means that after a while, if `zsysd` has nothing else to do, it will shutdown and not take any additional memory on your system.

Similarly, we don’t want zsys to slow down or interfere during the boot. This is why we hooked it up to the upstream OpenZFS systemd generator and it will only be run when doing a revert to a previous state.

### Everything is stored on ZFS pools

Secondly, we don’t want to maintain a separate database for ZFS dataset layout and structure. This approach would have been dangerous as it would have been really easy to get out of sync with what is really on disk, and being unaware of any manual user interaction on ZFS. That way, we let ZFS familiar system administrators handling manual operations while still being compatible with Zsys. Any additional properties we need are handled via user properties on ZFS datasets directly. Basically, everything is in the ZFS pool and no additional data are needed (you can migrate your disk from one disk to another).

### Permission mediation

Thirdly, permission handling is mediated via [polkit](https://www.freedesktop.org/software/polkit/docs/latest/) which is a familiar mechanism to administrator and compatible with company-wide policies. If any priviledge escalation is needed, the system will ask you for it.

![Polkit request for performing system write operation](/images/focal/zsys_polkit.png)

We will develop this topic with more details in future posts.

### Ease of use

This is the core of the command line experience: familiarity and discoverability. `zsysctl` has a lot of commands, subcommands and options. We are using advanced shell completion which will complete on **[Tab][Tab]** in both bash and zsh environments.
```sh
$ zsysctl state [Tab][Tab]
remove  save
```
We try to discard any optional arguments by limiting the number of them being required. However if you try to complete on `-` or ``--``, we will present any available matching options (some being global, other being local to your subcommand):
```sh
$ zsysctl state save -[Tab][Tab]
--auto                -s                    -u                    --user=               --verbose
--no-update-bootmenu  --system              --user                -v
```

Similarly, an help command is available for each commands and subcommands and will complete as expected:
```sh
$ zsysctl help s[Tab][Tab]
save     service  show     state
$ zsysctl help state
Machine state management

Usage:
  zsysctl state COMMAND [flags]
  zsysctl state [command]

Available Commands:
  remove      Remove the current state of the machine. By default it removes only the user state if not linked to any system state.
  save        Saves the current state of the machine. By default it saves only the user state. state_id is generated if not provided.

Flags:
  -h, --help   help for state

Global Flags:
  -v, --verbose count   issue INFO (-v) and DEBUG (-vv) output

Use "zsysctl state [command] --help" for more information about a command.
```

We made aliases for popular commands (or what we think are going to be popular :)). For instance `zsysctl show` is an alias for `zsysctl machine show`. Less typing for the win! :)

Also, all those commands and subcommands are backed by man pages. Those are for now the equivalent of --help but if you have the desire to enhance any of them, this is a simple, but very valuable contribution that we strongly welcome as [Pull Requests on our project](https://github.com/ubuntu/zsys)!

The github repo README has [a dedicated section](https://github.com/ubuntu/zsys/blob/master/README.md) with the details of all commands.

The best thing is that any of those are autogenerated at build time from source code (completion, man pages and README!). That means that the help and nice completions will never be out of sync with the released version. It also means that zsys developers can (via the `zsysctl completion` command) already experience and have a feeling of command interactions without installing tip on the system.

Finally, typos are allowed and we try to match commands that are close enough to your command:
```sh
$ zsysctl sae
Usage:
  zsysctl COMMAND [flags]
  zsysctl [command]

Available Commands:
  completion  Generates bash completion scripts
  help        Help about any command
  list        List all the machines and basic information.
  machine     Machine management
  save        Saves the current state of the machine. By default it saves only the user state. state_id is generated if not provided.
  service     Service management
  show        Shows the status of the machine.
  state       Machine state management
  version     Returns version of client and server

Flags:
  -h, --help            help for zsysctl
  -v, --verbose count   issue INFO (-v) and DEBUG (-vv) output

Use "zsysctl [command] --help" for more information about a command.

ERROR zsysctl requires a valid subcommand. Did you mean this?
	state
	save
```

For the ones attached to details, you should have seen that some commands and subcommands on our README isn’t available through completion or that we have some man pages on commands that aren’t shown there. There are indeed system-oriented hidden commands and this is why they are not proposed by default (like the `boot` command: `zsysctl bo[Tab][Tab]` won’t display anything). However, for testing, we are found of completion and if you type it entirely, you are back on completion enjoyable land:
```sh
$ zsysctl boot [Tab][Tab]
commit       prepare      update-menu
```

If you want to implement something similar in your (golang) CLI program, we [proposed our changes](https://github.com/spf13/cobra/pull/983) to the cobra upstream repository so that you can get advanced shell completion as well!

### Versatility

We didn’t want to force upon user a strict zsys layout. First, we will have different default ZFS dataset layouts between server and desktop (this will be detailed in a future blog post). We are also very aware that there are excited and passionate ZFS system administrator or hobbyist that want to have full control over their system. This is why:
* Any system can be untagged to prevent zsys controlling it. The system will then boot and behave as any ZFS on root manual installation. However, of course, any additional features we are providing through Zsys will be unavailable.
* Dataset layout can be a mix between what we provide by default and how system administrators have their habits or desired targets. For instance, bpool isn’t mandatory, any child dataset can be deleted, some persistent datasets can be created… We will explain more about dataset layouts and the types of datasets Zsys is handling in more advanced parts of this blog post series.

### Strongly tested

We put a strong emphasize on testing. Zsys itself is currently covered by more than 680 tests (from units to integration tests). It can exercise real ZFS system but we have also built an in memory mock (which can parallelize tests for instance). We can thus validate it against the real system with the same set of tests!

We also have more than 400 integration tests for grub menu building, covering a wide variety of dataset layouts configuration.

### Detailed bug reporting

I hope you can appreciate that we put a lot of thoughts and care on Zsys and how it integrates with the ZFS system.

Of course, bugs can and will occur. This is why [our default bug report template](https://github.com/ubuntu/zsys/issues/new?template=bug_report.md) is asking to run `ubuntu-bug zsys` as we collect a bunch of non private information on your system, which will help us to understand what configuration your are running with and what actually happened in various part of the system (OpenZFS, grub or Zsys itself)!

Again, most of the features are completely working under the hood and transparently to the user! I hope this gives you a sneak preview of what Zsys is capable of doing. I teased a lot about further development and explanation I couldn’t include there (this is already long enough) on a lot of those concepts. This is why we will dive right away into state management (which will include revert, bisections and more)! See you there :)

Meanwhile, join the discussion via the [dedicated Ubuntu discourse thread](https://discourse.ubuntu.com/t/).