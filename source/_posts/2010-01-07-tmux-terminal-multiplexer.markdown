---
layout: post
title: "tmux - terminal multiplexer"
date: 2012-03-27 13:28
comments: true
categories: [tmux, terminal, unix, linux]
---
Over the last couple of days I have been familiarising myself with [tmux](http://tmux.sourceforge.net/). 
As it says on the homepage,

{% blockquote %}
tmux is a terminal multiplexer: it enables a number of terminals (or windows), each running a separate program, to be created, accessed, and controlled from a single screen. tmux may be detached from a screen and continue running in the background, then later reattached.
{% endblockquote %}

I was attracted to tmux after seeing a number of enthusiastic postings on the 
[Arch forums](http://bbs.archlinux.org/viewtopic.php?id=84157), and by the promise of 
better management of multiple terminals in a single [dwm tag](http://dwm.suckless.org/)…

It's available in the community repo, so:

{% codeblock lang:sh %}
pacman -S tmux
{% endcodeblock %}

Then it is just a matter of configuring it to suit your style of working, 
keyboard bindings etc. Helpfully, it comes with some example config files to 
start you off. These can be found at <span class="file">/usr/share/tmux/*.conf</span>. 
I borrowed heavily from [Thayer Williams'](http://cinderwick.ca/) for mine:

{% codeblock lang:sh %}
# set prefix key to ctrl+t
unbind C-b
set -g prefix C-t

# more intuitive keybindings for splitting
unbind %
bind h split-window -h
unbind '"'
bind v split-window -v

# set vi keys
unbind [
bind Escape copy-mode
setw -g mode-keys vi

# send the prefix to client inside window (ala nested sessions)
bind-key a send-prefix

# toggle last window like screen
bind-key C-b last-window

# confirm before killing a window or the server
bind-key k confirm kill-window
bind-key K confirm kill-server

# toggle statusbar
bind-key b set-option status

# ctrl+left/right cycles thru windows
bind-key -n C-right next
bind-key -n C-left prev

# open a man page in new window
bind / command-prompt "split-window 'exec man %%'"

# quick view of processes
bind '~' split-window "exec htop"

# scrollback buffer n lines
set -g history-limit 5000

# on-screen time for display-panes in ms
set -g display-panes-time 2000

# start window indexing at one instead of zero
set -g base-index 1

# enable wm window titles
set -g set-titles on

# wm window title string (uses statusbar variables)
set -g set-titles-string "tmux:#I #W"

# session initialization
new -s TTYtter ttytter
neww -t 2
neww -d -t 3
neww -d -t 5 
selectw -t 1

#### statusbar ####
set -g status-interval 1
set -g status-justify centre # center align window list
set -g status-left '#[fg=green] #H #[default]'
set -g status-right '#[fg=blue,bright]Up#(uptime | cut -f 4-5 -d " " | cut -f 1 -d ",") 
#[default]:: #[fg=cyan]#(cut -d " " -f 1-4 /proc/loadavg) '

# default statusbar colors
set -g status-fg white
set -g status-bg default
set -g status-attr bright

# default window title colors
set-window-option -g window-status-fg white
set-window-option -g window-status-bg default
set-window-option -g window-status-attr dim

# active window title colors
set-window-option -g window-status-current-fg white
set-window-option -g window-status-current-bg default
set-window-option -g window-status-current-attr bright

# command/message line colors
set -g message-fg white
set -g message-bg black
set -g message-attr bright
{% endcodeblock %}

Copy this file to <span class="file">~/.tmux.conf</span>. The final result:

<a href="http://www.flickr.com/photos/jasonwryan/4252840248/" title="Archlinux:  tmux running in dwm by jasonwryan, on Flickr" target="_blank"><img src="http://farm3.static.flickr.com/2703/4252840248_01ca8e7cc1.jpg" width="500" height="293" alt="Archlinux:  tmux running in dwm"/></a>

### Updated 16/11/10 ###
For a current version of my <span class="file">.tmux.conf</span>, check my 
<a href="https://bitbucket.org/jasonwryan/eeepc/src/tip/.tmux.conf" title="bitbucket site" target="_blank">mercurial repository</a>. 
