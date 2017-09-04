+++
title = "Design experience and demos in GTK and Clutter"
date = "2009-08-25T08:10:00+02:00"
tags = [ "gnome" ]
aliases = ["/post/Design-experience-and-demos-in-GTK-and-Clutter"]
+++
    <p>Well, coming back from vacation, I had a couple of hours to "kill" in the train.</p>


<p>I saw a few days ago the awesome work of Davyd Madeley <a href="http://davyd.livejournal.com/280866.html" hreflang="en">on animating GTK+ and Clutter-Gtk from client-side-windows</a> based on the first step from <a href="http://blogs.gnome.org/alexl/2009/06/12/the-return-of-client-side-windows/" hreflang="en">Alexander's Larson</a>. Reading the comments some people say "this is sooooo cool, but totally unuseful".</p>


<p>That's the reason why I tried to figure out some pratical examples where some animations can teach about the component/widget the user is currently interacting with. Giving some blink effects so that people can understand a concept has already been successfull: take the example of the cube in compiz which has helped a bunch of GNU/Linux newcomers to understand what really a worspace is.</p>


<p>In my experience, the tab concept is quite difficult for some people like my mother, not having the habit of it (it has never come to her minds that it's like a traditional phone notebook). So, maybe some animations can help them to realize what it is.</p>


<p>I built Davyd's version of clutter-gtk and other stuffs and then, began to write some realistic animations on the GTK Notebook component.
BEWARE: as the frame can't be currently reparented and as I need to have one actor by frame, the current implementation is very hackish and just there for some proof of concept (code <a href="/public/projects/clutter-tests/testnotebook.c">here</a> for all animations below).</p>


<p>The first one is just a fade in/fade out, very unobtrusive.</p>

<div style="margin:1em auto;text-align:center">

  
  

<br>clutter gtk fading
</div>



<p><a href="/public/projects/clutter-tests/fade.ogv">Ogg version here</a>.</p>


<p>Second is a little more noticeable, with the new frame coming and overlapping the previous one.</p>


<div style="margin:1em auto;text-align:center">

  
  

<br>clutter gtk fade and show
</div>



<p><a href="/public/projects/clutter-tests/fade_one.ogv">Ogg version here</a></p>


<p>Third is maybe the best in term of representation: we change the current "sheet" (the frame) and put the new one in front:</p>

<div style="margin:1em auto;text-align:center">

  
  

<br>clutter gtk fade and move
</div>



<p><a href="/public/projects/clutter-tests/fade_move.ogv">Ogg version here</a></p>


<p>The two last ones are more for fun. Not that they don't represent (even maybe better than the previous one) the real behavior of such a component, but they take a lot of space on the screen:</p>


<p>Rotating from the left:</p>
<div style="margin:1em auto;text-align:center">

  
  

<br>clutter gtk rotating left
</div>



<p><a href="/public/projects/clutter-tests/rotate_left.ogv">Ogg version here</a></p>


<p>An finally, rotating from the bottom:</p>

<div style="margin:1em auto;text-align:center">

  
  

<br>clutter gtk rotating bottom
</div>



<p><a href="/public/projects/clutter-tests/rotate_under.ogv">Ogg version here</a></p>



<p>So, what do you think about those, does that bring a better user feedback? Maybe we can explore that way to make a properly binding between clutter and GTK for making such things. With Clutter, GTK components can be more interactive and attractive.</p>


<p>Of course, we still need a default fallback for not forcing clutter, though.</p>
