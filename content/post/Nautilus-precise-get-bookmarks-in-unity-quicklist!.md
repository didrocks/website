+++
title = "Nautilus precise unity quicklist gets bookmarks!"
date = "2012-01-17T09:25:00+01:00"
tags = [ "devel", "libre", "malife", "PU", "ubuntu" ]
aliases = ["/post/Nautilus-precise-get-bookmarks-in-unity-quicklist%21"]
+++
    <p><a href="/public/ubuntu/nautilus_quicklist.png" title="Nautilus quicklilst"><img src="/public/ubuntu/.nautilus_quicklist_s.jpg" alt="Nautilus quicklilst" style="float:left; margin: 0 1em 1em 0;" title="Nautilus quicklilst, janv. 2012" /></a>Flying back from Budapest, I hacked on a long-time awaited design whishlist: "<a href="https://bugs.launchpad.net/ayatana-design/+bug/723862">Nautilus quicklist does not contain the locations previously found under the 'Places' menu.</a>". This is about getting the Unity quicklist for the nautilus launcher icon to display the list of the user's bookmark. It was unexpecdictly more "fun" that what I thought it would be.</p>


<p>Indeed, there is already one <a href="https://wiki.ubuntu.com/Unity/LauncherAPI#Dynamic_Quicklist_entries">dynamic quicklist</a>, appearing when you are making a copy operation which can take some time (when the copy dialog appears), giving the possibility to "display the copy dialog" and "stop all current copy operation". In addition to that, nautilus removes quite regularly all bookmarks and read them.</p>


<p>Surprinsingly, there is no API in libunity/dbusmenu to create "chunks" of menu and merge them at the end, which would be useful in cases like this one when an application have multiple quicklist update source. That means that we need to be nitpick so that one quicklist source doesn't erase the other. I've created some UnityQuicklistHandler GObject class for that impemented into Nautilus. Also, Nautilus can have multiple desktop files in the launcher, all of them are updated in the quicklist (and I tried to minimize memory consumption there).</p>


<p>As a small bonus, there is as well now the <a href="https://wiki.ubuntu.com/Unity/LauncherAPI#Static_Quicklist_entries">static "open a new window" quicklist</a> entry to easily open a new nautilus window. I just <a href="https://launchpad.net/ubuntu/+source/nautilus/1:3.2.1-2ubuntu9">uploaded it in precise</a> right now. Enjoy! :)</p>