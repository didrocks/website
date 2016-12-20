+++
title = "Versioning schema change in daily release"
date = "2013-06-25T17:10:00+02:00"
tags = [ "libre", "PU", "ubuntu", "unity" ]
aliases = ["/post/Versioning-schema-change-in-daily-release"]
type = "post"
+++
    <p>Just a quick note on <a href="https://wiki.ubuntu.com/DailyRelease">Daily Releases</a> (process to upload to the ubuntu distribution more than 200 components Canonical is upstream for in various ubuntu series).</p>


<p><img src="/public/ubuntu/daily-release/numeros.jpg" alt="crazy numbers" style="display:block; margin:0 auto;" title="crazy numbers, juin 2013" /></p>


<p>After some discussions on #ubuntu-release today, we decided to evoluate the daily release versioning schema:</p>
<ul>
<li>we previously had &lt;upstream_version&gt;daily&lt;yy.mm.dd&gt;-0ubuntu1 as a daily release schema in the regular case</li>
<li>multiple releases a day for the same component would give: &lt;upstream_version&gt;daily&lt;yy.mm.dd&gt;.minor-0ubuntu1, where minor is an incremental digit</li>
<li>for maintenance branch, we previously had &lt;upstream_version&gt;daily&lt;yy.mm.dd&gt;(.minor)~&lt;series-version&gt;-0ubuntu1, where series-version is 13.04, 13.10…</li>
<li>feature branch appends the ppa name to it (after transformation), &lt;upstream_version&gt;daily&lt;yy.mm.dd&gt;.&lt;ppa_name_dotted&gt;</li>
</ul>
<pre></pre>

<p>Mainly due to handling <a href="https://wiki.ubuntu.com/StableReleaseUpdates">SRUs</a> regarding feature branch, multiple releases per day, transitioning from next ppa to the distro, supporting maintenance branch released with the same upstream version, but being a potentially different content than trunk, the version schema has been (slightly) simplified:</p>
<ul>
<li>now, the daily release version will be: &lt;upstream_version&gt;+&lt;series-version&gt;.&lt;yyyymmdd&gt;-0ubuntu1, like: 0.42+13.10.20130625-0ubuntu1 instead of 0.42daily13.06.25-0ubuntu1</li>
<li>for maintenance branches, we have thus now a similar schema: 0.42+13.04.20130625-0ubuntu1 for a daily happening the same day (instead of 0.42daily13.06.25~13.04-0ubuntu1)</li>
<li>the rules for minor versions (multiple releases a day) and feature branches remains the same</li>
</ul>

<p>This still enables the above cases for getting all the concurrencies working with what we deliver to distro as well to various ppas. The upstream merger should still be working and being compatible as well. :)</p>


<p>The <a href="http://bazaar.launchpad.net/~cupstream2distro-maintainers/cupstream2distro/trunk/revision/330">new code</a> is now deployed in production after <a href="http://bazaar.launchpad.net/~cupstream2distro-maintainers/cupstream2distro-config/trunk/revision/461">changing the configuration</a>. In addition to changing the test cases to the new versioning schema, you will notice that there are as well some additional tests to ensure the transition from the old to new world should happen seemlessly :)</p>


<p>All the <a href="https://wiki.ubuntu.com/DailyRelease/FAQ#Versionning_scheme">documentation</a> and <a href="https://wiki.ubuntu.com/DailyRelease/MovingNewRelease#Preparing_the_ppa_configuration_and_setting_the_new_series_version">processes</a> have been updated to the latest as well.</p>


<p>Happy daily releases!</p>
