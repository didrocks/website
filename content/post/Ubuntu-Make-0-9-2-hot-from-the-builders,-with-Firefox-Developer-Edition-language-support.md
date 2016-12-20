+++
title = "Ubuntu Make 0.9.2 hot from the builders, with Firefox Developer Edition language support"
date = "2015-08-04T11:23:00+02:00"
tags = [ "devel", "libre", "PU", "ubuntu", "ubuntu-make", "ubuntulovesdevs" ]
aliases = ["/post/Ubuntu-Make-0.9.2-hot-from-the-builders%2C-with-Firefox-Developer-Edition-language-support"]
+++
    <p>Ubuntu Make 0.9.2 has just been released and features language support in our Firefox Developer Edition installation!</p>


<p>Thanks to our new awesome community contributor <a href="https://github.com/oijazsh">Omer Sheikh</a>, <a href="https://wiki.ubuntu.com/ubuntu-make">Ubuntu Make</a> now enables developers to install <a href="https://www.mozilla.org/firefox/developer/">Firefox Developer Edition</a> in their language of choice! This is all backed with our mandatory medium and large extensive testsuites. Big thanks to him for getting that through!</p>


<p>The installation process will ask you (listing all available languages) what is your preference for that framework:</p>



<pre> $ umake web firefox-dev
 Choose installation path: /home/didrocks/tools/web/firefox-dev
 Choose language: (default: en-US)
 <a href="/post/ach/af/sq/ar/an/hy-AM/as/ast/az/eu/be/bn-BD/bn-IN/brx/bs/br/bg/my/ca/zh-CN/zh-TW/hr/cs/da/nl/en-GB/en-ZA/en-US/eo/et/fi/fr/fy-NL/ff/gd/gl/de/el/gu-IN/he/hi-IN/hu/is/id/ga-IE/it/ja/kn/ks/kk/km/kok/ko/lv/lij/lt/dsb/mk/mai/ms/ml/mr/nb-NO/nn-NO/oc/or/fa/pl/pt-BR/pt-PT/pa-IN/ro/rm/ru/sat/sr/si/sk/sl/son/es-AR/es-CL/es-MX/es-ES/sv-SE/ta/te/th/tr/uk/hsb/uz/vi/cy/xh" title="ach/af/sq/ar/an/hy-AM/as/ast/az/eu/be/bn-BD/bn-IN/brx/bs/br/bg/my/ca/zh-CN/zh-TW/hr/cs/da/nl/en-GB/en-ZA/en-US/eo/et/fi/fr/fy-NL/ff/gd/gl/de/el/gu-IN/he/hi-IN/hu/is/id/ga-IE/it/ja/kn/ks/kk/km/kok/ko/lv/lij/lt/dsb/mk/mai/ms/ml/mr/nb-NO/nn-NO/oc/or/fa/pl/pt-BR/pt-PT/pa-IN/ro/rm/ru/sat/sr/si/sk/sl/son/es-AR/es-CL/es-MX/es-ES/sv-SE/ta/te/th/tr/uk/hsb/uz/vi/cy/xh">ach/af/sq/ar/an/hy-AM/as/ast/az/eu/...</a> fr
 Downloading and installing requirements
 100% |#########################################################################|
 Installing Firefox Dev
 |##############################################################################|
 Installation done</pre>


<p>And here we go, with Firefox Dev Edition installed in french:</p>


<p><a href="/public/ubuntu/uld/FirefoxDevFr.png" title="Firefox Developer Edition en français svp!"><img src="/public/ubuntu/uld/.FirefoxDevFr_m.jpg" alt="Firefox Developer Edition en français svp!" style="display:block; margin:0 auto;" title="Firefox Developer Edition en français svp!, août 2015" /></a></p>


<p>You can as well use the new <strong>--lang=</strong> option to do that in non interactive mode, like scripts.</p>


<p><a href="https://github.com/bpsizemore">Brian P. Sizemore</a> joined as well the Ubuntu Make contributor crew with this release with some clarification of our readme page. Valuable contribution to all newcomers, thanks to him as well!</p>


<p>Some general fixes as well were delivered into this new release, full list is available in the <a href="https://github.com/ubuntu/ubuntu-make/commit/de3346985f6ad48a4d54052599af7a3f3c511840">changelog</a>.</p>


<p>As usual, you can get this latest version direcly in Ubuntu Wily, and through <a href="https://launchpad.net/~ubuntu-desktop/+archive/ubuntu/ubuntu-make">its ppa</a> for the 14.04 LTS, 15.05 ubuntu releases.</p>


<p>Our <a href="https://github.com/ubuntu/ubuntu-make/issues">issue tracker</a> is full of ideas and opportunities, and pull requests remain opened for any issues or suggestions! If you want to be the next featured contributor and want to give an hand, you can refer to <a href="/post/How-to-help-on-Ubuntu-Developer-Tools-Center">this post</a> with useful links!</p>