+++
title = "Build your application quickly with Quickly: Inside Quickly part 5"
date = "2009-09-16T11:55:00+02:00"
tags = [ "devel", "quickly", "ubuntu" ]
aliases = ["/post/Build-your-application-quickly-with-Quickly%3A-Inside-Quickly-part-5"]
+++
    <p>That will be the last part for user on the Quickly Core itself. Before jumping on the ubuntu-project description road, we will see miscellaneous stuff that I couldn't classified later.</p>


<h2>Miscellaneous</h2>

<p>Well, Quickly options are quite sparse for the moment (as we don't need more as of today), so, here are what can be useful for you:</p>


<h3>Help</h3>
<pre>
$ quickly --help
</pre>


<p>Of course, as the man page, this will give you the most important information you will certainly need :)</p>


<h3>Version</h3>
<pre>
$ quickly --version
Quickly 0.2.3
  Python interpreter: /usr/bin/python 2.6.2
  Python standard library: /usr/lib/python2.6
  
  Quickly used library: /usr/lib/python2.6/dist-packages/quickly
  Quickly data path: /usr/share/quickly/data
  Quickly detected template directories:
          /home/didrocks/quickly-templates
          /usr/share/quickly/data/templates/

Copyright 2009 Canonical Ltd.
  #Author 2009 Rick Spencer
  #Author 2009 Didier Roche
https://launchpad.net/quickly

quickly comes with ABSOLUTELY NO WARRANTY. quickly is free software, and
you may use, modify and redistribute it under the terms of the GNU
General Public License version 3 or later.
</pre>

<p>This one is very important: it gives you some internal and useful informations like Quickly version, python version, what paths for library are used by Quickly, template paths. Well, it's a good idea for every bug report to add those information. We are currently working on an apport hook to integrate it each time Quickly crashes.</p>


<h3>Verbose mode</h3>
<pre>
quickly --verbose &lt;command&gt;
</pre>


<p>This enables Quickly to be more verbose. It's pretty limited at this time, but will be developed later (for instance, we remove current glade warning about unknown icons). Verbose mode switch back default behavior. It will be neat if we will integrate more verbose debugging and verbose informations (planned for 0.4 release).</p>


<p>If you want to use verbose mode for more than one command without specifying each time a --verbose option, you can export QUICKLY variable and set it to verbose like this:</p>
<pre>
export QUICKLY=verbose
</pre>


<p>Then, all commands will be executed in verbose mode.</p>


<h3>Specific values like staging launchpad server</h3>


<p>Using --staging switch enables you to execute all related Launchpad stuff without reaching the real Launchpad server, but staging one instead. This is more focused on testing and is dedicated to specific command that needs Launchpad (share and release command, for instance, in ubuntu-project template). In a similar way, we can add support to other "staging" or equivalent system with that command.</p>


<p>As for verbose, you can export an environment variable to avoid specifying --staging in each command:</p>
<pre>
export QUICKLY=staging
</pre>


<h3>But I want to use both of them</h3>

<p>Don't panic! You can of course, add the two options like in every command line, but you still can export a QUICKLY environment variable. The field separator is ":" (as in PATH), so just add the two strings separated by it, like in:</p>
<pre>
export QUICKLY=verbose:staging
</pre>


<p>This will enable us to extend those functionalities in the near future if the Core or some templates needs it. We don't want to add too many clutter in environment variables, that's why we have only choosen QUICKLY environment variable and use IFS to separate values. Of course, that's only if you have a long suit of commands using this option, otherwise, using the corresponding option is a lot easier. All those values can be retrieved by templates: it's easier if you use a python based template, but it's transposed in environment variable whatever what was used (option or environment variable). We will detail that in next chapter.</p>



<p>Ok, that's it for the user side of Quickly Core. But I'm sure you have now an idea of an awesome template and you are eager to create it. You probably miss some information on that. Consequently, in next session, we will see how to create our own template. This will finish the Quickly Core tour itself. :)</p>