+++
title = "NotifyThis 0.1: notify everything easily!"
date = "2009-10-08T18:20:00+02:00"
aliases = ["/post/NotifyThis-0.1%3A-notify-everything-easily%21"]
+++
    <p>In our French <a href="http://www.ubuntu-party.org" hreflang="fr">ubuntu party</a> organized by <a href="http://www.ubuntu-fr.org/" hreflang="fr">ubuntu-fr</a>, we have approximately 20 free and ready to use computers for people to be able to try ubuntu on their own (we have someone around to help new users).</p>


<p>But that's a lot of wasted space for advertising our conferences, scheduled demonstrations, lessons we have during the party! So, the idea was to create a daemon notifying this, resulting in:
<img src="/public/projects/notifythis/example.png" alt="example.png" style="display:block;margin:0 auto" title="example.png, oct. 2009"></p>


<p><img src="/public/projects/notifythis/notifythis192.png" alt="notifythis192.png" style="float:right;margin:0 0 1em 1em" title="notifythis192.png, oct. 2009">
<a href="http://launchpad.net/notifythis" hreflang="en">NotifyThis</a> is here to fulfill this need. It's a desktop daemon (no pid file) using dbus for single instance and actions like pausing, stopping, forcing configuration file reload<sup>[<a href="#pnote-138-1">1</a>]</sup>…<p>


<p>Source file is in XML (default in /etc/notifythis-data.xml):</p>

<pre> &lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
 &lt;notifythis xmlns='http://launchpad.net/notifythis' xml:lang='fr'&gt;
   &lt;notiftypes&gt;
       &lt;type&gt;
           &lt;name&gt;lesson&lt;/name&gt;
           &lt;priority&gt;low&lt;/priority&gt;
           &lt;icon&gt;/usr/share/icons/Humanity/apps/22/gnome-terminal.svg&lt;/icon&gt;
       &lt;/type&gt;
       &lt;type&gt;
           &lt;name&gt;conference&lt;/name&gt;
           &lt;priority&gt;low&lt;/priority&gt;
           &lt;icon&gt;/usr/share/icons/Humanity/stock/22/stock_people.svg&lt;/icon&gt;
       &lt;/type&gt;
       &lt;type&gt;
           &lt;name&gt;demos&lt;/name&gt;
           &lt;priority&gt;low&lt;/priority&gt;
           &lt;icon&gt;/usr/share/icons/Humanity/places/22/user-desktop.svg&lt;/icon&gt;
       &lt;/type&gt;
       &lt;type&gt;
           &lt;name&gt;miscellaneous&lt;/name&gt;
           &lt;priority&gt;low&lt;/priority&gt;
           &lt;icon&gt;http://awesomeurl/dialog-warning.svg&lt;/icon&gt;
       &lt;/type&gt;
   &lt;/notiftypes&gt;
   &lt;notifevents&gt;
       &lt;event&gt;
           &lt;title&gt;Beginners lesson&lt;/title&gt;
           &lt;content&gt;Lesson for beginners in 10 min&lt;/content&gt;
           &lt;type&gt;lesson&lt;/type&gt;
           &lt;time&gt;2009-09-27 12:33&lt;/time&gt;
       &lt;/event&gt;
       &lt;event&gt;
           &lt;title&gt;Advanced lesson&lt;/title&gt;
           &lt;content&gt;Advanced lesson starting in 15 min&lt;/content&gt;
           &lt;type&gt;lesson&lt;/type&gt;
           &lt;icon&gt;http://unexistingurl/stock_dialog-warning.svg&lt;/icon&gt;
           &lt;time&gt;2009-09-26 22:01&lt;/time&gt;
       &lt;/event&gt;    
       &lt;event&gt;
           &lt;title&gt;Awesome conference&lt;/title&gt;
           &lt;content&gt;An awesome conference on Ubuntu will start in 5 min.\nKeep it hot!&lt;/content&gt;
           &lt;type&gt;conference&lt;/type&gt;
           &lt;time&gt;2009-09-27 23:54&lt;/time&gt;
       &lt;/event&gt;
       &lt;event&gt;
           &lt;title&gt;End of the event&lt;/title&gt;
           &lt;content&gt;Well… I guess you will have to leave NOW!&lt;/content&gt;
           &lt;type&gt;miscellaneous&lt;/type&gt;
           &lt;icon&gt;/usr/share/icons/Humanity/status/22/stock_dialog-warning.svg&lt;/icon&gt;
           &lt;time&gt;2009-09-27 23:55&lt;/time&gt;
       &lt;/event&gt;
   &lt;/notifevents&gt;
 &lt;/notifythis&gt;</pre>


