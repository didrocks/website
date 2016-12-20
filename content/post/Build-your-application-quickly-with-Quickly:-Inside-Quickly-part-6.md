+++
title = "Build your application quickly with Quickly: Inside Quickly part 6"
date = "2009-09-18T11:55:00+02:00"
tags = [ "devel", "quickly", "ubuntu" ]
aliases = ["/post/Build-your-application-quickly-with-Quickly%3A-Inside-Quickly-part-6"]
+++
    <p>This post will make a little pause in Quickly features reviewing to focus on the template creation. Ever wanted to create your own Quickly template, or to modify slightly the existing ubuntu-project template to make different choices? This part is for you!</p>


<p>If you aren't interested, we will begin the ubuntu-project template description next week, see you there!</p>


<h2>Quickly Templates</h2>

<h3>General stuff</h3>

<p>Templates can be written in <strong>whatever language you want</strong>. They are just a set of commands in a directory, containing commands to copy images, documents and interacting with the user.</p>


<p>Adding a command is quite easy: just drop it into the root template directory and make it executable. Quickly will know by this way that the current file is a command that Quickly Core can launch. Consequently, if you need additional internal commands that can be launched only by your own commands, just create a subdirectory in your template and add it there!</p>


<p>Ready to see how to create a template? Remember than template can be created for everything: generating advanced music list, managing LaTeX files, doing anything without any project (just having all commands that can be launched outside project), etc. You can even create a template with commands written in C to help users design a perl application. :)</p>


<h3>Template paths</h3>

<p>Quickly will retrieve template in multiple paths. To know which template paths are detected, you can use:</p>
<pre>
$ quickly --version
Quickly 0.2.3
  Python interpreter: /usr/bin/python 2.5.2
  Python standard library: /usr/lib/python2.5

  Quickly used library: /home/didrocks/Projets/quickly/quickly
  Quickly data path: /home/didrocks/Projets/quickly/data
  Quickly detected template directories:
          /home/didrocks/quickly-templates
          /home/didrocks/Projets/quickly/data/templates/

Copyright 2009 Canonical Ltd.
  #Author 2009 Rick Spencer
  #Author 2009 Didier Roche
https://launchpad.net/quickly

quickly comes with ABSOLUTELY NO WARRANTY. quickly is free software, and
you may use, modify and redistribute it under the terms of the GNU
General Public License version 3 or later.
</pre>


<p>What is interesting for us are the following lines:</p>
<pre>
  Quickly detected template directories:
          /home/didrocks/quickly-templates
          /usr/share/quickly/templates/
</pre>


<p>/usr/share/quickly/data/templates/ is what you should see if you installed Quickly with your package management system. That is the default location for templates.</p>


<p>If you built Quickly yourself and add some options like home= or root= (standard distutils options to specify a different home), your will find them in $your_targeted_directory/templates.</p>


<p>The first path (in my home folder), only appears if you have installed at least one template. This is the place for your personal templates that you don't want to share with user, or just for experimentations.</p>


<p>There are additional checks and possibles paths for people using trunk too, but that's not the point. :)</p>


<h3>Create a template from scratch</h3>

<p>The best way is to declare your template under ~/quickly-templates by creating a directory into it, like "my-awesome-template".</p>


<p>In it, create scripts/executable files and they will became commands. Easy, isn't it? :) The command name is the command script name without the extension (if present): <em>foo.py</em> becomes <em>foo</em> command, <em>bar.sh</em> will be <em>bar</em>, <em>foobar</em> remain <em>foobar</em> command.</p>


<h3>Create a template from another template</h3>

<p>But if you are found of an existing Quickly template but you disagree on the choice of a particular technology and want to modify it to apply it to your needs, there is an easy way to achieve this.</p>


<p>You don't have to copy yourself the corresponding templates, but instead, run:</p>

<pre>
$ quickly quickly &lt;origin template&gt; &lt;new template&gt;
</pre>


<p>So, for instance:</p>
<pre>
$ quickly quickly ubuntu-project kubuntu-project
</pre>


