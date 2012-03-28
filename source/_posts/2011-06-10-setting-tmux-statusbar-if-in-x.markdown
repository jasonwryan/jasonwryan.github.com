---
layout: post
title: "Setting tmux statusbar if in X"
date: 2011-06-10 22:56
comments: true
categories: [tmux, linux, hack]
---
[![Arch Linux: dwm, tmux & scratchpad](http://farm6.static.flickr.com/5303/5636783883_5b7272453b.jpg)](http://www.flickr.com/photos/jasonwryan/5636783883/ "Arch Linux: dwm, tmux & scratchpad by jasonwryan, on Flickr")

I have been posting quite a bit about
[tmux](http://tmux.sourceforge.net/ "tmux homepage") of late<sup>1</sup>. 
It is a marvellous application and lends itself to all sorts of interesting
uses. When you first install it, you will find a number of user config
files in <span class="file">/usr/share/tmux/</span>. I started with tmux by copying
[Thayer’s](http://cinderwick.ca/ "Thayer Williams' site") config across
and then working up from there. There is a section 
[at the bottom of this file](http://tmux.svn.sourceforge.net/viewvc/tmux/trunk/examples/t-williams.conf?revision=1833&view=markup "Thayer's conf in SVN")
that has always puzzled me, and so I decided to get to the bottom of it.

{% codeblock lang:sh %}
# show some useful stats but only when tmux is started
# outside of Xorg, otherwise dwm statusbar shows these already 
{% endcodeblock %}

It is a great idea. If you are running tmux in X, then you have
[dwm’s](http://dwm.suckless.org/ "dwm homepage") status bar telling you
what the time is, 
[how much battery you have left](http://beta.intuxication.org/jasonwryan/archer/file/tip/Scripts/dwm-status "dwm status bar in my mercurial repo")
etc,. You don’t need to replicate that functionality in tmux. If,
however, you are in the console, you don’t have that information, unless
you print it in tmux’s statusbar.

I had tried to get this to work once or twice over the last couple of
years, but never persevered with it. However, as I have been doing a bit
of work making the console as functional as possible<sup>2</sup>, I thought I
would revisit it.

![image](http://dl.dropbox.com/u/261312/Blog-images/tmux-status1.png)

The insight that I had that got it working for me was to use the
source-file option in <span class="file">.tmux.conf</span> to call the relevant statusbar,
depending if X was running or not. I just adapted Thayer’s code, like
so:

{% codeblock lang:sh %}
# Statusbar starting in X or not
if '[ -n "$DISPLAY" ]' 'source-file ~/.tmux/inx'
if '[ -z "$DISPLAY" ]' 'source-file ~/.tmux/xless'
{% endcodeblock %}

And then created those files with the desired statusbar options. So, for
example, if I am running tmux in the console, <span class="file">$HOME/.tmux/xless</span> is
loaded:

{% codeblock lang:sh %}
# Statusbar if tmux run in console
# information normally in dwm bar
set -g status-right-length 50
set -g status-left-length 50
set -g status-left "#[fg=green] #H #[default] …blah, blah, blah…
{% endcodeblock %}

Which prints a statusbar with the time and battery left on my laptop<sup>3</sup>:

![image](http://dl.dropbox.com/u/261312/Blog-images/tmux-status2.png)

All the relevant files are in 
[my mercurial repo](http://beta.intuxication.org/jasonwryan/archer/browse/tip/.tmux "My intuxication repository").

##### Notes
1. [Copy & paste in tmux](http://jasonwryan.com/blog/2011/06/07/copy-and-paste-in-tmux/ "Post on copy & paste in tmux"), 
[Sessions in tmux](http://jasonwryan.com/blog/2010/10/03/sessions-in-tmux/ "Sessions in tmux")
&amp; [Introduction to tmux](http://jasonwryan.com/blog/2010/01/07/tmux-terminal-multiplexer/ "Intro to tmux")

2. [Vim colours in the console](http://jasonwryan.com/blog/2011/04/06/vim-colours-in-the-console/ "Setting Vim colours in the console")

3. Image squashed to fit the requirements of this blog…
