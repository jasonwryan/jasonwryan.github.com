---
layout: post
title: "monsterwm"
date: 2012-01-18 10:03
comments: true
categories: [archlinux]
---
![image](/images/post_images/monster.jpg)

If you have visited here at all over the last couple of weeks, you would
have seen some screenshots from 
[my Flickr stream](http://www.flickr.com/photos/jasonwryan/ "All of my screenshots on Flickr")
of a new window manager,
[monsterwm](https://github.com/c00kiemon5ter/monsterwm "Project on Github").
Over the break, I have been playing around with it and, despite the fact
that the project is only really a few weeks old, it is well worth a
look.

Originally a fork of
[dminiwm](https://github.com/moetunes/dminiwm "dminiwm on Github"),
which was in turn based on
[catwm](https://github.com/pyknite/catwm "Project on Github") and
[dwm](http://dwm.suckless.org "dwm on suckless")<sup>1</sup>, monsterwm for
me—having used dwm for years—feels very much like a stripped down
version of dwm, with a few of the patches (like pertag, for example)
built in.

One of the projects goals, like it’s antecedents, is to keep the SLOC
low. By way of comparison, dwm’s long-standing goal has been to keep the
SLOC below 2000<sup>2</sup>. monsterwm’s current count is under 700. One of the
ways that monsterwm achieves this is *not* to incorporate a status bar;
as the README says:

> Monsterwm does not provide a panel and/or statusbar itself. Instead it
> adheres to the [UNIX philosophy](http://en.wikipedia.org/wiki/Unix_philosophy "Wikipedia entry")
> and outputs information about the existent desktop, the number of
> windows on each, the mode of each desktop, the current desktop and
> urgent hints whenever needed. The user can use whatever tool or panel
> suits him best (dzen2, conky, w/e), to process and display that
> information.

So, what is it like to run? Obviously it is fast. It comes with four
tiling modes: standard tile, backstack, grid and monocle and, as
mentioned above, the ability to define different layouts per desktop.
There are still a couple of features to be implemented; floating mode
being one of the more significant and a couple of minor bugs—as you
would expect for a project still in its relative infancy.

Overall, it is a very nice, minimalist window manager. If you don’t use
the [dynamic tagging feature of dwm](http://lubutu.com/rant/dwm-faq "CLS on dwm's tagging feature"),
then this would be a window manager to consider switching to. The
developer, Ivan “c00kiemon5ter” Kanakarakis, has been extremely
responsive to user’s suggestions and requests (the 
[thread on the Arch boards](https://bbs.archlinux.org/viewtopic.php?id=132122 "Arch Linux forums: monsterwm thread")
is quite active) and has been admirably clear about his vision for the
wm.

You can see my monsterwm configs in 
[my mercurial repository](https://bitbucket.org/jasonwryan/eeepc/src/241da582a0fd/Build/monsterwm-git "Mercurial repo: monsterwm").

##### Notes
1. Updated with the correct genealogy…

2. “dwm is only a single binary, and its source code is intended to
never exceed 2000 SLOC.” [dwm.suckless.org](http:..dwm.suckless.org "dwm homepage")

Cookie Monster image from [San Diego Shooter](http://www.flickr.com/photos/nathaninsandiego/3757033518/ "Creative Commons image on Flickr").