<p>It will create (if it doesn't already exist) ~/quickly-templates, the corresponding subdir and makes some additional checks.</p>


<p>The most important thing is that templates aren't binded: you can modify it independently than the one used at creation time. It's an easy and convenient way to start from existing work.</p>


<h4>Little brain breaker</h4>

<p>How to complicate it a bit? Oh well, if you really understood the previous parts, you can know how to do that differently:</p>
<pre>
wherever/you/are $ quickly -t ubuntu-project quickly kubuntu-project
</pre>


<p>or:</p>
<pre>
quickly/project/dir $ quickly quickly kubuntu-project
</pre>

<p>to copy the template associated with current project.</p>



<h3>Quickly Core does the hard work for you</h3>

<p>If you are using python or not to create your template, Quickly Core does already some part of the work for you. This enables you to relax and know that you are always launched in good conditions. Let's see what's automated.</p>


<p>If you didn't understood part 3, I urge you to read it again, you will need it to follow the next 2 sections :)</p>


<h4>Command launch context</h4>

<p>By default, all template commands (contrary to builtin ones) can be launched only in a project "templated" with your project. I guess you remember that there is some additional attributes that you can specify to change that behavior.</p>


<p>All the little black magic is in the commandsconfig file that has to be in the root directory of your template (its presence is not compulsory). Here is the example for the ubuntu-project template</p>

<pre>
# define parameters for commands, putting them in a list seperated
# by ';'
# if nothing specified, default is to launch command inside a project
# only and not be followed by a template
COMMANDS_LAUNCHED_IN_OR_OUTSIDE_PROJECT = tutorial
COMMANDS_LAUNCHED_OUTSIDE_PROJECT_ONLY = create
COMMANDS_FOLLOWED_BY_COMMAND =
</pre>


<p>As in every configuration files, you can add comments with <em>#</em>, and use <em>ATTRIBUTE = value</em> format. Quickly Core supports as well list in this format : <em>ATTRIBUTE = command1; command2; ...</em>. Of course, we are quite tolerant about the format (extra spaces, comments on the lines, etc.)</p>


<p>So, in that example, I can launch "quickly create ubuntu-project ..." outside a Quickly project only. As it's a template command that can be launched outside a project, it's a project followed by template (read chapter 3 again to see what it implies).<br>
All unlisted commands have the defaults: command not followed by a command nor a template and they can be launched inside a project only.</p>


<p>Also, COMMANDS_FOLLOWED_BY_COMMAND is almost self explanatory. Think about the <em>help</em> builtin command presented in part 3. (Not sure if any other command will need it, but well, all the logic is implemented in a generic way, so, let's get it available if some people need it) :)</p>


<p>Then, Quickly Core will do every checks to ensure that the command is in the good context and has the right parameters to be launched (or to propose this template if not specified for this command, etc.).</p>


<h4>Shell completion</h4>

<h5>Basic one</h5>

<p>The good news if you followed the previous section is that you have nothing to add. Shell completion automatically understands if the command should be available or not considering the current context (inside a project or not, a template already provided with the --template option on the command line, etc.) and propose you what you can use.</p>


<p>Same for templates proposal if you are executing a command out of a project. It will as well propose another command if you specified the right attribute.
Also, if a command name is in more than one template, it will automatically fetch to show you what is available to the user as previously described in part 4.</p>


<h5>Add advanced completion</h5>

<p>Ok, but I know, you want more than basic completion for your awesome template. For instance, the <em>license</em> command in the ubuntu-project template display available licenses.</p>


<p>Here, the logic is "logically" outside of Quickly Core as it shouldn't integrate template-specific stuff. So, once the basic completion will give no more solution (that is to say, context is ok, templates are specified, commands too if needed...), it will launched the script with <em>shell-completion</em> as the first argument, and then, what has been specified on the command line.</p>


<p>Let's take an example:</p>
<pre>
$ quickly -t ubuntu-project licence [Tab]
</pre>

<p>(Note that -t ubuntu-project is not compulsory if you are in a ubuntu-project "templated" project)</p>


<p>It will launch licence.py of ubuntu-project template project with <em>shell-completion ""</em> parameters.</p>

<pre>
$ quickly foo bar test [Tab]
</pre>


<p>It will launch foo script with <em>shell-completion bar test ""</em></p>


<pre>
$ quickly foo bar tes[Tab]
</pre>


<p>However, this latter will launch foo script with <em>shell-completion bar tes</em>. (as last parameter isn't empty, you know on which word tab completion is launched). But you still can show all candidates, like "you, should, test, it" and Quickly shell-completion script will fetch with current parameter (to choose here "test", only candidate which matches "tes").</p>


<p>Ok, but how to tell Quickly that "those results are values that can be used in shell-completion"? Easy, just print it! In the previous case, you can just execute in your code: <em>print "you should test it"</em> (or echo, or printf depending on the chosen language...). a blank space is needed but no order is required (it will be reordered automagically with <em><a href="/post/Tab" title="Tab">Tab</a><a href="/post/Tab" title="Tab">Tab</a></em>)</p>


<p>So, keep in mind that we can launch script with <em>shell-completion</em> parameter each time the user spawn shell completion. Conseuqnetly, design your commands accordingly (see later for a tools to help you with that if you are writing your template in python)</p>


<h4>Help to figure out help</h4>

<p>The help system use a similar feature. You just have to print the help when Quickly Core call your command with <em>help</em> parameter. Again, a convenient function will be described later for Quickly templates in python.</p>


<h4>Command line treatment</h4>

<p>Quickly Core is stripping all known options and arguments like templates (when following command like in command followed by templates without the -t option or not in a Quickly project) to have always the same state for the command.</p>


<p>With this, you are assured to have your own parameters to treat, not Quickly specific ones and no context to deal with. Focus on template coding, don't deal with this mess, Quickly Core does it for you :)</p>


<p>Special option that we saw in previous chapter is provided on commodities function for python (see below) or in environment variable. For instance, there will be a <strong>staging</strong> string in QUICKLY environment variable if the user used the <strong>--staging</strong> option or exported directly the variable. Same system is used for verbose mode. Quick reminder: multiple values are separated by ":".</p>


<h4>Launching from root project directory</h4>

<p>You can always consider your command is launched from project root directory, even if the user is in a subsubsubdir (or even deeper in the directory tree hierarchy) of the project. Quickly Core in all its cleverness and goodness guide your script to change their current working directory. :)</p>


<p>The only restriction is for commands that can be launched outside a project and that are actually launched in this context (outside of any project). This is a normal behavior, as they are specifically design to be launched outside a project. :)</p>


<p>This is the case, for instance, for the <em>create</em> command in the ubuntu-project template: there is a pre-hook executed (see below) and then, we launch the template in the parent dir of the project directory. Be aware of that. :)</p>



