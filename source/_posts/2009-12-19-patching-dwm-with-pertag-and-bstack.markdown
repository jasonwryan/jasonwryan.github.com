---
layout: post
title: "Patching dwm with pertag and bstack"
date: 2009-12-19 17:40
comments: true
categories: [dwm, archlinux]
---
Over the last several months I have been using [dwm](http://dwm.suckless.org/) 
as my window manager on two [Arch Linux](http://www.archlinux.org/)
machines:  my <a href="http://www.flickr.com/photos/jasonwryan/4196530478/" title="Flickr image"
target="_blank">EeePC 901</a> and an <a href="http://www.flickr.com/photos/jasonwryan/4193384965/" title="Flickr image"
target="_blank">old Dell</a> at work.

There is a lot to like about dwm, but the most attractive qualities for me
are the fact that it is [under 2000 SLOC](http://en.wikipedia.org/wiki/Source_lines_of_code) 
and is very configurable.

The most recent version, 5.7.2, included some changes that meant that some
of the patches wouldn't play nicely together anymore; trying to combine
any of the layout patches with [pertag](http://dwm.suckless.org/patches/pertag)
would result in the code failing to compile.

Fortunately, there is a fix described on [the mailing list](http://lists.suckless.org/dev/0911/2317.html).
Essentially, it involves manually applying the patch to the source code.

Start with a vanilla version of the code. Move to the source directory and
apply the pertag patch:

{% codeblock lang:sh %}
cd $BUILD/dwm/src/dwm-5.7.2/ patch -p1 < dwm-5.7.2-pertag.diff
{% endcodeblock %}

Then manually add the [bstack](http://dwm.suckless.org/patches/bottom_stack) 
(or any of the other layout patches) to <span class="file">dwm.c</span>:

Include the `bstack` declaring function with the others, beginning line 153. I
inserted it alphabetically…

{% codeblock lang:c %}
static void attachstack(Client *c); 
static void bstack(Monitor *m);
static void buttonpress(XEvent *e);
…
{% endcodeblock %}

And then, *after* the `#include “config.h”` declaration at
275, add in the rest of the patch. For consistency's sake, I included it
at line 415:

{% codeblock lang:c %}
void bstack(Monitor *m) { 
  int x, y, h, w, mh; 
  unsigned int i, n;
  Client *c; for(n = 0, c = nexttiled(m-&gt;clients); c; c = nexttiled(c-&gt;next), n++); 
  if(n == 0) 
return;
…
{% endcodeblock %}

Add you keybindings into your header file (<span class="file">config.h</span>) and recompile:

{% codeblock lang:sh %}
makepkg -efi
{% endcodeblock %}

Done.

The only fly in the ointment is that, for me anyway, I have been unable to
get any of the other layout patches to work with this setup; either by regular
patching or using the above method. YMMV.

### Update
So I was wrong: you can get pertag to work with all the
other patches, and it is [not that difficult](http://jasonwryan.com/2010-03-02-patching-dwm)…

