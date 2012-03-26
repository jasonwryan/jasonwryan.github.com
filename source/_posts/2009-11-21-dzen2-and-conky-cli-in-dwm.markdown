---
layout: post
title: "dzen2 and conky-cli in dwm"
date: 2009-11-21 12:07
comments: true
categories: [dwm, conky, dzen, archlinux] 
---
I struggled over the last couple of days to pipe conky into a Dzen2 status bar alongside the default dwm bar.

Here is how I sorted it.

First, I installed `conky-cli` and `dzen2`. Conky-cli is in AUR, and dzen2 is in Community:

{% codeblock lang:sh %}
pacman -S dzen2 && yaourt conky-cli
{% endcodeblock %}

Then it was a matter of setting up the relevant files.

{% codeblock .xinitrc.sh %}
#!/bin/sh
# ~/.xinitrc
setxkbmap -option terminate:ctrl_alt_bksp &amp;
eval `cat ~/.fehbg` &amp;
numlockx &amp;
xscreensaver -no-splash &amp;
# Dzen &amp; conky
(sleep 15s &amp;&amp; conky | dzen2 -x '500' -e '' -fg '#dcdcdc' -bg '#3f3f3f' \
-w '650' -ta r -fn '-*-terminus-*-r-normal-*-*-120-*-*-*-*-iso8859-*' -p ) &amp;
# Start dwm
exec ck-launch-session ~/Scripts/dwm-start
{% endcodeblock %}

{% codeblock .conkyrc .sh %}
background no
out_to_console yes
update_interval 2
total_run_times 0
use_spacer none
double_buffer yes
TEXT
${if_existing /sys/class/power_supply/BAT0/present}^i(/home/jason/Build/dwm/dzen/bat.xbm)…
{% endcodeblock %}

{% codeblock dwm-start .sh %}
while true; do 
    dwm >/dev/null
done
{% endcodeblock %}

The key is —and it took me a while to work it out— to delay the loading of conky until <em>after</em> dwm has painted it's
status bar; otherwise `conky | dzen2` is obscured.

Now, the “before” shot:

{% img http://dl.dropbox.com/u/261312/Blog-images/conky-dzen.png %}

…and the finished product:

<a href="http://www.flickr.com/photos/jasonwryan/4121324068/" title="Conky-cli to dzen2 in dwm on Arch by jasonwryan, on Flickr" target="_blank"><img src="http://farm3.static.flickr.com/2666/4121324068_6ec29262d7.jpg" width="500" height="293" alt="Conky-cli to dzen2 in dwm on Arch"/></a>
