---
title: "Ubuntu GNOME Shell in Artful: Day 12"
date: 2017-09-14T11:25:25+01:00
tags: [ "pu", "ubuntu", "gnome" ]
banner: "images/artful-shell-transition/alt-tab-application-multiple-windows.png"
type: "post"
---

We'll focus today on our advanced user base. We, of course, try to keep our default user experience as comprehensible as possible for the wider public, but we want as well to think about our more technical users by fine tuning the experience… and all of this, obviously, while changing our default session to use GNOME Shell. For more background on our current transition to GNOME Shell in artful, you can refer back to our decisions regarding our default session experience as [discussed in my blog post](/2017/08/03/ubuntu--guadec-2017-and-plans-for-gnome-shell-migration/).


# Day 12: Alt-tab behavior for advanced users.

Some early feedbacks that we got (probably from people being used to Unity or other desktop environments) is that application switching via Alt-Tab isn't something completely natural to them.
However, we still think, and this was a common shared experience between both GNOME Shell and Unity that model fits better in general. People who disagrees can still install one of the [many GNOME Shell extensions](https://extensions.gnome.org/) for this.

When digging a little bit more, we see that the typical class of users complaining about that model is the power users, who has multiple windows of the same applications (typically terminals), and want to switch quickly between the last 2 windows of either:

* the current application
* the focused window of the last 2 applications

The first case is covered by **[Alt]** + **[Key above tab]** (and that won't change even for ex-Unity users)[^1]. However the second case isn't.

That could lead to some frustrating experience if you have a window (typically a browser) standing in the background to read documentation, and having a terminal on top. If you want to quickly switch back and forth to your terminal (having multiple windows), you end up with:

{{< youtube D32VEP0sFk4 >}}

Note that I started from one terminal window and a partially covered browser window, to end up, after 2 quick alt-tabs to two terminal windows covering the browser application.

We want to experiment back with quick alt-tab. Quick alt-tab is alt-tabbing before the switcher appears (the switcher doesn't appear right away to avoid flickering). We can imagine in that case, we can switch between the last focused window of the last 2 applications. In the previous example, it would put us back to the initial state. However, if we wait enough for the switcher to be displayed, then, you are in the "application mode", where you switch between all applications, and the default (if you don't go into the thumbnail preview), is then to raise all windows of that applications. That forced us to increase slightly the delay before the switcher appears.

That was the default of our previous user experience and we didn't experience bug report about that behavior, meaning that it seems that it fits both power user, and a more traditional type of users (having mostly one window per application, and so not being impacted, or not using quick alt-tab, but the application switcher itself, which doesn't change behavior). We proposed a patch and [opened a discussion upstream](https://bugzilla.gnome.org/show_bug.cgi?id=787627) to see how we can converge to this idea, which might evolve and being refined in a later release to only be restricted to terminals from where the discussion seems to lead on. For now, as usual, this change is limited to the ubuntu session and doesn't impact the GNOME vanilla one.

Here is the result:

{{< youtube k-IBk-Fibj4 >}}

Another slight inconsistency was the **[Alt]** + **[key above tab]** in the switcher itself. Some GNOME Shell releases ago, going into the window preview mode in the switcher was enabling selecting a particular window instance, but was still raising all other windows of the selected application. The selected window was just the most top one. Later, this behavior has been changed to only raise the selected window.

While using **[Alt]** + **[Tab]** followed by **[Alt]** + **[key above tab]** selecting directly the second window of the current app made sense in the first approach (as, after all, selecting the first window was the same effect than selecting the whole app), now that only the selected window is raised, it makes sense to select the first window of the current application on initial key press. Furthermore, that's already the behavior of pressing the **[down]** array key in the alt-tab switcher. For Ubuntu Artful, this behavior is as well only available in the ubuntu session, however, it seems that [upstream is considering the patch](https://bugzilla.gnome.org/show_bug.cgi?id=786009) and you might see it in the next GNOME release.

As usual, if you are eager to experiment with these changes before they migrate to the artful release pocket, you can head over to [our official Ubuntu desktop team transitions ppa](https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/transitions) to get a taste of what's cooking!

We hope to find a way to satisfy both advanced and casual users, tuning the right balance between the 2 use cases. Focusing on a wider audience doesn't necessarily mean we can't make the logical flow compatible with other type of users.

[^1]: Note that I didn't write **[Alt]** + **[~]** because any sane keyboard layout would have a **²** key above tab. :)
