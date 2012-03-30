---
layout: post
title: "Using Dropbox to Share Dotfiles"
alias: /post/327549146/dotfiles
date: 2010-01-11 11:10
comments: true
categories: [linux, hack]
---
I regularly back up all my dotfiles to <a href="http://www.dropbox.com/" title="Synch your computers securely...">Dropbox</a> 
as a matter of course. I had intended, at some point, to set up a 
<a href="http://git-scm.com/" title="Online version control system">Git repo</a> to automate the process and to make it easier to share these files with others.

However, best intentions etc., in the end it was far easier to simply extend what I was already doing using Dropbox.

First, I created a directory in my <span class="file">Dropbox/Public</span> that would contain the dotfiles I wanted to share:

{% codeblock lang:sh %}
$ mkdir Dropbox/Public/Configs
{% endcodeblock %}

Then it was just a matter of creating links to the files I wanted to host there:

{% codeblock lang:sh %}
$ cd Dropbox/Public/Configs
$ ln -s ../../path/to/dotfile
…
{% endcodeblock %}

The next step was to create a simple <acronym title="HyperText Markup Language">HTML</acronym> page 
(and a stylesheet to make it look presentable) and drop them into the same directory.

<a href="http://dl.dropbox.com/u/261312/Configs/index.html" title="jason's dotfile repo"><img src="http://dl.dropbox.com/u/261312/Blog-images/dropbox-repo.png"></a>

### Caveat ###
Dropbox has the very handy feature of retaining versions of all the files you upload. Unfortunately, in this case, that means that the symlinks will always point to the file that you originally set them at — <em>irrespective of any updates you overwrite them with</em>.

So, if you change any of the files significantly, you will need to recreate the symlink…

#### Update ####
It seems that the symlink system does work: Dropbox will respect the latest version. YMMV.
