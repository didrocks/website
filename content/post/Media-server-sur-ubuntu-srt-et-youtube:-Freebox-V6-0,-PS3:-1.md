+++
title = "Media server sur ubuntu - srt et youtube: Freebox V6 - 0, PS3: 1"
date = "2011-02-27T16:28:00+01:00"
tags = [ "libre", "malife", "PUF", "ubuntu" ]
aliases = ["/post/Media-server-sur-ubuntu-srt-et-youtube%3A-Freebox-V6-0%2C-PS3%3A-1"]
+++
    <p>Étant l'heureux possesseur d'une freebox V6, et étant comme tout le monde en <a href="http://bugs.freeplayer.org/task/3575">incapacité de lire les fichiers de sous-titre</a> (srt), j'ai décidé ce week-end d'essayer de régler le problème (principalement pour la tranquilité du ménage ;)).</p>


<h2>Lire les fichiers srt</h2>

<p>Résumons, le protocole <a href="http://fr.wikipedia.org/wiki/Universal_Plug_and_Play">UPnP</a>, utilisé entre la freebox server et le freebox player, ne permet pas de lire les fichiers srt (ce n'est tout simplement par suppporté par le protocole).</p>


<p>La solution est donc de se servir d'un serveur<sup>[<a href="#pnote-8-1" id="rev-pnote-8-1">1</a>]</sup> domestique (celui où se trouve ce blog en réalité), afin de réencoder au fur et à mesure la vidéo avec les sous-titres et ne proposer qu'un flux unique contenant la réunion des deux. Pour cela, j'ai utilisé <a href="http://doc.ubuntu-fr.org/mediatomb">mediatomb</a> sur mon serveur lucid, et après avoir <a href="http://doc.ubuntu-fr.org/mediatomb?rev=1296123284&amp;do=diff">corrigé et simplifié</a> le script sur la documentation francophone d'ubuntu, j'ai enfin accès aux vidéos, avec des sous-titres sur la freebox V6! Pour ceux qui veulent en savoir plus, voir les <a href="http://mediatomb.cc/pages/transcoding">avantages et les inconvénients du transcoding</a>.<p>



<h2>Vidéos sur Youtube</h2>

<p>Vu qu'il m'arrive (rarement) de regarder quelques émissions sur Youtube, je me suis dit qu'il serait dommage de s'arrêter en si bon chemin :)</p>


<p>Je me suis donc mis en quête d'envoyer les flux mp4 (H.264) directement au freebox player. La <a href="http://mediatomb.cc/pages/documentation">documentation officielle de mediatomb</a> explique cela assez bien. Il suffit d'ajouter un:</p>


<p><code>&lt;account user="utilisateur" password="mot de passe"/&gt;</code>
au bon endroit du fichier de configuration (<em>/etc/mediatomb/config.xml</em>), puis de changer la section:</p>


<pre>     @@&lt;YouTube enabled="yes" refresh="28800" update-at-start="yes" purge-after="604800" racy-content="exclude" hd="no"&gt;
       &lt;favorites user="utilisateur"/&gt;
       &lt;standardfeed feed="most_viewed" time-range="today"/&gt;
       &lt;playlists user="utilisateur"/&gt;
       &lt;uploads user="utilisateur"/&gt;
       &lt;standardfeed feed="recently_featured" time-range="today"/&gt;
     &lt;/YouTube&gt;@@</pre>

<p>en replaçant bien entendu le nom d'utilissateur et le mot de passe aux bons endroits.</p>


<p>Cependant, cela ne marchait pas (je n'avais pas accès au compte "Online Service") de mediatomb. En lisant la documentation, il est fait état que cette fonctionnalité utilise curl. Pas de problème, un apt-get install curl fixe cela! Cependant, après un redémarrage de mediatomb, je ne vois toujours rien à part les flux récents sur Youtube :/</p>


<p>Quelques recherches montrent rapidement que la version incluse dans la 10.04 (0.12.0~svn2018) de mediatomb, ne supporte plus YouTube. Je trouve alors le ppa de micahg (le mainteneur d'ubuntu) qui a backporté 0.12.1 pour lucid.</p>


<p>Installation et hop redémarrage! Je vois alors toujours les flux récents et mes favoris, mais le Freebox player m'indique que le fux n'est pas valide. Que se passe-t-il?</p>


<p>En regardant les logs, je vois que mediatomb se reçoit (méchamment) une page 404 de Youtube. Après un peu de recherche, il semble que Youtube ait récemment changé quelques adresses. Un <a href="http://sourceforge.net/tracker/index.php?func=detail&amp;aid=3064910&amp;group_id=129766&amp;atid=715780">patch</a> posté sur le bugtracker de mediatomb corrige ceci. Je l'ai intégré à 0.12.1 et envoyé sur <a href="https://launchpad.net/~didrocks/+archive/ppa">mon ppa</a> pour lucid. Après installation, tout se passe bien, je vois donc les vidéos sur Youtube sur la freebox V6. Il me suffit d'ajouter les vidéos en favoris sur mon compte pour y avoir directement accès par upnp sur tous mes lecteurs :)</p>


<p>Mais quel fût ma stupeur en voyant de nombreuses erreurs (tâches vertes, freeze complet, etc.) sur <a href="http://tc.v3.cache5.c.youtube.com/videoplayback?sparams=id,expire,ip,ipbits,itag,algorithm,burst,factor&amp;fexp=903103&amp;algorithm=throttle-factor&amp;itag=18&amp;ipbits=8&amp;burst=40&amp;sver=3&amp;signature=468D5C8FAAC04C975F5CCF4EF7E093DF6170F03B.4C4137E228F9AD9ED30DB74DF12F40C24228C7C1&amp;expire=1298844000&amp;key=yt1&amp;ip=82.0.0.0&amp;factor=1.25&amp;id=704e332f0b699f51&amp;redirect_counter=1">cette vidéo</a> par exemple. Voulant vérifier que le problème venait bien de médiatomb, je me suis rué sur ma playstation 3. Elle cependant, lit cette même vidéo avec le même serveur UPnP mediatomb parfaitement… je peux mettre en pause, accélérer, arrêter.</p>


<p>Bref, encore du travail à faire au niveau de la Freebox V6 et de son player? Il semble bien, aussi bien pour trouver une solution pour ces sous-titres que dans les codecs supportés (même si le H.264 est officiellement supporté). Report de bug pour le freeplayer <a href="http://bugs.freeplayer.org/task/5797">ici</a>.).</p>
<div class="footnotes"><h4>Notes</h4>
<p>[<a href="#rev-pnote-8-1" id="pnote-8-1">1</a>] redondance!</p><div>
