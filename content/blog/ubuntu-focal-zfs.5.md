---
title: "ZFS focus on Ubuntu 20.04 LTS: ZSys state collection"
date: 2020-05-01T09:36:19+02:00
tags: [ "pu", "ubuntu", "zfs" ]
banner: "/images/focal/garbage_collection.jpg"
type: "post"
manualdiscourse: "https://discourse.ubuntu.com/t/XXXXX"
draft: True
---

# ZFS focus on Ubuntu 20.04 LTS: ZSys state collection

In the [past couple of articles](https://didrocks.fr/TODO), we explained the core state concept of [ZSys](https://github.com/ubuntu/zsys), and when we create them. A lot of those operations are automated either on a time-scheduled (user states save), on system changes (installation, upgrade or removal for system states) and also when you ask to revert to a previous states.

Even if individually, the cost of a state save is really low, this creates more and more ZFS datasets over time that will take some spaces. We needed to come with a strategy to clean them up on the go, silently, for our users.

## State garbage collection

### Main principles

The strategy of our state garbage collection policy applies to both system and user states. Our goal is to keep meaningful states to revert to, and space them as much as possible. Of course, current system and user states (for any users linked to current system states) will never be purged.

Our default policy is built against the idea that recent state saves are more important than the one in distant past. Also, you want finer grain state saving to optionally revert to, on something that happened recently than something that happened a couple of months ago. We thus keep more states on the recent past and decay more and more states as time pass by. As a bonus, in general, the more recent a state is, the smaller delta is has with the current state and thus, the less space it’s taking on your disks.

We first apply it to system states, as they all have user states linked with. We don’t immediately destroy user states when we decide to purge a system state, but we rather unlink them. The user state save is meaningful by itself, and we want to ensure having the best possible outcome when considering user state in term of spacing up state saves when going over them once dealt with system states.

Basically, if you have 3 states to keep for a given period, and your starting state repartition is looking like that:
```
-----------|-----------|----------------|----|------------|----------------|-------> (time)
```

Each **|** here represents a state save. Ideally, we will try to space the states equally to have:
```
-----------|-----------------------------------|------------------------------|-------> (time)
```

instead of the less useful:
```
------------------------------------------------|------------|----------------|-------> (time)
```

Note that that the states can have dependencies between them (especially when you are reverting, and as such, have dataset clones). You can think of it as multiple branches of a tree and you can’t remove a branch unless all leaves are removed. Similarly, some manual states can’t be removed, and thus, we are trying to equilibrate the whole state repartition so that you have more meaningful one.

Imagine on the previous scheme that one state can’t be removed (manual or having dependencies that can’t be purged).
```
-----------|-----------|---|----------------|------------K----------------|-------> (time)
```

K is the state to keep. We will thus keep to have an evenly spaced distribution:
```
-----------|---------------|-----------------------------K------------------------> (time)
```

So, the reality can be not as easy and evenly spaced as previously illustrated, but the garbage collector is trying to do its best for you. A scheme is easily drawn, but in practice, this is more complex and involves multiple pass collections (as some states having some dependencies can be freed once their reverse dependencies are collected).

### When is it scheduled?

We are analyzing and collecting by default once a day. This is done by a global systemd zsys-gc timer:
```sh
$ systemctl list-timers zsys-gc
NEXT                         LEFT     LAST                         PASSED UNIT          ACTIVATES      
Tue 2020-05-12 08:47:15 CEST 16h left Mon 2020-05-11 08:47:15 CEST 7h ago zsys-gc.timer zsys-gc.service

1 timers listed.
Pass --all to see loaded but inactive timers, too.
```

This one is associated to `zsys-gc.service` which calls under the hood `zsysctl service gc` once a day. This both collects the system states, and each user states for the current machine.

### What do we collect?

By default, we only collect automated generated states, as we think those are less meaningful to you than the one you proceeded manually by a `zsysctl save <MyOwnState>` command. In practice, only unassociated filesystem states or snapshot states starting by `@autozsys_` prefix will be collected. In addition, any user dataset clones, untagged (not attached to any system states by a `com.ubuntu.zsys:bootfs-datasets`, we will come back to the previse mechanism in a future article) will be cleaned up as well.

Note that you should clean your manual state save yourself, with `zsysctl state remove <MyOwnState>` for instance, for an explicit removal. Something else you can do is to request taking into account all states for a given period, automated and manual, with the `--all` flag: `zsysctl service gc --all`. This flag is global for the system and all users.

Valid state candidates for garbage collection are any state that don’t have a dependencies over them, we should thus just quickly recheck what dependencies are.

### What are a dependencies of one state?

We have already seen that when going over [states removal in the previous article](https://didrocks.fr/TODO) which will exactly show them to you, but as a reminder, states can have dependencies on other states when:

* your state is associated with filesystem datasets which has dataset snapshots over it (typically: a revert to a previous which had snapshots after the revert point)
* your state is associated with dataset snapshots and you have a state made of filesystem dataset cloned over it (typically: after a revert)
* your state is associated with dataset snapshots and you have made a manual clone on it. Until you remove manually the clone, the dependency will be kept
* an user state will have a dependency over its system state if one is associated with it.

Note that as we just have told, we are going over multiple passes and generations. Thus, some states that had dependencies can be then freed up and will be then a valid candidate for garbage collection. Of course, manual states will never be considered and can thus lock down an automated state.

In conclusion, if you really want a particular state (manual or automated) and its dependencies to be purged, including its manual clone, you should call directly `zsysctl state remove` command. This will give you a chance to analyze and see how you can force a removal of one state, even if it has dependencies.

## Default policy

The default policy is embedded in the binary itself. Here is the definition up for us to study:
```yaml
history:
  # Keep at least n history entry per unit of time if enough of them are present
  # The order condition the bucket start and end dates (from most recent to oldest)
  gcstartafter: 1
  keeplast: 20 # Minimum number of recent states to keep.
  #    - name:             Abitrary name of the bucket
  #      buckets:          Number of buckets over the interval
  #      bucketlength:     Length of each bucket in days
  #      samplesperbucket: Number of datasets to keep in each bucket
  gcrules:
    - name: PreviousDay
      buckets: 1
      bucketlength: 1
      samplesperbucket: 3
    - name: PreviousWeek
      buckets: 5
      bucketlength: 1
      samplesperbucket: 1
    - name: PreviousMonth
      buckets: 4
      bucketlength: 7
      samplesperbucket: 1
    - name: PreviousYear
      buckets: 11
      bucketlength: 30
      samplesperbucket: 1
    - name: Previous2Years
      buckets: 1
      bucketlength: 365
      samplesperbucket: 2
```

Any time period is in days and the whole policy starts at midnight for the current day. Remember that those rules apply to both system and for each user, independently.

What does this all mean?

* We keep all state saves for the current day (from midnight to now)
* We also keep all previous state saves for the previous day. `gcstartafter: 1` (GC start after a whole day).
* We keep the last 20 saved states (in total, even if we should have removed more of them in previous days). `keeplast: 20`. The idea of this is avoiding for instance one user (or the whole system) to not connect for days and having suddenly all the history being collected with very few states to revert to if things don’t go well.

Then the `gcrules` can have as many buckets as you desire. Our policy is:

* For the previous Day (after on fulle day of retention of all snapshots due to `gcstartafter: 1`), the rule `PreviousDay` defines one bucket (`buckets: 1`) of size 1 day (`bucketlength: 1`), where we keep 3 states. So basically, we keep 3 states for the previous full day.
* For the 5 days before (`buckets: 5` of size 1 day (`bucketlength: 1`)), we keep one state (`samplesperbucket: 1`). It means thus that we keep one state per day on those 5 days.
* We divide the previous month, in 4 buckets (`buckets: 4`) of 7 days each (`bucketlength: 7`) and keep one state for each (`samplesperbucket: 1`). In english, this means that we try to keep one state save per week over the previous month.
* Before that, on the previous year or so (`buckets: 11` for each `bucketlength: 30`), we keep one state (`samplesperbucket: 1`). In full world, over the previous year, we keep one state per month.
* And on the year before (`buckets: 1` with `bucketlength: 365`), we keep two states over that duration (`samplesperbucket: 2`). We try thus to keep a state every 6 months approximately over that period of time.

Note that state dependencies can be cross-buckets, and each buckets will always be considered independently from each other.

### What happens in case of under limit and over limit ?

Basically, those are goals. If you have taken more state saves (with manual `zsysctl save <statename>` call), as those counts in the whole budget and can’t be purged automatically, we will thus have more states in our final results, and all automated states may be purged.

Similarly, if we have a lot of states with dependencies that can’t be cleaned up, we will end up with many states (over limit).

If you don’t start your system for a given period of time and don’t have enough state saves to file up a bucket, you will not reach the limit of course and all states will be kept.

Remember that we always treat system states before treating user states, and then go over each user states independently after detaching those which were linked to purged system states.

### Can we tweak that manually?

We described the default policy embedded in the binary. However, as an experiment (and because all those rules are not set in stone yet), you can define your own policy manually by copying the configuration policy file [zsys.conf](https://github.com/ubuntu/zsys/blob/master/internal/config/zsys.conf) as `/etc/zsys.conf` and tweaking it here. This change will be effective on daemon restart.

### Future work

In addition to gradual improvements to our policy, we will deliver some ways to says "destroy all state saved before <date>" and other facilities to mass-remove states as needed. Also, identifying which states are eating a lot of disk space will be of help for the users. Pressing more or less aggressively our garbage collection policy based on how much disk space is left for you is another track we want to explore in the future.

## Final note on states

We hope this syntax is readable, it was quite a journey to find some way to define precisely our garbage collection policy and organize this. The principle is really built on keeping as many recent states as possible (with our default rules for current and previous day + the `keeplast` parameter for the system and each users, independently), while purging more and more states as time pass by (through the bucket definitions). We are trying to equally space the states we are keeping based on dependencies constraints.

As you can think, all of this is backed up with a very extensive test suite as this is playing with system and user data. However, we may be tweaking this over time based on your feedback and pressure on disk over the `rpool` and `bpool`. You are always on the driver’s seat either by forcing taking into account all states in the gc (`--all` flag), by manually removing states and dependencies `zsysctl state remove <your_state>` or by tweaking the GC policy manually with `zsys.conf`.

But that’s not the end story of our journey through the ZFS integration in Ubuntu 20.04 LTS! In the next blog post, we will list some remaining ZSys user-facing features we didn’t mention yet. We will try to highlight some more tweaking you can do on your system and how all those pieces fits together. See you there :)

Meanwhile, join the discussion via the [dedicated Ubuntu discourse thread](https://discourse.ubuntu.com/t/).