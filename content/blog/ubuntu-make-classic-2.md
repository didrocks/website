---
title: "Ubuntu Make as a classic snap: getting a 16.04 snap"
date: 2017-07-12T11:27:24+02:00
tags: [ "snaps", "ubuntu-make", "uld", "ubuntulovesdevs", "pu" ]
banner: "images/snap-16.04.png"
type: "post"
slug: "ubuntu-make-as-classic-snap-getting-16.04"
draft: true
---

This is a suite of blog posts explaining how we snapped Ubuntu Make which is a complex software study case with deep interactions with the system. For more background on this, please refer to our previous blog post giving [a quick introduction](/2017/07/05/ubuntu-make-as-a-classic-snap-intro) on the topic.

# Creating the snap skeleton

The snap skeleton was pretty easy to create. [Galileo](https://github.com/LyzardKing) from our community  got a [first stance at it](https://github.com/ubuntu/ubuntu-make/commit/523e8115a37a141f7d11a5e09909d726ce357c46). We can notice multiple things:

 * Description and package name are directly coming from our debian package.
 * It executes our developer git wrapper binary (which is different from the one people executes from the deb)
 * He declared all build dependencies and binary package dependencies as respectively build-packages and stages-packages in the snap.
 * The confinement has been set to classic. We’ll cover that a little bit more in a minute.
 
# First test and small cleanup
The snap was working as expected on a machine which had already Ubuntu Make installed in its debian package flavor. One of the first obvious cleanup was to remove both “debhelper” and “dh-python” build dependencies, as those are helpers specific to build a debian packages.

However, having a working classic snap on a system is a double edge-sword. It speeds up drastically the first transition to create a snap, but despite the snap working perfectly well here, it wouldn’t work at all on other machines.

# Classic snap: decisions and traps

First, let me state that classic snaps are great. Without that feature it wouldn’t be possible to actually deliver Ubuntu Make as a snap.
 
Indeed, as we discussed in the previous blog post, Ubuntu Make interacts heavily with the system, in particular the apt database (and some other system directories). Confined or devmode snaps aren’t abled to do that for very good reason. Transitioning to a classic snap gives us the same power than a debian package on this regard, while still enabling for a smoother transition.
 
If you aren’t clear about the differences between confined, devmode or classic snap, I would suggest you to take some time to look at the excellent [advanced snap usage tutorial](https://tutorials.ubuntu.com/tutorial/advanced-snap-usage) which has an entire section on interfaces and permissions explaining in details each kind of confinement.
 
So, classic snap it is then for our confinement strategy! We have access to the whole system as any other debian package, enabling us to deal with that apt database, great! However, that also means that our python program has access to all python packages installed on the system… So, our first test may have been biased and context-dependent.
 
And indeed it was, testing on a blank VM confirmed this: dependencies shipped in the stage-packages list in the snap aren’t used, only the system ones are, from the python binary, to every installed dependencies. This is the default tradeoff with classic snap: it’s really like a debian package and you are, by default, dependent on what is installed on the system. This is why always testing on a vanilla distribution installation is a good idea.

# Fixing by using snap internal dependencies
So… How can we use those stage-packages?


When working on a confined (strict or devmode) snap, snapcraft is creating a wrapper for us to use (via environment variables, most of the time) dependencies and binaries shipped by the snap. As by design classic snaps don’t do that (they promise to leave all your environment variables untouched), we thus need to do it ourself.
 
The resulting binary is the following one: https://github.com/ubuntu/ubuntu-make/commit/930df61a9699dc2ca04766f6e77100da1af03f28, where we add the python3 binary to the snap PATH, export some python-related variables (*PYTHONUSERBASE*, *PYTHONHOME*) to point to our internal python versions and tweak *LD_LIBRARY_PATH* based on our architecture, so that our code can find needed C and C++ libraries.

This is not as much different from what snapcraft generates for strict or devmode snaps.
 
We then just needed to amend our snapcraft.yaml to ship that new binary wrapper as you can [see here](https://github.com/ubuntu/ubuntu-make/commit/5bce8b458899110ec2478576255a9b7faea41bea).
 
# One last thing…
So, we now have our binary working in simple case and life looked good (`umake --help` works, ship it! ;)). Basic frameworks installation worked like a charm.

However, I started to get “interesting” issues only in some very specific cases. Ubuntu Make was starting and then giving me some infamous “ImportError: module encoding not found”.
 
This was more visible on 14.04 LTS (but we’ll get to 14.04 properly in next blog post), and can be reproduced on 16.04 LTS if I removed python3 from the system. So clearly, something was picking up the python3 system binary at some point. What could it be?
 
Ubuntu Make re-executes itself as root when required (and then, drop its privileges quickly) to be able to install debian packages from apt. As this is only for some frameworks in case those packages weren’t installed, this is why it was only for certain code path. sudo drops environment variables while re-executing and so, /usr/bin/env python3 was matching again the system ones (and was looking for the system python packages).

A [small code modification](https://github.com/ubuntu/ubuntu-make/commit/8af76308dafa87fecb50d29d9dd51b5cd269fd0f) made us reexporting in that case *PATH*, *LD_LIBRARY_PATH* and *PYTHON\** env variables.
 
With all this, Ubuntu Make worked like a charm as a snap on a blank 16.04 LTS machine. But as it’s a classic snap and we saw that those snaps need extra-caution, I wanted to test across other versions. Also, I wanted to continuously deliver our latest and greatest without manual intervention. We’ll see those two in the next chapter of this blog post serie.
