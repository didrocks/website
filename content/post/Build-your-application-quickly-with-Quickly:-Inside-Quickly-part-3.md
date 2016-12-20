+++
title = "Build your application quickly with Quickly: Inside Quickly part 3"
date = "2009-09-11T11:55:00+02:00"
tags = [ "devel", "quickly", "ubuntu" ]
aliases = ["/post/Build-your-application-quickly-with-Quickly%3A-Inside-Quickly-part-3"]
+++
    <p>Here we go for another topic on Quickly tour! This chapter will be really fast to read compared to last one.</p>


<h2>Getting some help with Quickly</h2>

<pre></pre>

<p>Quickly has an builtin help command that can help you to retreive help from whenever command.</p>


<h3>Inside a project</h3>

<p>You can search directly help for builtin command and the ones from the associated template of your project.</p>


<h4>template commands</h4>

<p>In a ubuntu-project "templated" project, simply do:</p>
<pre>
$ quickly help release
</pre>

<p>to get some help from the release command of ubuntu-project template.</p>


<h4>builtin command</h4>
<pre>
$ quickly help commands
</pre>

<p>give some help on the builtin <em>commands</em> command.</p>


<h4>commands from another template</h4>

<p>As for launching commands, you can get help from other template command by:</p>
<pre>
$ quickly --template footemplate help awesome
</pre>

<p>This will print the help associated to the awesome command of the footemplate template. Nice, isn't?</p>


<h3>Outside a project</h3>

<p>There, you can get help from builtin command and still from template command</p>

<h4>builtin command</h4>

<p>As previously:</p>
<pre>
$ quickly help getstarted
</pre>


<h4>template commands</h4>

<p>From the last part, you should be able to guess what's the attribute of the help command...
You got it! It's a command followed by template (but also, as I said too, a command followed by command, cf previous example)</p>


<p>Taking, again the example of ubuntu-project and getting some help from the release command:</p>
<pre>
$ quickly help ubuntu-project release
</pre>


<p>If you followed all the previous concepts successfully, how can I do that differently?</p>
<pre>
$ quickly -t ubuntu-project help release
</pre>

<p>got it!</p>
<pre>
$ quickly help -t ubuntu-project  release
</pre>

<p>and</p>
<pre>
$ quickly help release -t ubuntu-project
</pre>

<p>works as well!</p>


<p>Note that in the last case, you don't have shell-completion for the "release" field (as the template is defined after the help command)</p>


<p>As usual, completion is there to help you in all those cases.</p>


<p>All is done by the core itself, if you develop a template, you will get all those automation from Quickly Core for nothing! Great, isn't?</p>


<p>Next chapter will be on the shell-completion itself! It will help you to understand that the expression "all is guided through bash-completion" is not a wild dream.</p>


<p>NOTES:</p>
<ol>
<li>no, quickly help help as "googling google" doesn't make the world dying from entropy creation cycle.</li>
<li>yes, quickly quickly exist. What does it do? <em>$ quickly help quickly</em> to know it, of course :)</li>
</ol>