---
layout: post
title: "Install .otf Fonts in OpenOffice"
alias: /post/87190732/install-otf-fonts-in-openoffice
date: 2009-03-17 19:51
comments: true
categories: [linux,unix,hack]
---

OpenOffice won’t recognize TrueType fonts with the extension `.otf`, so you need to convert them to `.ttf`
in order to use them in that suite.

{% codeblock lang:sh %}
sudo apt-get install fontforge
{% endcodeblock %}

If all the `.otf` files are in the same folder, then you are good to go.

First, you need the conversion script, which I found in [this post](url=http://www.stuermer.ch/blog/convert-otf-to-ttf-font-on-ubuntu.html]this post[/url):

{% codeblock otf2ttf.sh %}
#!/usr/local/bin/fontforge
# Quick and dirty hack: converts a font to truetype (.ttf)
Print("Opening "+$1);
Open($1);
Print("Saving "+$1:r+".ttf");
Generate($1:r+".ttf");
Quit(0);
{% endcodeblock %}

Save it as `otf2ttf.sh` into the folder with the `.otf` files.

Then run:

{% codeblock lang:sh %}
for i in *.otf; do fontforge -script otf2ttf.sh $i; done
{% endcodeblock %}

Move all the converted fonts to your your shared directory:

{% codeblock lang:sh %}
sudo mv *ttf /usr/share/fonts/truetype/myfonts/
{% endcodeblock %}

And then change the permissions and reload the cache:

{% codeblock lang:sh %}
sudo chmod go+rx /usr/share/fonts/truetype/myfonts/
sudo chmod go+rx /usr/share/fonts/truetype/myfonts/*.ttf
sudo fc-cache -f -v
{% endcodeblock %}

And the fonts will now appear in OpenOffice. Easy.
