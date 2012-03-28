---
layout: post
title: "Using Vimprobable"
date: 2011-06-26 14:08
comments: true
categories: [linux, browser, hack]
---
![image](http://dl.dropbox.com/u/261312/Blog-images/vimprobable2.png)

I [posted last year about Vimprobable](http://jasonwryan.com/blog/2010-10-07-vimprobable "Post on using Vimprobable")
a [webkit browser](http://www.webkit.org/ "Webkit site") that uses
Vim-like keybinds and can be run completely without the rodent.

Over the last couple of months I have become increasingly comfortable
with it, and with the addition of 
[some great recent functionality](http://vimprobable.org/pipermail/vimprobable-users/2011-June/000800.html "External handlers patch on ML"),
it has become my default browser.

The project has good documentation, in the form of two man pages:
`vimprobable` and `vimprobablerc`, but I thought I’d share some of my
customizations and working methods.

### Copy &amp; Paste
The only time I have been tempted to take my hand off the keyboard was
to insert the cursor at the correct character to begin a selection to
copy and paste. This is unnecessary. With the right options in
<span class="file">$XDG\_CONFIG\_HOME/vimprobable/vimprobablerc</span>, 
you can do it all from the keyboard.

Map the keys you would like to use to enter and exit caret mode. I use
<kbd>Ctrl</kbd><kbd>c</kbd> to enter and <kbd>Shift</kbd><kbd>c</kbd> to 
return to normal mode:

{% codeblock lang:sh %}
map <C-c>=:set caret=true
map <S-c>=:set caret=false
{% endcodeblock %}

Then, it is just a matter of identifying the text that you wish to copy
(in the photo above I was copying a script to launch `tabbed`), and
highlighting it via search. So the sequence is:

<pre>
1.  <kbd>/</kbd> - to enter search mode
2.  <kbd>#</kbd><kbd>!</kbd> <kbd>Enter</kbd> - the shebang and then enter to highlight the correct
    sequence
3.  <kbd>Ctrl</kbd><kbd>c</kbd> to enter caret mode
4.  and then <kbd>Shift</kbd><kbd>Up</kbd> or <kbd>Shift</kbd><kbd>Down</kbd> to highlight the following or
    preceding lines
5.  yanking the selected text is <kbd>Shift</kbd><kbd>y</kbd>, and the text is now stored in
    your clipboard.
</pre>

### Opening new instances in tabbed
I highlighted the section on tabbed from the 
[Vimprobable site](http://www.vimprobable.org/ "Vimprobable website") for a reason.
The second tip is how to set up
[tabbed](http://tools.suckless.org/tabbed "tabbed on suckless.org") to
work well with other applications.

By default, any new windows (ie., instances of Vimprobable) opened from
within Vimprobable are “caught”—and managed— by `tabbed`. However, if you
open a link from, say
[newsbeuter](http://newsbeuter.org/ "ncurses rss reader") or
[Tyrs](http://tyrs.nicosphere.net/index.html "ncurses twitter/identi.ca client"),
this will open an instance outside `tabbed` which, if you are using a
tiling window manager like
[dwm](http://dwm.suckless.org/ "THE titling wm"), means this window gets
pushed to master in your stack. This is not entirely satisfactory.

Thanks to 
[Connor Lane Smith on the suckless ML](http://lists.suckless.org/dev/1106/8761.html "Tabbed thread on suckless ML"),
I have a solution to have all new instances of Vimprobable sent to
`tabbed`, whether opened in another application, or if I click on a link
in my terminal, [Urxvtc](http://www.rxvt.org/ "Rxvt. org").

CLS’s solution is elegantly simple. Launch tabbed with an `.xid` file in
<psan class="file">/tmp</span>:

{% codeblock lang:sh %}
"$(tabbed -d >/tmp/tabbed.xid); vimprobable2 -e $(</tmp/tabbed.xid)"
{% endcodeblock %}

Then it is just a matter of setting up other applications to pass links
to the `tabbed` <span class="file">tmp</span> file. For example, in my
<span class="file">$XDG\_CONFIG\_HOME/tyrs/tyrs.cfg</span>, I have:

{% codeblock lang:sh %}
openurl_command = vimprobable2 -e $(</tmp/tabbed.xid) %s
{% endcodeblock %}

The workaround for Urxvtc was a little trickier. I finally got there
with some help from 
[Riccardo Murri on Unix & Linux SE](http://unix.stackexchange.com/questions/15550/passing-clicked-links-in-rxvt-to-a-script/15555#15555 "Unix SE answer").
In my <span class="file">~/.Xdefaults</span> I have:
 
{% codeblock lang:sh %}
! -- Clickable urls -- !
URxvt.perl-ext-common: default,matcher
URxvt.urlLauncher: /home/jason/Scripts/vimprobtab.sh
URxvt.matcher.button: 1
{% endcodeblock %}

…and the wrapper script <span class="file">vimprobtab.sh</span> looks like this:
    
{% codeblock lang:sh %}
#!/bin/bash
# Script to open Vimprobable instances in tabbed
exec vimprobable2 -e $(</tmp/tabbed.xid) "$1"
{% endcodeblock %}

Now whenever I open a link in an application or by clicking a link in
the terminal (in [irssi](http://irssi.org/ "IRC client"), for example)
it is passed to Vimprobable in `tabbed`.
