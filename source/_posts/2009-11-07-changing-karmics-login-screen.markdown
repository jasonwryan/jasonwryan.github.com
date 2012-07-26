---
layout: post
title: "Changing Karmic's Login Screen"
date: 2009-11-07 18:46
comments: true
categories: [ubuntu,linux,hack]
---
With Ubuntu 9.10, Karmic Koala, they have dispensed with the GDM in order to
accelerate boot time. This has made for a faster boot, but a side effect has
been that there is no longer a straightforward way to theme your login - and
the default isn’t just ugly, it’s fugly…

Fear not. It can be fixed.

The first thing you want to do is replace the background image. You can find it
at `/usr/share/images/xsplash`.

Before doing anything rash, make a copy of the directory:

{% codeblock lang:sh %}
sudo cp -r /usr/share/images/xsplash ~/xsplash-backup
{% endcodeblock %}

Then delete all of the images that begin with `bg_`:

{% codeblock lang:sh %}
sudo rm /usr/share/images/xsplash/bg_*
{% endcodeblock %}

Use the gimp to create a background image for your desktop and save it into
this directory, and name it `bg_2560x1600.jpg` …irrespective of the resolution
of your screen. Leave the other images as they are: the logo and the
marvellously named throbber will sit over your new background as you boot up…

Next you can change the colours of the login box to complement your new
background. This is all controlled in
`/usr/share/themes/HumanLogin/gtk-2.0/gtkrc`.

Again, make a copy:

{% codeblock lang:sh %}
sudo cp /usr/share/themes/HumanLogin/gtk-2.0/gtkrc \
/usr/share/themes/HumanLogin/gtk-2.0/gtkrc-backup
{% endcodeblock %}

Then open the file and edit the relevant lines of code at the beginning of the
file:

{% codeblock lang:sh %}
sudo vim /usr/share/themes/HumanLogin/gtk-2.0/gtkrc
{% endcodeblock %}

{% img http://dl.dropbox.com/u/261312/Blog-images/karmic-text.png 'Screenshot of editing text' %}

As you can see, the colour scheme I have chosen uses dark grey to match my
desktop. Feel free to experiment, you can’t really break anything…

<a href="http://www.flickr.com/photos/jasonwryan/4081800113/" title="Customized Karmic Login Screen by jasonwryan, on Flickr"><img src="http://farm3.staticflickr.com/2536/4081800113_d4b98a9b3c.jpg" width="500" height="313" alt="Customized Karmic Login Screen"></a>

Simple, really.

