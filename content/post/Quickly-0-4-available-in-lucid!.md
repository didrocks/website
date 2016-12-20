+++
title = "Quickly 0.4 available in lucid!"
date = "2010-04-15T12:40:00+02:00"
tags = [ "devel", "PU", "quickly", "ubuntu" ]
aliases = ["/post/Quickly-0.4-available-in-lucid%21"]
+++
    <p>I'm proud to announce the availability of <a href="https://launchpad.net/quickly" hreflang="en">Quickly</a> 0.4 in lucid. This new release brings  shiny new features (more than 6 months of heavy development), lowering again the barrier for <a href="http://www.jonobacon.org/2010/01/30/connecting-the-opportunistic-dots/" hreflang="en">opportunistic developers</a>. Development should just be easy and fun!
<img src="/public/projects/quickly/quickly-logo.png" alt="Quickly Logo" style="float:left;margin:0 1em 1em 0" title="Quickly Logo, sept. 2009"></p>



<p>Thanks to all awesome contributors making this release happened: Philip Peitsch, Petar Vasić, Jens Persson, Łukasz Jernaś Brian, Jonathan Lange and Shane Fagan. Special kudos to <a href="http://theravingrick.blogspot.com/" hreflang="en">Rick Spencer</a> for his continue devotion to opportunistic development and making quickly-widgets.</p>


<h2>What's new in 0.4?</h2>


<p>Well, a lot of stuff, but more precisely, let's me highlight some special features from the <a href="https://launchpad.net/quickly/0.x/0.4" hreflang="en">NEWS</a> file content, containing the full story.</p>


<h3>The core</h3>

<ul>
<li>One of the main matter for a lot of user was to use space and dashes as their name project. This wasn't available due to a lot of technical and packaging issues. This is part of past now! You can create your awesome "FoO bAr" project and enjoy it!</li>
</ul>
<ul>
<li>What's new in the core is template inheritance. You can now create a template picking commands from other templates. That made possible releasing the ubuntu-cli template with <strong>0</strong> line of code! Just shipping a new boiler plate. We hope that would help people in their template creation. Of course, it's still possible to override some commands locally.</li>
</ul>
<ul>
<li>We also check and recreate your credential if you deleted it in Launchpad inadvertently. Less thing to care about for you! Let's focus on <strong>your</strong> app!</li>
</ul>
<ul>
<li>Did you remember how sweet is Quickly regarding shell completion? Now the same has been extended to Quickly option itself.</li>
</ul>
<ul>
<li>An apport hook for Quickly itself. To report bugs on Quickly, do not hesitate 'ubuntu-bug quickly' on Ubuntu to fetch all needed information on our side.</li>
</ul>
<ul>
<li>The core now support <code>--</code> arguments for subcommands like quickly run <code>-- -help</code> won't run help from Quickly itself but from your app.</li>
</ul>
<ul>
<li>A Quickly API is available, for people wanting to communicate or integrate Quickly to an IDE, it's fully possible to embed Quickly in your app, spread the Quickly love!</li>
</ul>

<h3>The ubuntu-application template and derivatives:</h3>


<h4>ubuntu-project renamed</h4>

<p>One of the main feature of 0.2.x was the ubuntu-project template. This one has been renamed to <strong>ubuntu-application</strong>. Your app will be transitioned automatically to this new template thanks to the core supporting upgrade in templates and projects now<sup>[<a href="#pnote-174-1">1</a>]</sup><p>


<p>Consequently, no more <code>quickly create ubuntu-project fooby</code>, but <code>quickly create ubuntu-application fooby</code>!</p>


<h4>GPG key setup</h4>

<p>What seemed to have given a lot of trouble to Quickly users in 0.2.x was the gpg key setup. For those who don't know, this key is needed when you upload your package to launchpad. Now, Quickly is more clever about this and doesn't bring random bits to create your email address. This happens in two steps:</p>

