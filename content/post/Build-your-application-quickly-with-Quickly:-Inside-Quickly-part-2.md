+++
title = "Build your application quickly with Quickly: Inside Quickly part 2"
date = "2009-09-09T11:55:00+02:00"
tags = [ "devel", "quickly", "ubuntu" ]
aliases = ["/post/Build-your-application-quickly-with-Quickly%3A-Inside-Quickly-part-2"]
+++
    <p>This session is giving some fundamentals on Quickly, let's dive into them!</p>


<h2>Two components: core and template</h2>


<p>There is mainly two parts in Quickly:
<ins>Quickly Core</ins> is a command line parser and context checker. It has <strong>builtin</strong> commands and can handle and launch <strong>template</strong> commands.
<ins>Quickly templates</ins> are group of commands and files that are used on a certain purpose: you can create templates to manage your documents, a LaTeX skeleton, or to easily create projects gathering a certain number of technologies.</p>


<p>Consequently, using templates, you can create a project which will be binded to your templates. Templates can be written in whatever language you wish. That's the reason why we don't use optparse in Quickly Core component.</p>


<p>The application can be extended to run on other systems, and use different tools, etc... There is nothing stopping you from doing that. However, if you stray off the chosen path, some of the commands may no longer work for you.</p>


<p>We would love to see, for instance, a fedora-project template, gnome-project one, plasmoid-project, zeitgeist-plugin... and this tour will give you some trick to help you create one (in part 6).</p>


<p>But again, consider that you can create your template for everything, like managing your documents (without even creating any project). You can give some commands to create a new "company's set of documents" for a secretary ; he/she will complete them and then "quickly share" (for instance) for uploading it in the company's sharing documentation server. Quickly Core is not binding into developing only, power is completely given to template developers.</p>


<h3>The ubuntu-project templates</h3>

<p>The ubuntu-project template is the first template released today (some more coming for creating gedit plugins and ubuntu-game template).</p>


<p>The ubuntu-project template enables you to easily create a project using a given set of strong opinionated chosen technology ubuntu's people are mostly using:</p>
<ul>
<li>Python for the language</li>
<li>pygtk for the UI framework</li>
<li>Glade for the UI editor</li>
<li>Gedit for the code editor</li>
<li>bzr for version control</li>
<li>Launchpad for code hosting</li>
<li>desktopcouch for storage/database</li>
</ul>

<p>The idea is to make choices (as Ubuntu make choices in their default applications list) for developers wondering what technology is "good to use for...". Choosing in a technology jungle can sometimes be a mess. Nevertheless, this is just a the skeleton: nothing prevents you for removing the desktopcouch part if you don't want to use it for instance (same for other pieces of technology).</p>


<p>It's also easy to create your own template from ubuntu-project to remove what part you don't want (if you are found of git and don't want to use bzr, for example). More on that in next sections.</p>


<p>Also note, there is no Quickly runtime or base class library. Using the Ubuntu Project won't bring in any dependency on Quickly itself.</p>


<h3>Templates, context and command management.</h3>

<h4>General</h4>

<p>So, we have builtin and template command and you can also be inside or outside a project. All that forms a context.</p>


<p>A project created with the <strong>create</strong> command will be associated with the template used in the create command like in:<br>
$ quickly create ubuntu-project foo<sup>[<a href="#pnote-122-1">1</a>]<sup><br>
This command can only be launched outside any Quickly project and needs a template to be specified on the command line.</p>


<p>Then, when you are in a project, you can launch <em>$ quickly &lt;command&gt;</em> to launch the &lt;command&gt; from the binded templates or Quickly builtin command. No need to specify a template.</p>


<p>Also, you can launch any command wherever you wish in the project hierarchy (subfolder, subsubfolder...), the command will behave the way it's designed.</p>


<h4>Get the list of all available commands.</h4>

<p>Nothing more easy, just run wherever you wish:</p>

<pre>
$ quickly commands
[builtins]      commands
[builtins]      getstarted
[builtins]      help
[builtins]      quickly
[ubuntu-project]        change-lp-project
[ubuntu-project]        create
[ubuntu-project]        dialog
[ubuntu-project]        edit
[ubuntu-project]        glade
[ubuntu-project]        license
[ubuntu-project]        package
[ubuntu-project]        release
[ubuntu-project]        run
[ubuntu-project]        save
[ubuntu-project]        share
[ubuntu-project]        tutorial
</pre>


<p><em>builtins</em> are... builtins command and others are templates owning commands. You can even check there that <em>commands</em> is a builtin one.</p>


<h4>Choosing the template</h4>

<p>You can still launch commands from another template in your current project. Let's say you need the "awesome" command from the bar template to be launched in your current ubuntu-project "templated" project, you still can with <code>$ quickly --template bar awesome</code><sup>[<a href="#pnote-122-2">2</a>]</sup><p>


<h4>Attributes of commands/Advanced brain breaker :)</h4>


