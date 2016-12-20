+++
title = "Unity Radios lens for quantal"
date = "2012-07-24T19:15:00+02:00"
tags = [ "devel", "libre", "malife", "programmation", "PU", "ubuntu", "unity" ]
aliases = ["/post/Unity-Radios-lens-for-quantal"]
+++
    <p>After having worked on the <a href="/post/Added-rhythmbox-radio-support-the-unity-music-lens">local radio scope for the music lens</a>, I spent some time with pure python3 code, which made me experiencing a bug in Dee with pygi and python3 (now <a href="https://launchpad.net/ubuntu/+source/dee/1.0.10-0ubuntu2">all fixed</a> in both precise and quantal)</p>


<p>In addition to that, it was the good timing to experiment more seriously some mocking tool for testing the online part of the lens, and so I played with <a href="http://www.voidspace.org.uk/python/mock/">python3-mock</a>, which is a really awesome library dedicated to that purpose<sup>[<a href="#pnote-175-1" id="rev-pnote-175-1">1</a>]</sup>.<p>


<p>So here was my playground: an <a href="https://launchpad.net/unity-lens-radios">Unity dedicated radio lens</a>! You can search through thousands of online available radios, ordered by categories with some contextual informations based on your current language and your location (Recommended, Top, Near you).</p>


<p><a href="/public/projects/divers/unity_lens_radios_full_view.png" title="Unity lens Radios full view"><img src="/public/projects/divers/.unity_lens_radios_full_view_m.jpg" alt="Unity lens Radios full view" style="display:block; margin:0 auto;" title="Unity lens Radios full view, juil. 2012" /></a></p>


<p>As with most of lenses you can refine any search results with filters. The supported ones are countries, decades and genres. The current track and radio logos are displayed if supported and double clicking any entry should start playing the radio in rhythmbox.</p>


<p><a href="/public/projects/divers/unity_lens_radios_search_and_filter.png" title="Unity lens Radios search and filter"><img src="/public/projects/divers/.unity_lens_radios_search_and_filter_m.jpg" alt="Unity lens Radios search and filter" style="display:block; margin:0 auto;" title="Unity lens Radios search and filter, juil. 2012" /></a></p>


<p>Was a fun experiment and it's now <a href="https://launchpad.net/ubuntu/+source/unity-lens-radios">available on quantal</a> for your pleasure ;)</p>


<p>Oh btw:</p>


<p>didrocks@tidus:~/work/unity/unity-lens-radios/ubuntu$ nosetests3
..................................................</p>

<hr />

<p>Ran 50 tests in 0.164s</p>


<p>OK</p>
<div class="footnotes"><h4>Note</h4>
<p>[<a href="#rev-pnote-175-1" id="pnote-175-1">1</a>] loving the patch decorator!</p><div>
