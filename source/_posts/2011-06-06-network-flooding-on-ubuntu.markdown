---
layout: post
title: "Network Flooding on Ubuntu"
date: 2011-06-06 13:40
comments: true
categories: [ubuntu, linux]
---
![image](/images/post_images/ubuntu-logo.png)

I have been experiencing problems with my wifi LAN when playing music
through
[mpd](http://mpd.wikia.com/wiki/Music_Player_Daemon_Wiki "Music Player Daemon homepage")
or watching movies. My upload speeds would climb to around 120-130Kb/s
and my wifi would just go down.

Apart from being frustrating, this was inconvenient as it meant that if
I was doing either of those things, my stereo in the living room
couldn’t stream music from my 
[squeezebox server](http://www.mysqueezebox.com/index/Home "Squeezebox home"), or
other members of the family (or guests) couldn’t connect to the access
point to get online.

After asking on 
[Ask Ubuntu](http://askubuntu.com/questions/45772/playing-music-or-movies-kills-my-wifi "My question on Ask Ubuntu")
(which, like many of the Ubuntu-related fora is populated with people
who are more well-intentioned than they are knowledgeable - which seems
to me one of the fundamental problems the distro has; but I’ll save that
rant for another day), one helpful user suggested I use
[wireshark](http://www.wireshark.org/ "Wireshark website") to diagnose
the network traffic.

I *really* should have thought of that myself. Nonetheless, it was
immediately instructive. Wireshark quickly identified that, as soon as I
started mpd, one of my ports was being flooded: 46560

`lsof` showed that pulse audio was the culprit:

{% codeblock lang:sh %}
pulseaudi 2190 jason 39u IPv4 14110 0t0 UDP mybox:56751->224.0.0.56:46560
{% endcodeblock %}

Quelle surprise… When software engineers want a case study of opacity
and abstraction from the user, this should be their first port of call.

Worse, it is a known bug. 
[First logged in August, 2009](https://bugs.launchpad.net/ubuntu/+source/pulseaudio/+bug/411688 "Pulse bug on launchpad").
And apparently insufficiently serious to warrant any attention. Fair
enough, I suppose, given the fix is easy enough *if you know what you
are trying to fix…*

Install the Pulse audio preferences tool:

{% codeblock lang:sh %}
sudo apt-get install papref
{% endcodeblock %}

…and disable Multicast/RTP.

Another example for me of Ubuntu’s fast waning utility as a distribution
for people that want to be able to exercize some control over their
system. It is only still installed on this machine because there isn’t
an official [Arch Linux mirror](https://wiki.archlinux.org/index.php/Mirrors "Arch wiki mirror list")
here and, with my international traffic capped to 30Kb/s until last
month, I wasn’t prepared to try and run a distro on that sort of
bandwidth. Now that has changed, I suspect that Ubuntu’s days are
numbered on this box.