<ol>
<li>collecting all available email addresses that Quickly can find and order them in the best possible order (DEBEMAIL, EMAIL, the AUTHOR file, setup.py your Launchpad emails account…). The thing is that some of those addresses may not be yours if more than one person works on the project, that's where step 2 occurs.</li>
<li>compare with all available local GPG key and those uploaded to launchpad.<sup>[<a href="#pnote-174-2">2</a>]</sup>. This enables us to know who YOU are and what key you should use to sign your package. This part involved patching Launchpad and fun hack evening with jml ;)<li>
</ol>

<p>Another patch for ssh key has been approved in Launchpad too, bit we will integrate the client side in 0.6. <strong>Easy</strong> and <strong>fun</strong>, didn't I say that already?</p>


<h4>Choosing and sharing to multiple ppa and bzr branches.</h4>

<p>You can now choose to which ppa you want to share/release your code. You can use Use <code>--ppa ppaname</code> or<code> --ppa team/ppaname</code> (shell completion even work with that! I must admit if it's very slow (more than 10s) due to a lot of requests to Launchpad). You can setting that up definitively by <code>quickly configure ppa &lt;ppa_name&gt;</code>.</p>


<p>The same with bzr branch is also possible (<code>quickly configure bzr &lt;bzr_branch&gt;</code>).</p>


<h4>Easier licensing</h4>

<p>Previously, license command asked you to fill a Copyright file which can contain tweaked copyright and author names. Now, this file has been removed and we keep only one AUTHORS file where you put your name and those participating in the project. You can create your own COPYING file<sup>[<a href="#pnote-174-3">3</a>]</sup> for your own licence, otherwise, it will license with command line supplied license (MIT has been added). At first share/release, the license is now automagic if you didn't licensed previously, and it will  take your name/email and license your proect under GPLV3. Each share/release command still update the license to all your files as in 0.2.x.<p>


<p>Again, all your project created with 0.2 will be migrated automatically to this scheme. Don't bother about about licensing if it doesn't interest you. Quickly is doing the right thing (at least, we try) for you.</p>


<h4>Automatic about dialog</h4>


<p>The about dialog is now fiddled automatically with up-to-date information at each &quot;release&quot; command. It will add your logo, the current version of the program, home page, the copyright, the authors, license… No more need to care about having this updated manually ! (the Credits show the authors, the licence… the licence :))</p>


<p><img src="/public/projects/quickly/Capture-A_propos_de_Slip_Cover.png" alt="A Propos de... Quickly 0.4" style="display:block;margin:0 auto" title="A Propos de... Quickly 0.4, avr. 2010"></p>


<p>hum, it seems that Rick is already enjoying using spaces in his project name ;)</p>


<h4>New versionning scheme</h4>


<p>The ubuntu-application template brings a new version scheme to avoid errors on failed share or release package (the critical section has been narrowed). So, if you release this month (April 2010), your applications would be <strong>10.04</strong>. If you release a new version again in the same month, it will be <strong>10.04.1</strong> and so on… When you share, it just takse the last version and adds -publicX like <strong>10.04.1-public1</strong>, then  <strong>10.04.1-public2</strong> and so on. Of course, it's still possible to provide your own version as a parameter of <code>quickly release</code> or <code>quickly share</code>, but you won't have the full sweet safety field of Quickly for avoiding rejected package :)</p>


<h4>Enhanced release command.</h4>


