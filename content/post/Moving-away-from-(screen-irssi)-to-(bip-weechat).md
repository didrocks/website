+++
title = "Moving away from (screen + irssi) to (bip + weechat)"
date = "2010-01-18T20:40:00+01:00"
aliases = ["/post/Moving-away-from-%28screen-irssi%29-to-%28bip-weechat%29"]
+++
    <p>I have been a highly screen + irssi user for some years running on my server and then, just sshing to it.
Apart from the high latency on typing you can get when your connection begins to lag, everything fulfil my need.</p>


<p>However, last week, <a href="http://www.piware.de" hreflang="en">pitti</a> told me that he was using a wonderful feature which is called "smart filtering".
If you are a long time IRC user and particularly if you go to crowded channels, you know how seeing all this join/part messages can be annoying. Of course, most of IRC clients can hide all join/part/quit/away messages on selected channels.</p>


<p>But <a href="http://dev.weechat.org/post/2008/10/25/Smart-IRC-join-part-quit-message-filter" hreflang="en">smart filtering</a> brings the concept a little bit further: you can tell "show me /quit /join only if the user was speaking on the channel in the last xxx minutes". Great to not speak into the wild when you're not always looking at your channel user list every minute! Unfortunately, it seems there is no such extension for irssi. As writing a plugin in perl is not my taste, I moved to <a href="http://www.weechat.org/" hreflang="en">weechat</a> where this feature has been implemented. It was the right time to try as well <a href="http://bip.t1r.net/" hreflang="en">bip</a> (an IRC proxy).</p>


<p>bip gaves me some hard time because intrepid<sup>[<a href="#pnote-157-1">1</a>]</sup> version is buggy: it preprends backlogs with - (and xchat doesn't highlight the channel if a message begins with "-") and adds "+" to other messages. Well, updating it just fix it, but it took some time to figure out (xchat was hiding those - et +).<p>


<p>Once done, I tried weechat, which seems to be a very great IRC client in a modular architecture. No more latency in typing things as the client is now local and connecting to my bip proxy. The only annoying part is that I'm connected to 4 different IRC networks (and consequently, 4 bip networks) and buffers<sup>[<a href="#pnote-157-2">2</a>]</sup> aren't properly ordered after restarting it. I have to run /layout apply 3 times to get the correct ordering at each start :( This is a <a href="https://savannah.nongnu.org/bugs/?26110" hreflang="en">known bug</a>.<p>


<p>Then, I installed the notification script. The annoying bit for me was that I got spammed with too many notifications:</p>
<ul>
<li>you are notified everytime someone highlight you in a channel (seems logical for a notification script)</li>
<li>you are notified at each private message someone sent you!</li>
</ul>

<p>And there is where the nightmare begin: if you begin with a long private conversation to someone, you'll get spammed for endless time (especially when notification are shown for 10 secondes at each message!). To fix that, I've just hack quickly on notify.py script (yes, it's in python) to add a smart_notification option (off by default).</p>


<p>If you run: <strong>/set plugins.var.python.notify.smart_notification on</strong> then smart notification is activated. That means that you won't get notified if someone is either highlighting you in the current channel you have a buffer opened to, or in your current private conversation. That means, logically, you won't get notification if you're looking at the channel you are currently looking at. You'll only see the red/yellow/whatever color line weechat highlight your text. Of course, if your are highlighted not in your current buffer (someone is beginning a private conversation you didn't switch to), you'll still be notified. The idea is only to warn you if you have something that needs to get your attention you're not currently looking at right now, sweet isn't? :)</p>


<p>The new version (0.0.4) is sent upstream, if it won't get it before lucid, I'll upload it to Ubuntu to get it there.</p>


<p>You can find this new version there: <a href="/public/projects/divers/notify.py">notify.py</a></p>
<div><h4>Notes</h4>
<p>[<a href="#rev-pnote-157-1">1</a>] yes, my server runs intrepid, I'll reinstall a clean architecture when lucid will be released<p>
<p>[<a href="#rev-pnote-157-2">2</a>] name of channel or private window screen</p><div>