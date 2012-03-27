---
layout: post
title: "Sessions in tmux"
date: 2010-10-03 20:15
comments: true
categories: [tmux, archlinux, hack]
---
<a href="http://www.flickr.com/photos/jasonwryan/5046091307/" title="Sessions in tmux by jasonwryan, on Flickr" target="_blank"><img src="http://farm5.static.flickr.com/4146/5046091307_39c200f1b0.jpg" width="500" height="293" alt="Sessions in tmux"/></a>

Now that I have been using it for several months, I have become absolutely dependent upon <a href="http://tmux.sourceforge.net/" target="_blank">tmux</a>, the terminal multiplexer.
It is both ruthlessly efficient, and can be configured to match exacting requirements. One of those options is to initialize sessions with a preloaded window configuration. After reading the <a href="http://wisconsinlinux.org/tmux-manpage-from-cvs/tmux.1.txt" target="_blank">man page</a> (which, truth be told, could be a little clearer) and some experimentation, I have worked out how to set up my tmux sessions how I want them.
I have just updated the <a href="http://wiki.archlinux.org/index.php/Tmux">Arch Wiki tmux entry</a>,
so thought I would share it here.

The relevant part of my config file looks like this:

{% codeblock lang:sh %}
# session initialization
new  -n TTYtter /home/jason/Scripts/ttytter.pl
neww -n irssi /home/jason/Scripts/startirssi.sh
neww -n ncmpcpp/vifm ncmpcpp
splitw -v -p 50 -t 0 vifm
neww -n bash bash
selectw -t 1
{% endcodeblock %}

Which, when I start tmux with the attach (or a) option, gives me the following window layout:

* TTYtter
* irssi
* ncmpcpp/vifm &nbsp; &nbsp; # window split vertically in half, ncmpcpp on top of vifm
* bash

The `selectw` option means that the default view is TTYtter.

Simple.
