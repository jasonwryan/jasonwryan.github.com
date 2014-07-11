---
layout: post
title: "tmux &amp; SSH Keys"
date: 2011-09-10 16:46
comments: true
categories: [tmux, linux, bash]
---
![image](/images/post_images/tmux-ssh.jpg)

I have posted before about tmux<sup>1</sup>, the terminal multiplexer. One of the
(minor) frustrations that I have to-date done nothing about was setting
up persistent SSH key management across sessions. The ability to enter
the passphrase for my public keys once, and then to be able to access
remote hosts—or {git,hg} push—without having to re-enter a passphrase.

My solution is quite hackish, but it works and, given 
[the only other solutions](http://duckduckgo.com/?q=tmux+ssh-agent "tmux and ssh-agent on DDG")
I was able to find by searching seem even more convoluted, I thought I
would share it.

There are a couple of separate steps, none of them of any real
consequence, but each necessary.

First, in your <span class="file">$HOME/.tmux.conf</span> you need to enable update-environment:

{% codeblock lang:sh %}
set -g update-environment "DISPLAY SSH_ASKPASS SSH_AUTH_SOCK
SSH_AGENT_PID SSH_CONNECTION WINDOWID XAUTHORITY"
{% endcodeblock %}

From the [man page](http://www.openbsd.org/cgi-bin/man.cgi?query=tmux&sektion=1 "tmux manual"):

> Set a space-separated string containing a list of environment
> variables to be copied into the session environment when a new session
> is created or an existing session is attached. Any variables that do
> not exist in the source environment are set to be removed from the
> session environment (as if -r was given to the set-environment
> command).

The next step is to create a script (mine is bound to a key sequence to
launch from scratch, you can adopt a simpler approach) that starts tmux
and initializes ssh-agent:<sup>2</sup>

{% codeblock lang:sh %}
#!/bin/bash
PID=$(pgrep tmux)
new="tmux -f $HOME/.tmux/conf new -s secured"
old="tmux attach -t secured -d"

if [[ -z "$SSH_AUTH_SOCK" ]]; then
    eval `ssh-agent`
    trap "kill $SSH_AGENT_PID" 0
fi

if [[ -z "$PID" ]]; then
    urxvtc -title "SSH" -e sh -c "${new}"
else
    urxvtc -title "SSH" -e sh -c "${old}"
fi

ssh-add
{% endcodeblock %}

The final step is to install
[keychain](http://www.funtoo.org/wiki/Keychain "Keychain homepage"), and
set it up. I followed the 
[instructions on the Arch Wiki](https://wiki.archlinux.org/index.php/SSH_Keys#Using_keychain "...because it rocks!")
and opted for a couple of lines in <span class="file">$HOME/.bash\_profile</span><sup>3</sup>, like so:

{% codeblock lang:sh %}
/usr/bin/keychain -Q -q --nogui ~/.ssh/id_rsa
[[ -f $HOME/.keychain/$HOSTNAME-sh ]] && 
    source $HOME/.keychain/$HOSTNAME-sh
{% endcodeblock %}

That’s it. Now, hitting a key sequence opens a terminal, I am prompted
for my passphrase and then, if I open another window, or another tmux
session *as long is the first session is still running, attached or
not*, I am authenticated and can SSH or push without a prompt.

#### Updated 7/10/11

I refined the sequence to provide me a little more flexibility: now the
keychain is only called if I start a specific named tmux session. To do
this, I changed my <span class="file">$HOME/.bash_profile</span> to test for that session:

{% codeblock lang:sh %}
tsess=$(tmux ls)

if [[ "${tsess%%:*}" = "secured" ]] && 
   [[ -f $HOME/.keychain/$HOSTNAME-sh ]]; then
    # start keychain
    /usr/bin/keychain -Q -q --nogui ~/.ssh/id_rsa
    . $HOME/.keychain/$HOSTNAME-sh
fi
{% endcodeblock %}

##### Notes
1.  Previous posts:
    -   [tmux: terminal multiplexer](http://jasonwryan.com/blog/2010/01/07/tmux-terminal-multiplexer/ "First post on tmux")
    -   [Sessions in tmux](http://jasonwryan.com/blog/2010/10/03/sessions-in-tmux/ "tmux sessions")
    -   [Copy & paste in tmux](http://jasonwryan.com/blog/2011/06/07/copy-and-paste-in-tmux/ "Copy and paste in tmux")
    -   [Setting tmux status bar if in X](http://jasonwryan.com/blog/2011/06/10/setting-tmux-status-bar-if-in-x/ "tmux status bars")

2.  Updated to include session test. 
3.  Alternatively, placing the script in <span class="file">/etc/profile.d/keychain.sh</span> will
    start the agent when you login, as opposed to starting a specific
    shell. Your call on security versus convenience…

Creative Commons image from [mer de glace](http://www.flickr.com/photos/marts-pics/2292588212/in/photostream/ "Public Key on Flickr")
