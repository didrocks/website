+++
title = "How to help on Ubuntu Developer Tools Center"
date = "2014-09-10T16:15:00+02:00"
tags = [ "devel", "libre", "programmation", "PU", "ubuntu", "ubuntulovesdevs" ]
aliases = ["/post/How-to-help-on-Ubuntu-Developer-Tools-Center"]
+++
    <p>Last week, we announced our "<a href="/post/Ubuntu-loves-Developers">Ubuntu Loves Developers</a>" effort! We got some great feedback and coverage. Multiple questions arose around how to help and be part of this effort. Here is the post to answer about this :)</p>


<h3>Our philosophy</h3>


<p>First, let's define the core principles around the <a href="https://github.com/didrocks/ubuntu-developer-tools-center">Ubuntu Developer Tools Center</a> and what we are trying to achieve with this:</p>
<ol>
<li>UDTC will always download, tests and support the latest available upstream developer stack. No version stuck in stone for 5 years, we get the latest and the best release that upstream delivers to all of us. We are conscious that being able to develop on a freshly updated environment is one of the core values of the developer audience and that's why we want to deliver that experience.</li>
<li>We know that developers want stability overall and not have to upgrade or spend time maintaining their machine every 6 months. We agree they shouldn't have to and the platform should "get out of my way, I've got work to do". That's the reason why we focus heavily on the latest LTS release of Ubuntu. All tools will always be backported and supported on the latest Long Term Support release. Tests are running multiple times a day on this platform. In addition to this, we support, of course, the latest available Ubuntu Release for developers who likes to live on the edge!</li>
<li>We want to ensure that the supported developer environment is always functional. Indeed, by always downloading latest version from upstream, the software stack can change its requirements, requiring newer or extra libraries and thus break. That's why we are running a whole suite of functional tests multiple times a day, on both version that you can find in distro and latest trunk. That way we know if:</li>
</ol>
<ul>
<li>we broke ourself in trunk and needs to fix it before releasing.</li>
<li>the platform broke one of the developer stack and we can promptly fix it.</li>
<li>a third-party application or a website changed and broke the integration. We can then fix this really early on.</li>
</ul>

<p>All those tests running will ensure the best experience we can deliver, while fetching always latest released version from upstream, and all this, on a very stable platform!</p>


<h3>Sounds cool, how can I help?</h3>


<h4>Reports bugs and propose enhancements</h4>


<p>The more direct way of reporting a bug or giving any suggestions is through the <a href="https://github.com/didrocks/ubuntu-developer-tools-center/issues">upstream bug tracker</a>. Of course, you can always reach us out as well on <a href="https://plus.google.com/+DidierRoche">social networks like g+</a>, through the comments section of this blog, or on IRC: #ubuntu-desktop, on freenode. We are also starting to look at the <strong>#ubuntulovesdevs</strong> hashtag.</p>


<p>The tool is really to help developers, so do not hesitate to help us directing the Ubuntu Developer Tools Center on the way which is the best for you. :)</p>


<h4>Help translating</h4>


<p>We already had some <a href="https://translations.launchpad.net/ubuntu-developer-tools-center">good translations contributions</a> through launchpad! Thanks to all our translators, we got Basque, Chinese (Hong Kong), Chinese (Simplified), French, Italian and Spanish! There are only few strings up for translations in udtc and it should take less than half an hour in total to add a new one. It's a very good and useful way to contribute for people speaking other languages than English! We do look at them and merge them in the mainline automatically.</p>


<h4>Contribute on the code itself</h4>


<p>Some people started to offer code contribution and that's a very good and motivating news. Do not hesitate to <a href="https://github.com/didrocks/ubuntu-developer-tools-center">fork us on the upstream github repo</a>. We'll ensure we keep up to date on all code contributions and pull requests. If you have any questions or for better coordination, <a href="https://github.com/didrocks/ubuntu-developer-tools-center/issues/new">open a bug</a> to start the discussion around your awesome idea. We'll try to be around and guide you on how to add any framework support! You will <a href="https://github.com/didrocks/ubuntu-developer-tools-center/issues/5">not be alone</a>!</p>


<h4>Write some documentation</h4>


<p>We have some <a href="https://wiki.ubuntu.com/ubuntu-developer-tools-center">basic user documentation</a>. If you feel there are any gaps or any missing news, feel free to edit the wiki page! You can as well merge some of the documentation of the <a href="https://github.com/didrocks/ubuntu-developer-tools-center/blob/master/README.md">README.md</a> file or propose some enhancements to it!</p>


<p>To give an easy start to any developers who wants to hack on udtc iitself, we try to keep the <a href="https://github.com/didrocks/ubuntu-developer-tools-center/blob/master/README.md">README.md</a> file readable and up to the current code content. However, this one can deviate a little bit, if you think that any part missing/explanation requires, you can propose any modifications to it to help future hackers having an easier start. :)</p>


<h4>Spread the word!</h4>


<p>Finally, spreading the word that <a href="#ubuntulovesdevs</strong> or in blog posts, or just chatting to your local community! We deeply care about our developer audience on the Ubuntu Desktop and Server and we want this to be known!<p>


<p><img src="/public/ubuntu/uld/uld.png" alt="uld.png" style="display:block; margin:0 auto;" title="ULD" /></p>


<p>For more information and hopefully goodness, we'll have an <a href="http://ubuntuonair.com/">ubuntu on air session</a> session soon! We'll keep you posted on this blog when we have final dates details.</p>


<p>If you felt that I forgot to mention anything, do not hesitate to signal it as well, this is another form of very welcome contributions! ;)</p>


<p>I'll discuss next week how we maintain and runs tests to ensure your developer tools are always working and supported!</p>