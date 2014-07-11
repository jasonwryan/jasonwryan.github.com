---
layout: post
title: "Copy &amp; Paste in tmux"
date: 2011-06-07 19:54
comments: true
categories: [tmux, linux]
---
![image](/images/post_images/tmux-copy.png)

I’ve posted a couple of times<sup>1</sup> about
[tmux](http://tmux.sourceforge.net/ "tmux homepage"), the brilliant
terminal mutliplexer. One question I see raised a lot in \#tmux is how
to copy and paste.

The documentation for tmux is excellent: the 
[man page](http://www.openbsd.org/cgi-bin/man.cgi?query=tmux&sektion=1 "BSD man page: tmux")
one of the most clear and thorough I have read. And while it is
explained there, I thought I would share my approach anyway.

First, I set tmux to use vi keys:

{% codeblock lang:sh %}
setw -g mode-keys vi
{% endcodeblock %}

Then I change the default keybinds to suit my workflow. As explained in
the man page, the default keys to enter and exit copy mode are <kbd>[</kbd> 
and <kbd>]</kbd> respectively. I reset those in my <span class="file">.tmux.conf</span>
to more intuitive and Vim-like keys:

{% codeblock lang:sh %}
unbind [
bind Escape copy-mode
unbind p
bind p paste-buffer
bind-key -t vi-copy 'v' begin-selection
bind-key -t vi-copy 'y' copy-selection
{% endcodeblock %}

This seems a lot closer to all of the other Vim-like apps that I run -
where <kbd>Escape</kbd> changes the mode, <kbd>v</kbd> begins visual selection, 
<kbd>y</kbd> yanks text to the buffer and <kbd>p</kbd> puts text copied from the buffer.

So, to string it all together, the command sequence necessary to copy
some text from tmux and paste it is now (I bind my prefix to <kbd>t</kbd>):

<pre>
<kbd>Ctrl</kbd><kbd>t</kbd>,<kbd>Escape</kbd>   # enter copy mode
# move cursor to the start or end of the desired text string
<kbd>v</kbd>                        # to activate highlighting
# move cursor to cover the requisite string
<kbd>y</kbd>                        # to capture the string
<kbd>q</kbd>                        # exit copy mode
<kbd>Ctrl</kbd><kbd>t</kbd>,<kbd>p</kbd>       # put/paste the text in the desired location
</pre>

Simple.

This means that you can scroll back through the output of *anything* in
your terminal, and copy and paste it anywhere. Once you set it up, it is
a very powerful tool…

##### Notes
1.[tmux - Terminal-multiplexer](http://jasonwryan.com/blog/2010/01/07/tmux-terminal-multiplexer/) &amp; [Sessions in tmux](http://jasonwryan.com/blog/2010/10/03/sessions-in-tmux/)
