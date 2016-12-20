+++
title = "Build your application quickly with Quickly: Inside Quickly part 7"
date = "2009-09-21T11:55:00+02:00"
tags = [ "devel", "quickly", "ubuntu" ]
aliases = ["/post/Build-your-application-quickly-with-Quickly%3A-Inside-Quickly-part-7"]
+++
    <p>We previously saw the general concepts around Quickly and more particular its core.</p>


<p>So, now, it's time to dive into the different commands of the first Quickly template which is <em>ubuntu-project</em>.</p>


<h2>What brings me ubuntu-project template?</h2>


<p>To make programming easy and fun, we've made some opinionated<sup>[<a href="#pnote-127-1">1</a>]</sup> choices about what tools, apis, etc.. to use.<p>


<p>In a nutshell:</p>
<ul>
<li>Python for the language</li>
<li>pygtk for the UI framework</li>
<li>Glade for the UI editor</li>
<li>Gedit for the code editor (though this is easy for you to change if you choose another one)</li>
<li>bzr for version control</li>
<li>Launchpad for code hosting</li>
<li>desktopcouch for storage/database (!)</li>
</ul>

<p>ubuntu-project template adds some glue there to all those technologies to lower the barrel and facilitate the learning curve.</p>


<h3>create command</h3>

<p>The first command you will certainly use is to create an ubuntu-project "templated" project. You can just execute the following line to get a working project directory:</p>
<pre>
quickly create ubuntu-project Myproject
</pre>


<p>This causes a bunch of info to be dumped to the command line, but ends with the application being run. What Quickly does is to copy over basically a sample application, and do some text switcheroos to customize the application with the name provided.</p>


<p>Here is the obligatory screenshot of a newly created Quickly application without any change:</p>


<p><a href="/public/projects/quickly/Capture-Myproject.png"><img src="/public/projects/quickly/.Capture-Myproject_m.jpg" alt="Quickly default image" style="display:block;margin:0 auto" title="Quickly default image, sept. 2009"></a></p>


<p>Note that in addition to the main dialog, you get a preferences and also an about dialog.</p>


<p>Under the hood, the command initiate a bzr repository and make a first commit to your project, this will enable you to track your changes and revert them later if needed, sharing the code on Launchpad, and so on... So, here it is, your project uses all previously described technology and is under revision control. You also have a nice .desktop file for showing your application in desktop menus for free. Enjoy! \o/</p>


<p>Then, we considerate that all subsequent commands will be run from a ubuntu-project "templated" project folder.</p>


<h3>run command</h3>

<p>If you've closed the application and want to run it again, change to your project directory, and use:</p>
<pre>
$ quickly run
</pre>


<p>This will run your project as a subprocess.</p>


<h3>tutorial command</h3>

<p>This command gives you the definitive must-have tutorial to know, indicating you what to do once your project is created, what can be changed, how to build it, etc. It goes also in some desktopcouch-db tweaking.</p>
<pre>
$ quickly tutorial
</pre>


<p>It will launch your default web browser and point you to a bunch of documentation to realize a successful ubuntu application.</p>


<p>Note that you can launch this command outside any project<sup>[<a href="#pnote-127-2">2</a>]</sup> too by spawning:<p>
<pre>
$ quickly tutorial ubuntu-project
</pre>

<p>or:</p>
<pre>
$ quickly -t ubuntu-project tutorial
</pre>



<p>That's enough for today. It was rather short, but you have now plenty of stuff to discover by yourself: you can now create your own project and play with it. We won't cover here what lines to change into the default code of your project or how to add new couchdb storage as all of this is already described in the tutorial. We will rather focus on commands.</p>


<p>Next stop will be on how to edit/change the content of your software, and save your stuff at regular interval.</p>
<div><h4>Notes</h4>
<p>[<a href="#rev-pnote-127-1">1</a>] *very* opinionated would say Rick Spencer ;)<p>
<p>[<a href="#rev-pnote-127-2">2</a>] just a quick reminder for those who didn't follow previous parts ;)</p><div>