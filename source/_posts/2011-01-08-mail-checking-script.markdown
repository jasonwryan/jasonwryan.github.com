---
layout: post
title: "Mail Checking Script"
date: 2011-01-08 11:04
comments: true
categories: [archlinux, dwm, bash]
---
![image](http://dl.dropbox.com/u/261312/Blog-images/mailcheck.png)

I use [mutt](http://www.mutt.org/ "Mutt homepage") and
[offlineimap](https://github.com/jgoerzen/offlineimap/wiki "Offlineimap on github")
to manage email on my Arch Linux machines. I like the simplicity and the
power that this approach offers; that’s power in the sense of
extensibility and customization, by the way…

Using [dwm](http://dwm.suckless.org/ "dwm homepage") as my window
manager, I had relied on
[conky and dzen2](http://jasonwryan.com/blog/2009-11-21-dzen2-and-conky-cli-in-dwm/ "My post on setting this up")
to pipe system information into my status bar. Unfortunately, conky only
supports one mail spool so if, like me, you use offlineimap to
synchronize multiple mailboxes, conky would only show new mail in your
primary box. I had asked in \#conky about multiple mail spools, but it
doesn’t seem to be supported.

When I recently started using
[WMFS](http://jasonwryan.com/blog/2010-12-26-wmfs-window-manager-from-scratch/ "My post on Window Manager From Scratch")
I decided to move to a 
[bash script for my status information](http://beta.intuxication.org/jasonwryan/archer/file/tip/Scripts/wmfs-status "Script in intuxication repo").
So I approached the problem afresh and decided to write a script to poll
the new mail directories and update my status bar when mail arrived.

Offlineimap creates a directory structure like so:

       Mail/ -------
          |         |
          |     Mail_Account_1
          |         |
          |         | --  INBOX 
          |         |       | -- cur
          |         |       | -- new
          |         |       | -- temp
          |         |
          |         | -- INBOX.Drafts
          |         |       | -- cur

As I was only interested in checking the <span class="file">INBOX/new</span> 
directory of each account, I had imagined that it would be necessary to set up some
elaborate array to effectively check each of the requisite sub-directories…

No, it was pretty simple. Just
[globbing](http://www.faqs.org/docs/abs/HTML/globbingref.html "ABS entry on globing")
and
[find](http://linuxmanpages.com/man1/find.1.php "man page for find"),
actually.

{% codeblock mailcheck.sh %}
#!/bin/bash
# Set maildirs
maildirs="$HOME/Mail/*/INBOX/new/"

find $maildirs -type f | wc -l
exit 0
{% endcodeblock %}

The glob (`*`) takes care of the different mail account names, and has
the added bonus of being portable, and find returns any files (`-type f`)
with `wc` counting the number of lines (the `-l` switch).
