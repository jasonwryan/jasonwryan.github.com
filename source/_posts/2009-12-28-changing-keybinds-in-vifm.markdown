---
layout: post
title: "Changing Keybinds in vifm"
alias: /post/303470948/changing-keybindings-in-vifm
date: 2009-12-28 13:50
comments: true
categories: [linux, hack]
---
[Vifm](http://vifm.sourceforge.net/) is a ncurses based file manager with vi-like
keybindings; it gives you complete keyboard control over your files without
having to learn a new set of commands.

<a href="http://www.flickr.com/photos/jasonwryan/4220632362/" title="Vifm by
jasonwryan, on Flickr" target="_blank"><img src="http://farm5.static.flickr.com/4039/4220632362_ea980811e1.jpg" width="500"
height="227" alt="Vifm"/></a>

The only drawback, for me, is that as of the current version (0.5-1) you
can't map keys. While the developer has indicated it is on the todo list,
he also flagged that it is some way off. If, like me, you `map : ;`
in [Vim](http://www.vim.org/), then not having this functionality is a minor
irritant. Fortunately, it is also easily fixed. Ken Steen, the Vifm developer,
was kind enough to point me in the right direction…

Download [the tarball](http://aur.archlinux.org/packages.php?ID=6011) from 
<acronym title="Arch User Repository">AUR</acronym> and untar it etc.,

{% codeblock lang:sh %}
$ tar -xzf vifm.tar.gz cd vifm/
{% endcodeblock %}

Build the package

{% codeblock lang:sh %}
$ makepkg -s
{% endcodeblock %}

Then you will need to edit the file, keys.c

{% codeblock lang:sh %}
vim src/vifm-0.5/src/keys.c
{% endcodeblock %}

Search for the command key

{% codeblock lang:sh %}
/case ':':
{% endcodeblock %}

In this version it is at line 982. Change the entry to your key of choice —
in my case <kbd>;</kbd> — and save and quit.

Recompile Vifm and you are done&#8230;

{% codeblock lang:sh %}
$ makepkg -efi
{% endcodeblock %}

### Update 18/2/10  
Vifm has now been moved to Community, so you can use
[ABS](http://wiki.archlinux.org/index.php/Arch_Build_System%22) to build the package.
