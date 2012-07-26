---
layout: post
title: "WMFS - Window Manager From Scratch"
date: 2010-12-26 11:08
comments: true
categories: [archlinux]
---
[![image](http://dl.dropbox.com/u/261312/Blog-images/wmfs.png)](http://www.flickr.com/photos/jasonwryan/5289976238/lightbox/ "Screenshot on Flickr")

I have been experimenting with a couple of new window managers over the
last week or so. Not that I am in any way dissatisfied with
[dwm](http://dwm.suckless.org/ "suckless page for dwm") — far from it; I
do from time to time, however, like to see what else is on offer.

[WMFS](http://wmfs.info/projects/wmfs "WMFS homepage") is a fairly new
entrant in the window manager marketplace. The developers describe it
as:

> a lightweight and highly configurable tiling window manager for X… It
> supports Xft (Freetype) fonts and is compliant with the Extended
> Window Manager Hints (EWMH) specifications, Xinerama and Xrandr.

The [git version is available](http://aur.archlinux.org/packages.php?ID=26924 "AUR page") in
AUR, and is straightforward to build and install. The fun, as always,
comes with customization…

As is says on the box, WMFS is highly configurable. The configuration is
all done (initially) in one file, which you will need to copy to your
<span class="file">~/.config/wmfs/directory</span>:

{% codeblock lang:sh %}
cp /etc/xdg/wmfs/wmfsrc ~/.config/wmfs/
{% endcodeblock %}

Even the default config is relatively fully featured. There are nine
default layouts (about five more than I usually need, but it is always
better to over cater). The <span class="file">wmfsrc</span> is quite well 
commented, so most of the options are self explanatory.

One thing I did note about editing this file, which is probably more a
reflection of my haste rather than the wm itself, is that it seemed
*very* unforgiving of any errors. More than once I had to copy the
default wmfsrc back across and start again. My advice is to make any
changes cumulatively, and test by reloading before continuing.

You can see my config file in my 
[bitbucket repo](https://bitbucket.org/jasonwryan/eeepc/src/36ae3d14507b/.config/wmfs/wmfsrc "Mercurial repository for EeePC").
Most of the changes have been made in setting up my preferred tags and
keybindings – so they are consistent with my dwm and
[echinus](http://plhk.ru/ "Echinus window manager home") setups.

One of the great features of WMFS is the status bar. You can pipe all
sorts of information into it with a bash script, and you can use colours
and icons (if that is your thing). Simply sourcing the bash script from
within wmfsrc will see it up and running:

{% codeblock lang:sh %}
background_command = "$HOME/.config/wmfs/autostart.sh"
{% endcodeblock %}

Or, if you prefer to use conky:

{% codeblock lang:sh %}
conky | while read -r; do wmfs -s -name "$REPLY"; done
{% endcodeblock %}

I use the status bar to display some basic system information. You can
see from 
[my status script](https://bitbucket.org/jasonwryan/eeepc/src/36ae3d14507b/Scripts/wmfs-status "Script in bitbucket repo")
that most of it is simply piping commands through the awesomeness that
is [awk](http://awk.info/ "Awk Community Portal").

I hadn’t thought that I would find a replacement for dwm, but WMFS is a
serious contender. It has all of the functionality out-of-the-box,
including:

-   pre-set layout per tag
-   rules to send applications to specific tags
-   completely customizable keybinds, and – as a bonus,
-   [Vi](http://en.wikipedia.org/wiki/Vi "Wikipedia page")-driven
    command interface

Unlike dwm, it requires no patching for any of this. It has been
completely stable while I have been using it and, while not as light as
dwm, it is still in the minimalist camp in terms of size.
