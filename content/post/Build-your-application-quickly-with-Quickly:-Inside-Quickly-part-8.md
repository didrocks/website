+++
title = "Build your application quickly with Quickly: Inside Quickly part 8"
date = "2009-09-23T11:55:00+02:00"
tags = [ "devel", "quickly", "ubuntu" ]
aliases = ["/post/Build-your-application-quickly-with-Quickly%3A-Inside-Quickly-part-8"]
+++
    <p>You have now your amazing and remarkable new Quickly "ubuntu-project" templated project but don't know how to start hacking on it? Here are some tips for you, just there, keep on!</p>


<h2>Modifying your ubuntu-project</h2>


<h3>edit command</h3>


<p>Quickly edit is a convenient command to open all of your python files contained in your project in your default editor, ready for editing. Just run:</p>

<pre>
$ quickly edit
</pre>


<p>anywhere in your project tree.</p>


<p>It will most likely open them on gedit, apart in case you put other values in EDITOR or SELECTED_EDITOR environment variables. Consequently, if you previously configured your editor with sensible-editor, this one will be chosen<sup>[<a href="#pnote-130-1">1</a>]</sup>.<p>



<h3>glade command</h3>


<p>This command enables you to open all generated UI files in Glade, so that you can modify your interface. UI files are where your interface is described and Glade is here to give you some handy way of defining it.</p>
<pre>
$ quickly glade
</pre>



<p>Note that If you just run Glade from the Applicatons menu <strong>it won't work</strong> with Quickly. Indeed, what Quickly does is assume that there is one UI file for each Python class for each window type instead of a single big ui file that defines all of the UI for the whole project. This allows each class to derive from window, and most importantly from Dialog. Quickly needs to generate some xml files to tell Glade about these classes and if you just load Glade from the Applications menu, Glade doesn't get to see those UI files and won't load the UI files rather than risk corrupting them.</p>


<h3>dialog command</h3>


<p>This command helps you to create a new dialog into your project.</p>

<pre>
$ quickly dialog &lt;dialog_name&gt;
</pre>


<p>where &lt;dialog_name&gt; is one or more words separated with underscore.</p>


<p>This will create:</p>
<ol>
<li>A subclass of gtk.Dialog called DialogNameDialog in the module DialogNameDialog.py</li>
<li>A glade file called DialogNameDialog.ui in the ui/ directory</li>
<li>A catalog file called dialog_name_dialog.xml also in the ui/ directory</li>
</ol>

<p>The default opened file is the main window for your application. You can switch to others under the "Projects" menu.</p>


<h3>save command</h3>


<p>Ok, you should rather save your project at regular interval, using a revision control system (enabling you to revert back to any point in time, to share with other your code and merge their work into your). Quickly save enabling taking this kind of snapshot of your project:</p>

<pre>
$ quickly save notes about changes
</pre>


<p>where "notes about changes" is optional text describing what changes were made since the last save.</p>


<p>It basically commits all changes since the last save to bzr, using a default text if you don't specify one. If you need revert or otherwise use the revision control, use bzr directly.</p>


<p>Note that it does not push changes to any back up location.</p>



<p>Well, that's almost it. You can now really begin to work on your project using all the Quickly goodness. Next subject will be the last one: sharing your finished product and packaging it as easy as pie.</p>
<div><h4>Notes</h4>
<p>[<a href="#rev-pnote-130-1">1</a>] bryce told me "I was surprised that for once, I had my files opened with emacs" :)</p><div>