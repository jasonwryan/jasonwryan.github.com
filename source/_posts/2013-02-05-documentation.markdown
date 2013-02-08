---
layout: post
title: "Read The Fine Manual"
date: 2013-02-08 19:57
comments: true
categories: [archlinux, community]
---
{% img left http://dl.dropbox.com/u/261312/Blog-images/documents.jpg 'Documents on Flickr' %}
One of the canards about Free and Open Source Software that is peddled by the unscrupulous
marketers of proprietary software is that their product is superior because, among other
things, the scruffy, sandal-wearing Communists that write and distribute their software
for free do not bother to adequately document that software; thereby placing in jeopardy
the business processes in the enterprise that rely on said software to continue to
profitably grease the wheels of the juggernaut of Capitalism.

At heart, it really is an attempt to paint FOSS as a hobbyist's pursuit, unworthy of 
consideration by Fortune 500 <acronym title="Chief Inhibiting Officers">CIOs</acronym>
as it is not in the same league as “professional,” proprietary product. This 
<acronym title="Fear, Uncertainty & Doubt">FUD</acronym> is not restricted to 
the usual suspects; it has also been raised by other, presumably more
[objective commentators](http://www.osnews.com/story/5180 'Editorial on OS News'):

{% blockquote OSNews Editorial, 2003 http://www.osnews.com/story/5180 %}
Open Source documentation is terrible. There is no kinder way to put it. Even the commercially distributed software in the Open Source world has substandard documentation. (And believe me, as a technical writer coming from the Windows world, when I tell you that the commercial industrial standards are pretty low). Only a bare handful of Open Source software packages have any documentation at all. And out of the few that do, much of it is either unfinished, inaccurate, or outdated. What counts as explanatory text among geeks would bewilder a nuclear physicist, even a sober one.
{% endblockquote %}

A more recent survey, published in 2010, identifed the same issue for businesses,
in slightly less histrionic language:

{% blockquote Zenoss 2010 Survey http://gigaom.com/2010/08/11/enterprise-yeah-were-cool-with-open-source/ %}
The No. 1 reason for not choosing open-source solutions was lack of support followed by poor documentation.
{% endblockquote %}

My experience of FOSS documentation, particularly over the last twelve months, has been just the 
opposite. 

From the middle of 2012 on, I made a couple of significant changes in my computing
environment; I moved from 
[SysVinit](https://wiki.archlinux.org/index.php/SysVinit 'Archwiki page…')
to [systemd](http://freedesktop.org/wiki/Software/systemd 'Project web page'),
garden variety to
[UEFI booting](http://jasonwryan.com/blog/2013/01/25/uefi/ 'My post on UEFI booting')
and changed my shell from 
[Bash](http://www.gnu.org/software/bash/ 'GNU Bash page') to 
[Zsh](http://www.zsh.org/ 'Zsh homepage, one of the ugliest on the Web'). 
In each of these cases I found myself having to turn to the documentation the
community has provided in order to understand the implications of the change,
and to familiarise myself with the new tool or approach.

I posted at the time about [moving to systemd](http://jasonwryan.com/blog/2012/08/04/systemd/ 'The Leap to systemd'),
and I don't think—in retrospect—I credited Lennart Poettering enough for the series of posts
he wrote detailing the design decisions and implementation of that system; 
those posts (at the time 12, now
[grown to 20](http://0pointer.de/blog/projects 'Lennart Poettering's blog'))
are an impressively thorough, and thoughtful, document of the new init system.

Similarly, when I started reading about <acronym title="Universal Extensible Firmware Interface">UEFI</acronym>
booting, I quickly found myself at [Rod Smith's site](http://www.rodsbooks.com/ 'The author of… Just go there!').
Rod has compiled an exhaustive and authoritative resource on [GPT](http://www.rodsbooks.com/gdisk/ 'gdisk page'),
EFI Stub loading and UEFI booting in general. Reading through his clear, approachable
documentation made transitioning to UEFI a much simpler process than, given the 
inherent complexity in the ecosystem, I would have thought possible.

Then there is Zsh. 

{% blockquote zsh manual http://linux.die.net/man/1/zsh %}
Because zsh contains many features, the zsh manual has been split into a number of sections…
{% endblockquote %}

And goes it on to list those *seventeen* sections. If you are not a fan of `man` pages<sup>1</sup>, 
there is the 
[Zsh Documentation](http://zsh.sourceforge.net/Doc/Release/zsh_toc.html 'What it says on the tin…'),
the [User's Guide](http://zsh.sourceforge.net/Guide/zshguide.html 'User's Guide') and
[wiki](http://zshwiki.org/home/ 'Zsh Wiki'). This amounts to a literally staggering
amount of information—it is no wonder that preconfigured zsh systems like
[grml](http://grml.org/zsh/ 'grml homepage') are so popular; they provide
relatively simple ingress to the byzantine ways of this powerful shell.

Finally, also last year, I was privileged to chair the judging for the 
[2012 New Zealand Open Source Awards](http://www.nzosa.org.nz/ 'NZOSA site').
This exposed me to the amazing contribution 
[Michael Kerrisk](http://man7.orgi/mtk.index.html 'Man pages Michael maintains')
has made to GNU/Linux over the last decade. He is the author, or co-author, of
*over 300* `man` pages; and these aren't the glamorous, application pages read
by the masses, but the system pages that document the kernel and `glibc`
<acronym title="Application Program Interface">APIs</acronym>.

All of this suggests to me that the state of documentation for FOSS is
anything but parlous. At every turn over the last year, when I needed to
be able to refer to documentation for a project, there was a surfeit of it.
And I have only touched on what would normally constitute the primary
sources; there is also all of the other community-driven documentation
around projects, like the 
[Arch Wiki](https://wiki.archlinux.org/ 'The BEST GNU/LINUX wiki on the web')
or the [Stack Exchange sites](http://unix.stackexchange.com/ 'Unix & Linux SE'),
the support available on forums, IRC, blog posts, etc.

Compared to the sterile, mostly out-of-date (and expensive) documentation that 
accompanies proprietary software, FOSS is an Alexandrine utopia…

#### Notes
1. And, god knows, *I am*; `man` pages are a constant source of knowledge
and delight (and pretty much the only way I can contribute to projects that
I am indebted to).

Creative Commons image of documents by 
[Marcin Wichary](http://www.flickr.com/photos/mwichary/2322639175/) on Flickr.
