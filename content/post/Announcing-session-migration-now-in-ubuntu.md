+++
title = "Announcing session-migration now in ubuntu"
date = "2012-07-16T08:35:00+02:00"
tags = [ "libre", "PU", "ubuntu", "uds" ]
aliases = ["/post/Announcing-session-migration-now-in-ubuntu"]
+++
    <p>Just fresh hot of the press, <a href="https://launchpad.net/session-migration">session-migration</a> is now available in quantal.</p>


<p><img src="/public/projects/divers/gnome-session-logout.png" alt="Session icon" style="display:block; margin:0 auto;" title="Session icon, juil. 2012" /></p>


<p>This small tool is trying to solve a problem we encountered for a long time as a distributor, but had to postpone it way too long because of other priorities. :)
It basically enables packagers and maintainers to migrate in user session data. Indeed, when you upgrade a package, the packaging tools are running under root permissions, and only hackish solutions was used in the past to enable us to change some parts of your user configuration<sup>[<a href="#pnote-173-1" id="rev-pnote-173-1">1</a>]</sup>, like adding the FUSA applet, adding new compiz plugins on the fly… There are tons of example when a distribution needs to migrate some user data (logged or not when the upgrade is proceeding) without patching heavily the upstream project to add a migration support there.<p>


<p><a href="/public/projects/divers/fusa.png" title="Fusa in 2008"><img src="/public/projects/divers/.fusa_m.jpg" alt="Fusa in 2008" style="display:block; margin:0 auto;" title="Fusa in 2008, juil. 2012" /></a></p>


<p>This tool is executed at session startup, in a sync fashion. It contains caching and tries to execute the minimal chunk<sup>[<a href="#pnote-173-2" id="rev-pnote-173-2">2</a>]</sup>, based on the design of the excellent <a href="http://git.gnome.org/browse/gconf/tree/gsettings/gsettings-data-convert.c">gconf-&gt;gsettings migration</a> tool. It contains as well a dh-migrations package, with a debhelper hook (<code>--with migrations</code> calling <code>dh_migrations</code>), so that client desktop packages just have to ship a <code>debian/&lt;package&gt;.migrations</code> file linking to their migration scripts which will be shipped in the right directory. We can even imagine in the near future that when you install such a package, you end up with a notification that a session restart is necessary (and not a full reboot). Note as well that the migration happens per user and per session, so it's really important that the scripts are idempotents.<p>


<p>Was 3 days of fun coding this. ;) All of the C and perl codes are covered by a short, but complete testsuite run during the package build, so no breakage hopefully. ;) Associated man pages are <a href="man:session-migration">session-migration</a> and <a href="man:dh_migrations">dh_migrations</a>.</p>


<p>You can find more details on the <a href="https://blueprints.launchpad.net/ubuntu/+spec/desktop-q-upgrade-user-config">launchpad specification</a> as well as the recorded streamed discussion from UDS (<a href="http://mirrors.tumbleweed.org.za/uds-q/2012-05-07-17-55-desktop-q-upgrade-user-config.0.ogg">part 1</a>, <a href="http://mirrors.tumbleweed.org.za/uds-q/2012-05-07-17-55-desktop-q-upgrade-user-config.1.ogg">part 2</a>).</p>



<p><q>The illustrated session icon is under a GPL licence, found on <a href="http://www.iconfinder.com/search/?q=iconset%3AUltimateGnome">iconfinder</a>.</q></p>
<div class="footnotes"><h4>Notes</h4>
<p>[<a href="#rev-pnote-173-1" id="pnote-173-1">1</a>] Knowing that we try to respect as more as possible the gold rule and always trying to only change default, which is not possible sometimes<p>
<p>[<a href="#rev-pnote-173-2" id="pnote-173-2">2</a>] but support multiples layers based on XDG_DATA_DIRS</p><div>