<h4>Hook system</h4>

<p>Quickly supports some hooks. It enables to have some specific automation (part of code) executed before and/or after an execution of a command. They are Quickly Core builtin. Now, we just have one: pre_create, launched before any create function.</p>


<p>You can note that hooks aren't execute when calling the script for shell-completion and help.</p>


<p>pre_create creates the directory specified in the <em>create</em> command line and handle more complex cases like <em>$ quickly create template folder1/folder2/projectname</em>. Also, pre_create filters acceptable project names and populates <em>.quickly</em> file in the new project directory to put some generic parameters needed for Quickly Core compatibility handling.</p>


<p>So, if your template needs a Quickly project, you really should consider adding a <em>create</em> command which can be launched outside a project only. It will be a bad idea to create a project within an existing project, even if Quickly Core supports it. :)</p>


<h4>.quickly?</h4>

<p>This file should be located in the root project directory (it's how Quickly Core detects it, to be honest). The basic pre_create hook creates a file like that:</p>
<pre>
project = foo
template = ubuntu-project
format = 0.2.3
</pre>


<p>With that, we can store project name (which can differs with the folder case), template names, and Quickly format (corresponding to Quickly version) to handle migration.</p>


<p>You can add your own key/values pairs (again a convenient function is given if you develop your template in python) to help you in your template needs. The file format is the same than for quicklyconfig template file (comments, extra spaces, etc.).</p>


<h4>tools to help you (if your template is in python)</h4>

<p>Most of them are in quickly/templatetools and quickly/configurationhandler.</p>


<h5>access and modify .quickly</h5>

<p>configurationhandler is your friend for this one :)</p>


<p>The basic way is to import the module and load the content of the <em>.quickly</em> file:</p>
<pre>
from quickly import configurationhandler
if not configurationhandler.project_config:
    configurationhandler.loadConfig()
</pre>


<p>Here, we avoid to reload the configuration. You can do it if there are pending changes you want to forget. Then, you have an associated array called "project_config" and use it to retrieve some values:</p>

<pre>
print(configurationhandler.project_config['project'])
</pre>

