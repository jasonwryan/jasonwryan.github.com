---
layout: post
title: "Setting Urgent Hints in dwm"
date: 2011-01-25 17:45
comments: true
categories: [dwm, archlinux, bash]
---
[![image](/images/post_images/dwm-urgent.png)](http://www.flickr.com/photos/jasonwryan/5379701719/#/photos/jasonwryan/5379701719/lightbox/ "Arch Linux: dwm with statuscolors patch")

After nearly 12 months of running a pretty stable
[dwm](http://dwm.suckless.org "dwm homepage"), with a consistent
patchset (cycle,
[push](http://dwm.suckless.org/patches/push "Push on suckless wiki"),
and init pertag<sup>1</sup>) I finally added another one:
statuscolours<sup>2</sup>…

I have been experimenting with
[WMFS](http://jasonwryan.com/blog/2010/12/26/wmfs-window-manager-from-scratch/ "My post on using WMFS"),
which is another very nice, minimal tiling window manager and was really
enjoying the urgency hints in the status bar, so I decided to patch that
functionality into dwm.

Most terminal applications will send a bell when your attention is
required; in my case, I was looking for a way to trigger notifications
for new mail in
[mutt](http://www.mutt.org/ "If you are using another mail user agent, you are doing it wrong...")
or highlights in [irssi](http://irssi.org/ "IRC client"). In mutt, it
was just a case of setting a bell for when mail arrived:

{% codeblock lang:sh %}
set beep_new             # bell on new mail
{% endcodeblock %}

And in irssi:

{% codeblock lang:sh %}
beep_msg_level = "NOTICE MSGS HILIGHT";
{% endcodeblock %}

In [tmux](http://tmux.sourceforge.net/ "terminal mulitplexer"), to cover
all your terminal bases, add:

{% codeblock lang:sh %}
set -g bell-action any # listen for activity on all windows
{% endcodeblock %}

The next step is to pass those beeps to your terminal. The entry for
Urxvt is one I picked up after a quick search turned up 
[this post by rson](http://rsontech.net/articles/2008/11/17/1/handling-urgency-hints "Post on reson.tech")
which had the .Xdefaults line I was looking for:

{% codeblock lang:sh %}
URxvt*urgentOnBell: true
{% endcodeblock %}

Now, whenever a bell is triggered in irssi or mutt, that is passed to
the terminal as an urgent hint. The final step was patching statuscolors
into dwm.c to change the tag colour when the hint was received.

You can see my 
[patched dwm.c](https://bitbucket.org/jasonwryan/workstation/src/82e7f34ee0a3/dwm-5.8.2-2/5.8.2-2.diff  "dwm.c patched")
in my mercurial repo.

##### Notes
1. These patches are from the extremely helpful Arch Forum thread: 
<a href="https://bbs.archlinux.org/viewtopic.php?id=92895" title="Thread on Arch boards for dwm hackers">DWM Hackers Unite!</a>
2. I used a <a href="http://lists.suckless.org/dev/1011/6445.html" title="Dan Brown's patch">modified statuscolours patch</a>
from the suckless mailing list. It irons out the padding issues in the one on the wiki.
