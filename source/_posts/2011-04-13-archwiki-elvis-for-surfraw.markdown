---
layout: post
title: "Archwiki elvis for Surfraw"
date: 2011-04-13 19:29
comments: true
categories: [archlinux, surfraw, bash]
---
![image](http://miromiro.com/Blog-images/arch-elvis.png)

I have been using
[Surfraw](http://surfraw.alioth.debian.org/ "Surfraw manifesto") for the
better part of eighteen months, and I think that it is a perfect Unix
tool: it does *one* thing, extremely well… If you haven’t come across
it, Surfraw is a command-line interface to a couple of dozen
repositories of knowledge. It is described thus:

> It reclaims google, altavista, babelfish, dejanews, freshmeat,
> research index, slashdot and many others from the false-prophet,
> pox-infested heathen lands of html-forms, placing these wonders where
> they belong, deep in unix heartland, as god loving extensions to the
> shell.<sup>1</sup>

The shell scripts that power the searches of the repositories are called
*elvi*, the singular being *elvis*. Currently, in 
[the git repo](http://git.debian.org/?p=surfraw/surfraw.git;a=summary "Debian Git Repo: Surfraw"),
there are 
[over 100 elvi](http://git.debian.org/?p=surfraw/surfraw.git;a=tree;f=elvi;hb=HEAD "elvi in the git repo"),
ranging from Wikipedia through to the CIA website (yes, the project was
founded by [Julian Assange](http://en.wikipedia.org/wiki/Julian_Assange "Julian's Wikipedia page")),
Slashdot and The Wayback Machine.

As comprehensive as the list of elvi is, there was one glaring hole.
Arch Linux is represented in terms of elvi to search the
[AUR](http://aur.archlinux.org/ "Arch User Repository") and the
[official package repositories](http://www.archlinux.org/packages/ "Arch Package Database"),
but nothing for the 
[Arch Wiki](https://wiki.archlinux.org/index.php/Main_Page "Definitive Arch Linux documentation"),
perhaps the best GNU/Linux reference wiki on the web.

So I thought I would rectify that.

You can grab the archwiki elvis from my 
[mercurial repo](https://bitbucket.org/jasonwryan/eeepc/src/241da582a0fd/.config/surfraw/elvi/archwiki "mercurial repo"),
copy it into your local directory at <span class="file">$XDG\_CONFIG\_HOME/surfraw/elvi/</span> or
in the system folder at <span class="file">$XDG\_CONFIG\_DIRS/surfraw/</span> and you can surf the
Arch Wiki, or any one of the nine other language wikis, from the command
line.

> Surfrawize the soul of your favourite internet wonder. Join the
> **S**hell **U**sers’ **R**evolutionary **F**ront **A**gainst the
> WW**W** by submitting code. Reclaim heathen lands. Bear witness to the
> truth. Its love will set you free.

##### Notes
1. A bonus: all of the documentation is written like Unix
[agitprop](http://en.wikipedia.org/wiki/Agitprop "Wikipedia entry")…
