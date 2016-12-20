+++
title = "Quickly 0.4.1 is out!"
date = "2010-04-23T15:00:00+02:00"
tags = [ "devel", "PU", "quickly", "ubuntu" ]
aliases = ["/post/Quickly-0.4.1-is-out%21"]
+++
    <p>0.4.1 is a bug fixing release and will be the one in lucid final.
<img src="/public/projects/quickly/.Quicky-0.4.1_m.jpg" alt="Quickly 0.4.1" style="display:block;margin:0 auto" title="Quickly 0.4.1, avr. 2010"></p>


<p>It contains of course all the goodness of <a href="/post/Quickly-0.4-available-in-lucid!" hreflang="en">0.4 version</a> plus some bug fixes that early users encountered:</p>
<ul>
<li>remove ~/.selected_editor detection. Introduced confusion for users who doesn't understand why (nano, most of the time), was triggered instead of gedit (LP: #565586). It's still possible to override the choosen editor with EDITOR or SELECTED_EDITOR environment variables.</li>
<li>fix gpg key creation with no email address. Fortunately, new Quickly user won't have to bother that much with gpg key anymore (see 0.4 blog post to see what's still needed to be done)</li>
<li>fix again some tutorial issues which had been lost during html -&gt; docbook transition</li>
<li>better message and user help when ppa not found</li>
<li>add more debugging info in --verbose mode for gpg keys</li>
<li>updated translations</li>
</ul>

<p>0.4.1 should be building soon on the ubuntu builders, keep it hot! ;)
Release tarballs are available as usual <a href="http://launchpad.net/quickly" hreflang="en">on Launchpad</a></p>


<p>Kudos to Lutin for the photo :)</p>