<p>Making a release is quite tedious. In addition to push your code in your branch and the package (well, that's the goal of the ubuntu-application template after all), you should also release an upstream tarball, signing it, pushing those, create a milestone, attaching them and also provide a list of what's NEW for the release note. Seems a long task, no? Well, good news is that <code>quickly release</code> does all of this for you! for completing and releasing the changelog as a release note, it takes relevant message from <code>quickly save "your message"</code> or manual bzr commit to create a changelog with all those information. Next step in 0.6 will be to make a Launchpad announce the new release automatically, but this requires again extending the API, so, some Launchpad hacking<sup>[<a href="#pnote-174-4">4</a>]</sup>.<p>


<p>You can have a look <a href="https://launchpad.net/fooby" hreflang="en">here</a> for a dummy example (see "Series and milestones").</p>


<h4>Adding dependencies</h4>


<p><a href="https://launchpad.net/python-distutils-extra" hreflang="en">python-distutils-extra</a> rocks at detecting with a lot of magic your python dependencies. Unfortunately, it can't detect the dependencies for non python stuff and manually launched commands. It's now possible to provide a manual list of dependencies for your app. For this, <code>quickly configure dependencies</code> is your new sweet friend.</p>


<h4>Silent create/package/share/release commands</h4>


<p>All those command now will show some dots about the progress (--verbose for full log), but filter annoying output to only focus on what matters. If some warnings and errors are encounters, it will print them in a summary asking you if you wish to continue</p>


<blockquote><p>$ quickly release</p>
<p>
Récupération de la configuration Launchpad</p>
<p>
Connexion à Launchpad réussie</p>
<p>
......................</p>
<p>
<del></del><del></del><del></del><del></del><del></del><del></del><del></del><del></del>--</p>
<p>
Command returned some ERRORS:</p>
<p>
<del></del><del></del><del></del><del></del><del></del><del></del><del></del><del></del>--</p>
<p>
ERROR: Python module slip_coverconfig not found</p>
<p>
<del></del><del></del><del></del><del></del><del></del><del></del><del></del><del></del>--</p>
<p>
Command returned some WARNINGS:</p>
<p>
<del></del><del></del><del></del><del></del><del></del><del></del><del></del><del></del>--</p>
<p>
WARNING: the following files are not recognized by DistUtilsExtra.auto:</p>
<p>
bin/slip-coverc</p>
<p>
<del></del><del></del><del></del><del></del><del></del><del></del><del></del><del></del>--</p>
<p>
Do you want to continue (this is not safe!)? y/n: y</p>
<p>
Paquet Ubuntu créé dans le répertoire debian/</p>
<p>
......</p>
<p></p></blockquote>


<p>This is to avoid the whole bunch of information that can lost people in focusing on the important ones.</p>


<h4>Apport and Launchpad Integration</h4>


<p>Now, all projects will contain an apport hook and a launchpad integration in the menu. Of course, they only appears if you have the current package integrated. So, the project will still work on other distros, just not showing the integration.</p>


<p>Currently this only works for installed package (so, you can't run that for trunk). Existing project are updated as well to get this goodness. Some more work are waiting approval to get it work from trunk too (liblaunchpad waiting merges)
<img src="/public/projects/quickly/launchpad_integration.png" alt="Launchpad Integration" style="display:block;margin:0 auto" title="Launchpad Integration, avr. 2010"></p>


<h4>New debug command</h4>


<p>Just run <code>quickly debug</code> for some sweetness in the debug world with winpdb!</p>


<h4>Misc</h4>

<ul>
<li>Some commands have been renamed, to avoid cluttering you with multiple commands and being more descriptive:</li>
<li><code>quickly glade</code> is now <code>quickly design</code></li>
<li><code>quickly lp-project-change</code> is now <code>quickly configure lp-project</code> (configure is used for bzr, dependencies, lp-project, ppa)</li>
<li><code>quickly dialog</code> is now <code>quickly add dialog</code> to enable later boilerplate addition.</li>
<li>The tutorial is now in docbook format and ready for translation</li>
<li>The boiler plate is now fully i18n compliant</li>
</ul>

<h3>ubuntu-cli template</h3>


<p>So, with the importing tempate feature, here is a new template resulting from some requests about building command line only application. Again, there is <strong>0</strong> line of code, just importing commands from ubuntu-application template!</p>


<p>after running <code>quickly create ubuntu-cli "my CLI app"</code>, I got a new fresh my-cli-app program showing:
quickly run foo</p>


<p>I'm launched and my args are: foo</p>


<p>You also have a full of default binded option like:</p>

<blockquote><p>$ quickly run <del> </del>help</p>
<p>
Usage: my-cli-app <a href="/post/options" title="options">options</a></p>
<p></p>
<p></p>
<p>
Options:</p>
<p>
--version          show program's version number and exit</p>
<p>
-h, --help         show this help message and exit</p>
<p>
-d, --debug        Print the maximum debugging info (implies -vv)</p>
<p>
-v, --verbose      set error_level output to warning, info, and then debug</p>
<p>
-f FOO, --foo=FOO  foo should be assigned to bar@@</p></blockquote>


<p>those are good examples to see how to handle debugging level and random command line options (with eventually, parameters, counting occurrences of a parameter, …).</p>


<p>Of course, only a subset of ubuntu-application commands are available in this template (no <code>quickly add dialog</code>, neither <code>quickly design</code>, for instance ;)). Hope that will bring fun for making CLI application too!</p>


<h3>ubuntu-pygame template</h3>


<p>Who said developers don't have fun? Rick provided a new template for developing games:</p>


<p><img src="/public/projects/quickly/.Capture-pygame_window_m.jpg" alt="ubuntu pygame template" style="display:block;margin:0 auto" title="ubuntu pygame template, avr. 2010"></p>


<p>Like ubuntu-cli, ubuntu-pygame shares a large part of ubuntu-application commands and is just a new boilerplate (no command written, again). You should check out the full and complete tutorial available at <code>quickly tutorial</code></p>


<h2>What's next?</h2>


<p>Well, that was only a subpart of what was backed out in Quickly 0.4. We have tons of fixes and small enhancements that would have deserve to be put there. This post is already quite long so, I won't continue on. Check out the <a href="https://edge.launchpad.net/quickly/0.x/0.4" hreflang="en">NEWS</a> file if you want the full story!</p>


<p>Now, let's see what will be in 0.6. We will discuss the new features at next <a href="https://wiki.ubuntu.com/UDS-M" hreflang="en">UDS</a>, hope you can join either in person or by IRC. One of the next features will be nautilus integration thanks to the API. Some prototypes are already available as you can see them below. Launching a dedicated gedit session with some plugin enabled is also on track.</p>


<p><a href="/public/projects/quickly/quickly-nautilus1.png" title="Nautilus outside any project"><img src="/public/projects/quickly/.quickly-nautilus1_m.jpg" alt="Nautilus outside any project" style="display:block;margin:0 auto" title="Nautilus outside any project, avr. 2010"></a>
<a href="/public/projects/quickly/quickly-nautilus2.png" title="nautilus inside an ubuntu-application project"><img src="/public/projects/quickly/.quickly-nautilus2_m.jpg" alt="nautilus inside an ubuntu-application project" style="display:block;margin:0 auto" title="nautilus inside an ubuntu-application project, avr. 2010"></a></p>


<p>Let's keep it hot and don't hesitate to comment on the new features. Also, what do you think should be added to 0.6 (maybe renamed to Quickly 10.10)?</p>


<p>We have already a good bunch of documentation for Quickly, even if some like my <a href="/post/Build-your-application-quickly-with-Quickly%3A-part1" hreflang="en">blog posts</a> should be refreshed for 0.4, the <a href="https://wiki.ubuntu.com/Quickly" hreflang="en">wiki page</a> is still a good starting point, and I heard that there will be soon some new videos available…</p>


<p>Hope you will enjoy 0.4 and we always welcome your feedback on #quickly on freenode!</p>
<div><h4>Notes</h4>
<p>[<a href="#rev-pnote-174-1">1</a>] so, you will understand that once running 0.4, you won't be able to switch back to 0.2.x due to this transition (a lot of things will happen the first time you will either launch a command or use shell completion in your existing project. The good news is that, you don't have even to care about this ;)<p>
<p>[<a href="#rev-pnote-174-2">2</a>] it'll create one for you if you don't have any. However, it won't still upload it to Launchpad as it raises some security concerns by the launchpad guys. We will try to tackle those issues at <a href="https://wiki.ubuntu.com/UDS-M" hreflang="en">Maverick UDS</a> with them to have clearly nothing to do for 0.6.<p>
<p>[<a href="#rev-pnote-174-3">3</a>] no more LICENCE one<p>
<p>[<a href="#rev-pnote-174-4">4</a>] should be easier than the gpg/ssh stuff though</p><div>