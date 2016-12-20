+++
title = "Getting sound working during a hangout in raring"
date = "2012-12-31T10:21:00+01:00"
tags = [ "PU", "ubuntu" ]
aliases = ["/post/Getting-sound-working-during-a-hangout-in-raring"]
+++
    <p>Since approximately the beginning of the raring development cycle, I had an issue on my thinkpad x220 with google hangouts forcing me to use a tablet or a phone to handle them. What happened is that once I entered a hangout, after 40-60s, my sound was muted and there was no way to get the microphone back on, same for ouptut from other participants. Video was still working pretty well.</p>


<p>Worse, even after exiting the hangout, I could see the webcam was still on and the plugin couldn't reconnect or recreate a hangout, I had to kill the background google talk process to get it working back (for less than a minute of course ;)).</p>


<p>Finally, I took a look this morning and googling about that issue. I saw <a href="http://askubuntu.com/questions/168895/getting-google-talk-skype-to-work-with-pulseaudio">multiple reports</a> of people tweaking the configuration file to disable volume auto adjustement. So, I gave it a try:</p>
<ol>
<li>Edit <em>~/.config/google-googletalkplugin/options</em></li>
<li>Replace the audio-flags value set to 3 to use 1 (which seems disabling this volume auto adjustement feature)</li>
</ol>

<p>Then, kill any remaining google talk process if you have some and try a hangout. I tested for 5 minutes and I was still able to get the sound working! After quitting the hangout, the video camera is turned off as expected. I can then restart another hangout, so the google talk process don't seem to be stalled anymore.</p>


<p>Now, it's hard to know what regressed as everything was working perfectly well on ubuntu 12.10 with that hardware. I tried on chromium, chrome canary and firefox with the same result. The sound driver don't seem to be guilty as I tried to boot on an 12.10 kernel.  The next obvious candidate is then pulseaudio, but it's at the same version than in 12.10 as well. So it seems to be in the google talk plugin, and I opened a <a href="http://code.google.com/p/googlebugs/issues/detail?id=439&amp;q=didrocks">ticket on the google tracker</a>.</p>


<p>At least, with that workaround, you will be able to start next year with lasting sound during your hangouts if you got this problem on your machine. ;)</p>