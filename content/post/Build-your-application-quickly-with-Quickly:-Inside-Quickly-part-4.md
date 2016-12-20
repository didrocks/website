+++
title = "Build your application quickly with Quickly: Inside Quickly part 4"
date = "2009-09-14T17:55:00+02:00"
tags = [ "devel", "quickly", "ubuntu" ]
aliases = ["/post/Build-your-application-quickly-with-Quickly%3A-Inside-Quickly-part-4"]
+++
    <p>Yeah! It's already the 4th zoom of your Quickly trip and it's about shell completion.</p>


<h2>Complete everything quickly</h2>

<p>Quickly has an advanced shell completion tool. Currently, it's only binded to bash but plans are up to catch other shells (it's very easy, just call quickly shell-completion $0 from a script and perform little black magic to know the context).</p>


<p>You can reread part 2 if you want to understand every cases described below. Ensure as well you have Quickly 0.2.3.</p>


<h3>After installation</h3>

<p>Once Quickly is set up, you have to launch a new bash to get the bash completion working. This hasn't some workaround and it's the same with other tool (bzr, quilt...)</p>


<h3>Ok, let's try a basic example:</h3>

<h4>Outside any project</h4>
<pre>
quickly [Tab][Tab]
awesome commands    create      getstarted  help        quickly     tutorial
</pre>


<p>-&gt; We get there all builtin and template commands which can be launched outside a project.</p>


<p>Here, <em>commands, getstarted, help and quickly</em> are builtin commands. <em>Create and tutorial</em> are ubuntu-project template commands.</p>


<h5>Now, let's try a basic builtin command:</h5>
<pre>
$ quickly commands [Tab][Tab]
&lt;file list of my current folder&gt;
</pre>

<p>Ok, that means that we have no parameter to specify or that the <em>commands</em> command don't support more advanced completion.</p>


<h5>Testing a template command which can be launched outside a project:</h5>
<pre>
$ quickly tutorial [Tab][Tab]
footemplate               ubuntu-project
</pre>

<p>This is because the tutorial command is (in my personal installation) present in two templates, so as this command can be launched outside a template, you need to specify a template.</p>


<p>Then, picking one template:</p>
<pre>
$ quickly tutorial ubuntu-project [Tab][Tab]
&lt;file list of my current folder&gt;
</pre>

<p>That means that tutorial command doesn't have advanced shell completion either</p>


<p>Note that you can also do:</p>
<pre>
$ quickly --template ubuntu-project tutorial [Tab][Tab]
&lt;file list of my current folder&gt;
</pre>

<p>This time, as you have already defined a template, Quickly Core won't bother you to propose a template after the tutorial command.</p>


<h5>Advanced!</h5>

<p>You should understand why this doesn't display more commands:</p>
<pre>
quickly -t ubuntu-project [Tab][Tab]
commands    create      getstarted  help        quickly     tutorial
</pre>

<p>What's the difference with the first example? There is no <em>awesome</em> command<sup>[<a href="#pnote-124-1">1</a>]</sup>! It only lists builtin and ubuntu-project commands that can be launched outside a project. So, <em>awesome</em> command is certainly from another template than ubuntu-project in this case.<p>


<h4>Inside a project</h4>

<h5>Common case</h5>

<p>Let's give it a try:</p>
<pre>
$ quickly [Tab][Tab]
change-lp-project  getstarted         package            save
commands           glade              quickly            share
dialog             help               release            tutorial
edit               license            run
</pre>

<p>The list is made from builtin and associated template (here <em>ubuntu-project</em> template) command that can be launched inside the project (for instance, the <em>create</em> command is not listed).</p>


<h5>From another template</h5>

<p>As for help and launching a command, I can see commands from others templates, being in the same directory, that I can launch:</p>
<pre>
$ quickly -t footemplate [Tab][Tab]
commandfoo  getstarted  package     run
commands    help        quickly     tutorial
</pre>

