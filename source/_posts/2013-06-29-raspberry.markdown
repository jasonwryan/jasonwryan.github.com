---
layout: post
title: "RPi Headless Torrent Box"
date: 2013-06-29 09:40
comments: true
categories: [archlinux, arm, hack]
---
{% img left http://miromiro.com/Blog-images/raspberry-pi-logo.png Raspberry Pi logo %}
I was fortunate enough to be given a 
[Raspberry Pi](http://www.raspberrypi.org/ 'Pi homepage') earlier this year 
and, naturally enough, have been running 
[Arch ARM](http://archlinuxarm.org/ 'Arch Linux ARM site') on it over the 
last several months. Initially, I was at somewhat of a loss as to what to do 
with it, so was just (under)using it<sup>1</sup> for a persistent IRC connection. 
I have now, with very little effort, set it up as a headless torrent box and it is 
working nicely.

As is generally the case, this involved little more than stitching together a
few lightweight tools to accomplish the task. In this case, it is a combination
of 
[rtorrent](http://libtorrent.rakshasa.no/ 'rotrrent homepage') 
(with the canvas-colour patch applied), 
[surfraw](http://jasonwryan.com/blog/categories/surfraw/ 'Posts on surfraw')
and 
[w3m](http://w3m.sourceforge.net/ 'w3m homepage') for discovering 
torrents,<sup>2</sup>
[bittorrent sync](http://labs.bittorrent.com/experiments/sync.html 'Peer to Peer synching')
and, optionally, a <acronym title="Virtual Private Network">VPN</acronym>.

I use a 16GB <acronym title="Universal Serial Bus">USB</acronym> drive to act as 
storage for the device, and mount it at <span class="file">~/Downloads</span> 
via <span class="file">/etc/fstab</span>; depending on the size of your 
<acronym title="Secure Digital">SD card</acronym>, this may be superfluous.

First a note on building packages; building *anything* on the Pi—with only 
512MB <acronym title="Random Access Memory">RAM</acronym>—is a protracted 
and thankless task. So if you intend to do more of this, or do it regularly, 
set up
[cross-compiling](http://archlinuxarm.org/developers/distcc-cross-compiling 'Arch ARM instructions');
you won't regret the initial investment.

`rtorrent` is in the ARM repos, but I wanted to patch in colour, so I wrote 
[a PKGBUILD](https://gist.github.com/jasonwryan/5794623 'Gist of the thing…'), 
based on the package in [community] and 
[ashren's AUR package](https://aur.archlinux.org/packages.php?ID=31956 'Arch User Repository').
Once you have rtorrent installed and working, the rest is straightforward.

The default config for w3m is perfectly functional, but can be easily customized
further. The key to this setup is being able to find a torrent and then easily
add it to rtorrent's queue.  To do this, I modified the hack that I wrote up 
[a couple years back](http://jasonwryan.com/blog/2011/05/05/w3m/ 'Post on yanking URLs in w3m').
Essentially, you use w3m's external link function to trigger a script to grab 
the magnet link. Like so:

{% codeblock lang:sh ~/.w3m/config %}
extbrowser /home/jason/Scripts/magnets %s
{% endcodeblock %}

and the script from the 
[rtorrent wiki](http://wiki.rtorrent.org/MagnetUri#Handling_.22magnet:.22_URIs_via_a_bash_script 'Script for magnet links'):

{% codeblock lang:sh ~/Scripts/magnets %}
#!/bin/bash
cd $HOME/Downloads/Queue/watch/    # set your watch directory here
[[ "$1" =~ xt=urn:btih:([^&/]+) ]] || exit;
echo "d10:magnet-uri${#1}:${1}e" > "meta-${BASH_REMATCH[1]}.torrent"
{% endcodeblock %}

Remapping the w3m key to trigger the script to make it more Vim-like:

{% codeblock lang:sh ~/.w3m/keymap %}
keymap Y EXTERN_LINK
{% endcodeblock %}

And now hitting <kbd>Shift</kbd><kbd>y</kbd> over a magnet link adds it to your
queue.

The final piece was to ameliorate the step of transferring the completed files
from the Pi, or for allowing me to add torrents remotely. To do this, I used
a [bittorrent sync](http://jasonwryan.com/blog/2013/03/16/sync/ 'My post on Sync')
shared directory as <span class="file">~/Downloads</span>. All files are synched 
to my desktop as they are downloaded to the Pi. Simple.

{% img http://miromiro.com/Blog-images/rtorrent-2.png 'Screenshot of rtorrent in action…' %}

As I mentioned above, you could also use a VPN and, with a service
file, have it connect on boot if you wished to further protect your privacy.

#### Notes
1. This is not to say that I am now using it to it's full potential; quite the
   opposite. Recommendations or suggestions gratefully received.
2. Discovery is *your* business; what you choose to download and share may be the
   business of copyright holders…
