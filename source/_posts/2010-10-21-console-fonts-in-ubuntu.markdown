---
layout: post
title: "Console Fonts in Ubuntu"
date: 2010-10-21 20:38
comments: true
categories: [ubuntu, linux]
---
![image](/images/post_images/ubuntu-fonts.png)

Invariably, an [Ubuntu](http://www.ubuntu.com/ "Ubuntu homepage")
upgrade only ever brings me pain in one area: the framebuffer. Granted,
most Ubuntu users probably don’t care about the resolution of their
framebuffer because they probably rarely venture there.

For me, however, having the correct resolution and a decent font is
important. Not because I spend a huge amount of time there (I don’t),
but because in the past I have been forced to — and quickly learned that
if the resolution is off, it makes it *very* difficult to work with.

So the recent, otherwise completely successful, upgrade to Maverick
Meerkat once again left me with an almost useless console. Fortunately,
the hack for forcing a 1680x1050 resolution worked<sup>1</sup>, so it was just a
matter of getting
[Terminus](http://fractal.csie.org/~eric/wiki/Terminus_font "Terminus homepage")
to stick as the default console font.

This was one instance where Google failed me. There doesn’t seem to be
any authoritative guide to setting your console font permanently. I
edited <span class="file">/etc/default/console-setup</span> 
and added Terminus as the font, like so:

{% codeblock lang:sh %}
CODESET="Lat15"
FONTFACE="Terminus"
FONTSIZE="16"
{% endcodeblock %}

Issuing, from the console:

{% codeblock lang:sh %}
sudo setupcon -v
{% endcodeblock %}

would change the font to Terminus for that session but a reboot soon
reverted it to the default fixed (which, given 
[Ubuntu have been working on their fonts](https://wiki.ubuntu.com/Ubuntu%20Font%20Family), is
spectacularly ugly).

I finally asked in \#ubuntu and was pointed in the right direction:

{% codeblock lang:sh %}
sudo dpkg-reconfigure console-setup
{% endcodeblock %}

And *now* I have a workable framebuffer.

##### Notes
1. Hack to force higher framebuffer resolution in <span class="file">/etc/default/grub</span>

{% codeblock lang:sh %}
GRUB_GFXPAYLOAD_LINUX=1680x1050
{% endcodeblock %}