<p>Here, I get builtin and footemplate commands that I can launch inside a project. Easy!</p>



<h3>Advanced completion</h3>

<p>As previously said, some commands can have advanced completion.</p>


<h4>Command defined</h4>

<p>They can defined it themselves (apart from the template part and command which is automatically handled by Quickly Core).</p>


<p>For instance, the license command from ubuntu-project defines an advanced (optional) parameters which is the license, Using shell-completion enable to discover every available licenses supported by ubuntu-project template:</p>


<p>Inside a project:</p>
<pre>
$ quickly license [Tab][Tab]
BSD     GPL-2   GPL-3   LGPL-2  LGPL-3
</pre>


<h4>Command followed by command (headaches inside)</h4>

<p>Do you remember part 2? I discussed about attributes on commands and we see before command followed by templates. There are also commands followed by another command. As of today, only the <em>help</em> builtin command has this attribute. But remember: help command is also a command followed by a template. Seems to be an interested case for testing shell completion behavior, isn't?</p>

<h5>Outside a project</h5>
<pre>
$ quickly help [Tab][Tab]
commands        help            toto
getstarted      quickly         ubuntu-project
</pre>


<p><em>help</em> completion propose you with what you can get some help:</p>
<ul>
<li>directly builtin commands which can be launched outside any project</li>
<li>available templates (for later completion)</li>
</ul>
<pre>
$ quickly help ubuntu-project [Tab][Tab]
change-lp-project  edit               license            run
commands           getstarted         package            save
create             glade              quickly            share
dialog             help               release            tutorial
</pre>


<p>Here you get help on all builtins and commands from ubuntu-project template (as see in previous part for the help command)!</p>


<p>For bonus point:</p>
<pre>
$ quickly -t ubuntu-project help [Tab][Tab]
change-lp-project  edit               license            run
commands           getstarted         package            save
create             glade              quickly            share
dialog             help               release            tutorial
</pre>

<p>Same thing than previously, but using the -t option.</p>


<h5>Inside a project</h5>

<p>In this cas, help command completion will behave as expected after reading last chapter:</p>
<pre>
$ quickly help [Tab][Tab]
change-lp-project  edit               license            run
commands           getstarted         package            save
create             glade              quickly            share
dialog             help               release            tutorial
</pre>

<p>You have there all builtins and ubuntu-project template commands (given the fact that you are in a ubuntu-project "templated" project).</p>


<p>Bonus, for people who likes to complicate their life:</p>
<pre>
$ quickly -t footemplate help [Tab][Tab]
awesome     commands    getstarted  package     run
commandfoo  create      help        quickly     tutorial
</pre>

<p>Here, you are in a ubuntu-project "templated" project and you get help on all builtins commands and those from footemplate.</p>


<h4>Miscellaneous</h4>

<h5>Option completion</h5>

<p>Option benefit of course from completion:</p>
<pre>
$ quickly -[Tab][Tab]
-h          --help      --staging   -t          --template  --verbose   --version
</pre>


<h5>Template completion</h5>

<p>In addition to previous case of template completion, the -t/---template command has its own completion!</p>
<pre>
$ quickly --template [Tab][Tab]
footemplate               ubuntu-project
</pre>



<p>Good news is all of that is generically implemented in the Quickly Core itself. So, if you plan to create a Quickly template, you can get it also for free if you put right attributes to your personal commands (we will see that in a chapter 6).</p>


<p>Hope you will like this advanced completion setting (it gave me quite some headaches and a lot of trial/refactoring phases before getting it generically working and the code not being too ugly). Remember again that we will see how command can add to this completion their own level, as for help. :)</p>


<p>In next part, we will see miscellaneous things on the Quickly core itself.</p>
<div><h4>Notes</h4>
<p>[<a href="#rev-pnote-124-1">1</a>] this does not imply that the listed command aren't awesome ;)</p><div>