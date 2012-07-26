---
layout: post
title: "Vimprobable"
date: 2010-10-07 16:23
comments: true
categories: [browser, archlinux]
---
[![image](http://dl.dropbox.com/u/261312/Blog-images/vimprobable.png "Hacked Vimprobable2")](http://www.flickr.com/photos/jasonwryan/5055695051/ "Vimprobable on Flickr")

After just posting details of [my setup](http://jasonwryan.com/blog/2010/10/04/the-setup/), 
I have made a reasonably significant (and it goes almost without saying, unplanned)
change: after [uzbl](http://www.uzbl.org/) began unaccountably maxxing
out my CPU, I decided to give [Vimprobable](http://www.vimprobable.org/)
a go.

I covered it briefly in my review of [5 alternate browsers](http://jasonwryan.com/blog/2010/05/21/5-alternate-browsers/) and liked the
idea of another vim-based browsing experience.

There are two versions of Vimprobable; the differences, as described on
the website:

> Both versions are actively maintained. The first version can only be
> customised by editing config.h before compiling. It is pretty stable
> and very usable. Version 2 has grown fairly stable already as well,
> but it’s still seeing bigger changes, of course. It aims at allowing
> more customisation, for example through :set and :map commands.

I downloaded the tarball for 
[Vimprobable2 from the AUR](http://aur.archlinux.org/packages.php?ID=33779 "Arch User Repository: Vimprobable2"),
extracted the files and built the package.

Customization occurs in 3 files<sup>1</sup>: the first two, 
<span class="file">config.h</span> and <span class="file">keymap.h</span>
both require you to recompile after editing, which is a trivial task
given the size of the programme.

Vimprobable2 also features a third configuration option that doesn’t
require compiling after changes, <span class="file">vimprobablerc</span>. 
Should you wish to use it, you will need to create this file in 
<span class="file">~/.config/vimprobable/</span>.
Helpfully, there is a man page for all of the options in this file, to
read it issue:

{% codeblock lang:sh %}
man vimprobablerc
{% endcodeblock %}

Despite it’s spartan appearance, Vimprobable is a full-featured browser
— with the advantage of being driven solely from the keyboard. It
handles bookmarks (and the tagging thereof, which is impressive and
handy), multiple search engines, cookies, history, etc…

I have made a number of changes to the defaults, including moving some
of the keybindings to be closer to uzbl’s (habit, really). The other
area where I made some minor changes was to the hinting style. Hinting
is just the way Vimprobable (and other browsers like uzbl and
[Conkeror](http://conkeror.org/)) follow links; the default is, to put
it mildly, garish<sup>2</sup>, so I hacked around in <span class="file">hinting.js</span> 
to make it more attractive.

If you are interested, my vimprobable files are in
[bitbucket](https://bitbucket.org/jasonwryan/eeepc/src/tip/Build/vimprobable2-git/),
including the modified hinting.js.

### Update

I think this is a keeper. I installed
[tabbed](http://tools.suckless.org/tabbed "suckless' tabbed") and now
open Vimprobable in tabbed for tab management. A quick addition to my
keybinds in [dwm](http://dwm.suckless.org/ "dynamic window manager") and
I have a minimalist and powerful browser…

##### Notes
1. Of course, you are not limited to these files; the beauty of free
and open-source software is that you can change any of it…
      
2. To support this claim, I searched google for an image of the default
Vimprobable hinting style, and I could only find this 
<a href="http://www.linuxjournal.com/files/linuxjournal.com/linuxjournal/articles/103/10387/10387f2.jpg">screenshot of conkeror</a>,
which will give you an idea…