<p>What can I do in this <strong>notifythis</strong> namespace?</p>

<pre></pre>

<p>You associate an event instance (in <strong>notifevents/event</strong> ) to a <strong>notifytypes/type/name</strong> declared in the file. Types enables you to set some default priorities and icons to events. &lt;Icon&gt; refers to a path to an icon, which can be local to your computer or distant served by your preferred web server (http://…). In the latter case, NT will cache it for you locally. Consequently, using multiple times the same icon in different types or events will download it only once.</p>


<p>Title is used as… notification title and content as… content :) &lt;time&gt; is used to specify when the event will be notified. Categorizing your event is just a way to avoid multiple icon/priority definitions, but those two values can be overridden for a particular event simply specifying corresponding tag (&lt;icon&gt; for instance). If it can't be found, event type icon will be shown. If this one doesn't exist/can't be reachable either, a warning is spawn in log file and no icon will be displayed.</p>


<p>So, you can for your event, but it can be bound with any kind of other things like calendar, TODO items, and so on…</p>


<p>But well, as unscheduled event changes may happen<sup>[<a href="#pnote-138-2">2</a>]</sup>, you surely want to upload your xml file online and reload it in all NotifyThis daemon dynamically. It's possible too (and what we will do during our party).<p>


<p>In <strong>/etc/notifythis</strong>, you can set where the xml file is:</p>

<pre> # time in minutes beteen two xml file reloading, default is 30 minutes
 DELTA_BETWEEN_XML_RELOAD=30
 
 # xml files (separated by ';') by order of preferences.
 # It can an absolute or a relative path to this file.
 # It can be a network path (http://) too
 XML_FILES=notifythis-data.xml</pre>


<p>You can even specify multiple XML_FILES and first found one will be use (like in XML_FILES=http://my/central/xml/file.xml; /usr/local/mybackupfile1.xml;notifythis-data.xml). XML file will be reloaded every $DELTA_BETWEEN_XML_RELOAD min (30 min if not specified) in each daemon. If no xml file is found, previous working configuration is preserved and a retry is done every minutes then.</p>


<p>By default, NotifyThis is a desktop daemon<sup>[<a href="#pnote-138-3">3</a>]</sup> launched with a classical .desktop file in /etc/xdg/autostart/. Log are in ~/.local/share/notifythis/notifythis.log<p>


<p>Well, NT uses <a href="https://www.launchpad.net/quickly" hreflang="en">Quickly</a> to simplify project handling. I guess we should investigate in a desktop-daemon template,  making properly the double forking, using dbus instead of a pid file and dbus communication to reload configuration file, stop, pause… this can be handy! We can perhaps add a way for a template to reference another template to avoid redefining same commands twice and gives an easy way to call them (without having specifying --template ubuntu-project, for instance).</p>


<p>Well, if you want to give it a try, NotifyThis 0.1 is out and is available <a href="https://launchpad.net/~didrocks/+archive/proposed" hreflang="en">in my ppa</a>.</p>
<div><h4>Notes</h4>
<p>[<a href="#rev-pnote-138-1">1</a>] --help is your friend<p>
<p>[<a href="#rev-pnote-138-2">2</a>] and will certainly happen actually<p>
<p>[<a href="#rev-pnote-138-3">3</a>] you can launch it interactively as well, with --no-daemon and set different verbosity levels</p><div>