<p>WARNING: this part can be seen as quite tricky. It's needed if you really want to understand the template and builtin commands behavior or want to create your own template. If you don't understand/want to read it, don't desesperate and just jump into the "<strong>Ok, you killed me, what do I really need to know?</strong>" section :)</p>


<h5>Command attributes</h5>


<p>As already said, some commands (templates and builtin ones) can either:</p>
<ul>
<li>be launched outside a project only (case of "<strong>create</strong>" from ubuntu-project template command, for instance)</li>
<li>be launched inside a project only (case of "<strong>release</strong>" from ubuntu-project template command, for instance)</li>
<li>be launched outside or inside a project (case of "<strong>tutorial</strong>" from ubuntu-project template command, "commands" builtin command, for instance)</li>
</ul>

<p>The check is done during command launch (and shell-completion filtering as well) to ensure user doesn't perform any mistake (as users make some mistakes sometimes, don't they? ;)).
For instance, if you try to launch a command which can only be launched inside a project:
didrocks@laptop:~/not_a_quickly_project$ quickly --template ubuntu-project release<br>
ERROR: Can't find project in /home/didrocks/not_a_quickly_project.<br>
Ensure you launch this command from a quickly project directory.<br>
Aborting</p>



<p>Template command launched outside a project must be followed by a template if not specified as an option. Of course, this enables Quickly Core to know in which template to find the command. This will give:</p>

<pre>
$ quickly create ubuntu-project foo
                 ^^^^^^^^^^^^^^ ^^^
                     template  command arg
</pre>

<p>or:</p>
<pre>
$ quickly -t ubuntu-project create foo
          ^^^^^^^^^^^^^^^^^        ^^^
          template as an option  command arg
</pre>

<p>Same for tutorial command, which is an ubuntu-project command:</p>

<pre>
$ quickly tutorial ubuntu-project
                   ^^^^^^^^^^^^^^
                   template following command
</pre>

<p>or:</p>
<pre>
$ quickly -t ubuntu-project tutorial
          ^^^^^^^^^^^^^^^^^
         template as an option
</pre>

<p>Note: <strong>-t templatename</strong> can be set wherever on the command line.</p>


<p>As we will see in a later part, shell-completion automatically propose template commands that can be launched outside a project if you are outside a project, or inside if you are ... in a project folder. :)</p>


<p>As builtin commands are generalists, most of them doesn't need any template. But some have the attribute "followed by template" which will need also a template (example: 'help', 'quickly') specified on the command line.</p>


<p>An additional attribute we will se later is "followed by command" (case of the help command). There will be a dedicated part of the help.</p>


<h5>Launch a template command outside a project</h5>

<p>In case you launch a template command which is part of a template without specifying the template (either with -t option or following the command), you will have a message like that:<br>
$ quickly tutorial<br>
ERROR: tutorial command must be followed by a template and no template was found on the command line.<br>
Candidates template are: footemplate, ubuntu-project<br>
Arborting.</p>


<p>That means that both footemplate and ubuntu-project templates have a <strong>tutorial</strong> command and Quickly in all its kindness found them to help you. :)</p>


<p>So, then, you can try to launch (if quickly tutorial doesn't take any additional argument, which is the case):
<em>$ quickly tutorial footemplate</em> or <em>$ quickly tutorial ubuntu-project</em>.</p>


<h2>Ok, you killed me, what do I really need to know?</h2>


<p>Phew, this was the harder part of Quickly's concept. Now, the following will seem to be really easy once you get this. In any case, shell-completion will only propose in the current context what you need/can launch (we will discuss that later). So, if you are lost, just let you guided by it, it will propose whenever template/command args you need to specified in the current context. :)</p>


<p>Only knowing what is a template and a project should be sufficient for a daily (and happy? ;)) Quickly user.</p>




<p>Next part will be on getting help on commands.</p>
<div><h4>Notes</h4>
<p>[<a href="#rev-pnote-122-1">1</a>] foo can be also path/to/foo if path/to exists<p>
<p>[<a href="#rev-pnote-122-2">2</a>] -t instead of --template if you are lazy :)</p><div>