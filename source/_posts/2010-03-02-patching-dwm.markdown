---
layout: post
title: "Patching dwm"
date: 2010-03-02 20:16
comments: true
categories: [dwm, archlinux]
---
![image](http://dl.dropbox.com/u/261312/Blog-images/patching-dwm.png)

I had posted [a while back](http://jasonwryan.com/blog/2009/12/19/patching-dwm-with-pertag-and-bstack/ "Patching dwm with pertag & bstack")
about my (failed) attempt to patch [dwm](http://dwm.suckless.org "dwm homepage") 
with both pertag and some of the other layout patches. I had been able to get it to work with
[bottomstack](http://dwm.suckless.org/patches/bottom_stack "dwm wiki: bstack"),
but adding another patch caused some of the chunks to fail. It seems
pertag moves the code around to the point that – using 
[the standard patching method](http://dwm.suckless.org/patches/ "patches: dwm wiki") –
any further patches won’t take.

Further investigation, and some trial and a tolerable amount of error,
have however demonstrated to me that it is quite possible to patch dwm
to your heart’s content; or at least to my meagre needs. Which in the
case of [my EeePC](http://dl.dropbox.com/u/261312/Configs/index.html "EeePC configs")
is pertag, cycle and push, with the addition of bstack on 
[my workstation](https://bitbucket.org/jasonwryan/workstation/src/ "Workstation configs").

The solution, as I have found in almost every instance of my tinkering
with dwm, is stunningly simple. Start with a new version of dwm and
apply the 
[pertag diff](http://dwm.suckless.org/patches/pertag "dwm wiki: pertag"). Then
manually add all of the chunks of the subsequent patches.

All of the changes are now in dwm.c and your config.h should only have
the necessary keybindings to activate the functionality you have added
to this amazing window manager.

If you want to see what changes this makes, you can take a peek at 
[my .diff](https://bitbucket.org/jasonwryan/workstation/src/tip/dwm-5.7.2-2/ "diff in mercurial repo")
(for bstack, cycle, pertag and push). You are equally welcome to apply
it if that is the functionality that you are looking for in 5.7.2.