<p>(if the key doesn't exist, it will return the usual no index found).</p>


<p>You can then add new values that you need in your own template by:</p>
<pre>
configurationhandler.project_config['foo']=bar
</pre>


<p>To save it, you can finally call:</p>
<pre>
configurationhandler.saveConfig()
</pre>


<p>All modifications will be stored by <em>.quickly file</em> within the current project.</p>



<h5>handling easily help and shell-completion</h5>

<p>For that, we have a handle_additional_parameters which takes function as optional parameters. Those functions should print help and handle shell_completion method. In the following examples, the functions are names "my_help" and "my_shell_completion", but that doesn't matter.</p>

<pre>
from quickly import templatetools
def my_help():
    print _(&quot;&quot;&quot;Usage:
$ quickly change-lp-project

Enable to set or change the launchpad project binded with the current
ubuntu project.
&quot;&quot;&quot;)
templatetools.handle_additional_parameters(sys.argv, my_help)
</pre>


<p>As you notice, some parameters are optionals. Here is the example will both functions:</p>
<pre>
def my_shell_completion():
    if len(sys.argv) == 3:
        print &quot; &quot;.join(get_supported_licenses())
def my_help()
     print(&quot;foo&quot;)
templatetools.handle_additional_parameters(sys.argv, help=my_help, shell_completion=my_shell_completion)
</pre>



<h5>Get formated Quickly name of a project:</h5>


<p>Very easy function to get your project name compatible with Quickly Core (this is the same one which is put in .quickly file)</p>
<pre>
from quickly import templatetools
print(templatetools.quickly_name(project_name))
</pre>

<p>An error is issued if the input is not supported currently by Quickly Core.</p>



<h5>apply file permission to another file</h5>

<p>This has been handy in the <em>license</em> command of ubuntu-project, it can interest others:</p>

<pre>
from quickly import templatetools
templatetools.apply_file_rights(fcopyright.name, fcopyright_out.name)
</pre>


<p>It applies to the second file path the permission of the first path file.</p>



<h5>Launchpad and bzr</h5>


<p>Disclaimer: we put support in Quickly Core itself for Launchpad and bzr, but nothing is loaded until you explicitly need it (and consequently, specify it) in your template. We will be happy to add other support from widely used system for template creator convenience. So, do not hesitate to propose your own binding.</p>


<p>We have some generic tools now for binding to launchpad project and creating/using credential so that template's developers don't become crazy if they want to use Launchpad in their template.</p>


<p>Basically, to setup launchpad credential and bind to a project (or use it afterwards), the code is:</p>

<pre>
# connect to LP (this create credential if needed, either, retrieve it)
from quickly import launchpadaccess
launchpad = launchpadaccess.initialize_lpi()
</pre>


<p>Once the user created his credential (or was connected automatically if it's not the first time it's called within Quickly), you have your launchpad python object to perform whatever you wish. It automatically targets staging if it was specified on the command line or by environment variable. It also setup bzr whoami if it was not done before.</p>


<p>All the "first configuration" step is done only once in Quickly: the credential is shared within every templates.</p>

<pre>
# get the project (choose and bind to one if not already done)
project = launchpadaccess.get_project(launchpad)
</pre>


<p>With that code, once connected to Launchpad, you will bind your project to a launchpad project. It will ask the user (the first time it's launched within a project) to what existing project he wishes to be bound, make a search with the string provided and propose some candidates to him. When done, you can play with the "project" python object in your template!</p>


<p>Staging stuff is also supported without any extra work. If you really need to know if you are on staging server, you can launch: ///launchpadaccess.lp_server///
(it can be "staging" or "edge"). No need to parse environment variable. :)</p>


<h5>verbose mode</h5>

<p>It's really easy to figure out if you are in verbose mode or not (specified by the user by environment variable or by using command line option):</p>

<pre>
from quickly import templatetools
if templatetools.in_verbose_mode():
</pre>


<p>in_verbose_mode() will be True of False, it's self-explanatory!</p>



<p>Well, Quickly Core is not so tiny as we can infer at first glance and a lot is covered under the hood too. It's not only a dummy command line parser as we are used to present it for simplicity, but you have been able to see that it has some advanced function as well.</p>


<p>Next week, we will see the first created template for Quickly. You already know some part of this one as I took it a lot of times as an example: yes it's the one called <em>ubuntu-project</em>! Back again to the user wave. :)</p>