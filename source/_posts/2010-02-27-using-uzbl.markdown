---
layout: post
title: "Using Uzbl"
date: 2010-02-27 10:19
comments: true
categories: [archlinux, browser]
---
[![Arch &
uzbl](http://miromiro.com/Blog-images/uzbl.jpg)](http://www.flickr.com/photos/jasonwryan/4352140405/ "Arch & uzbl by jasonwryan, on Flickr")

A couple of weeks ago, I switched my default browser from
[Conkeror](http://conkeror.org/ "Conkeror homepage") to
[uzbl](http://www.uzbl.org/ "uzbl homepage"). It wasn’t that I was
dissatisfied with Conkeror *per se*; more that as I was almost
exclusively using vim-like applications, it seemed almost perverse to be
browsing with an application that used Emacs-style keybindings. Over
time, the simplest operations seemed—in comparison to
[Vim](http://www.vim.org/ "THE $EDITOR")— to demand ridiculously complex
key strokes.

So, I installed uzbl.

> Uzbl follows the UNIX philosophy - “Write programs that do one thing
> and do it well. Write programs to work together. Write programs to
> handle text streams, because that is a universal interface.”
> [uzbl homepage](http://www.uzbl.org/)

Consequently, the default uzbl version is designed to do little more
than browse web pages. It doesn’t handle cookies, manage downloads,
block ads, save bookmarks, or do any of the things that you would expect
of its WebKit kin, ~~Firefox~~ Safari and Chrome.

This isn’t to say that it *can’t* do any of these things; like any
well-designed
[UNIX](http://en.wikipedia.org/wiki/UNIX "Wikipedia entry") program, it
is extensible and interoperable with programs through a range of
scripts. The difference is that you get to choose which functionality
matters to you, and implement it on an as-needs basis. This makes uzbl
very light, very fast, and very powerful.

The version I installed, uzbl-tabbed, ships with a number of default
scripts that perform most of the functions that you will need. There is
also a [scripts page](http://www.uzbl.org/wiki/scripts "uzbl wiki: scripts")
on the wiki that has a growing number of scripts contributed by users.

I installed a couple of [Pat Brisbin’s scripts](http://pbrisbin.com:8080/pages/scripts.html "Pat's scripts page");
an ingenious bash script for bookmarking that creates an HTML page of
your bookmarks that you can serve locally or remotely (I keep mine on
[Dropbox](http://www.dropbox.com "Dropbox home") so I can access it from
my phone as well), and a tidy little download manager.

Like Vim, uzbl is modal; you switch between command and insert mode. By
configuring uzbl to use Vim key bindings, you can seamless integrate it
into your desktop environment. Here you can see the status bar showing
uzbl in command mode, on the Arch forums home page, which has completely
loaded (the other figure is the X root window number).

![image](http://miromiro.com/Blog-images/uzbl-bar.png)

As an Arch Linux user, I’m keen to support a project that has been
developed by the Arch community (if you want to see the history of the
project, there is a [long thread](http://bbs.archlinux.org/viewtopic.php?id=70700&p=1 "uzbl history")
on the Arch boards); but ultimately this software appeals because it is
so configurable: you can customize it to your heart’s content (see my
config file on my [mercurial repo](https://bitbucket.org/jasonwryan/eeepc/src/tip/uzbl/ "uzbl file on bitbucket")).

There are packages for most of the major distros: [give it a go](http://www.uzbl.org/get.php "Download and install details on the wiki").
