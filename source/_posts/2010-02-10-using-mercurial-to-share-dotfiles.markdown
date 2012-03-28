---
layout: post
title: "Using Mercurial to Share Dotfiles"
date: 2010-02-10 20:36
comments: true
categories: [vcs, linux]
---
I posted a couple of weeks back about 
[using dropbox to share dotfiles](http://jasonwryan.com/blog/2010-01-11-using-dropbox-to-share-dotfiles)
and while I am pleased with how well that works, I had always intended to see how well a
version control system would handle that (relatively trivial) chore.

I chose [Mercurial](http://mercurial.selenic.com/) only because it is the 
system used by [the suckless community](http://suckless.org/) and, as their
[philosophy](http://suckless.org/manifest/) and the products they
produce are of such exceptional quality, no further investigation seemed
necessary.

Installing mercurial, on both my Arch machines and the Ubuntu box was simple
enough:

{% codeblock lang:sh %}
# pacman -S mercurial
{% endcodeblock %}

Then, rather than use my <span class="file">/home</span> directory as the local repo
(which for some reason—probably completely irrational—seemed somewhat risky),
I opted to create the repository in a separate directory in my home folder and
link to the relevant files.

I signed up for a free account at [bitbucket](http://bitbucket.org) and created a repository
called “dotfiles.”

The it was just a matter of cloning the new repository to my local
machine:

{% codeblock lang:sh %}
hg clone https://jasonwryan@bitbucket.org/jasonwryan/dotfiles/
{% endcodeblock %}

and then entering the newly created directory and linking to the files that
I wanted to share:

{% codeblock lang:sh %}
cd dotfiles/ ln /home/jason/.Xdefaults ln /home/jason/.apvlvrc
…
{% endcodeblock %}

Note: I initially tried symlinking, but Mercurial didn't like that
*at all*, hence the hard links…

Then it was just a matter of telling Mercurial that the requisite files
are in the folder:

{% codeblock lang:sh %}
hg add
{% endcodeblock %}

Commit the changes:

{% codeblock lang:sh %}
hg ci
{% endcodeblock %}

…and then push the changes to the [bitbucket repository](https://bitbucket.org/jasonwryan/):

{% codeblock lang:sh %}
hg push
{% endcodeblock %}

This was certainly a lot quicker to setup than the Dropbox method but, for
all that I am using it for, it seems a little like overkill. I'll chalk
it up as a good way for me to get my head around <acronym title="Version
Control Systems">vcs</acronym> more than anything else…

### The Mercurial Book
For more information on Mercurial, I highly
recommend Bryan O'Sullivan's excellent
[Mercurial: The Definitive Guide](http://hgbook.red-bean.com/).

#### Update 
Post edited to reflect that fact that I now run two repos,
one for the <a href="https://bitbucket.org/jasonwryan/workstation/src/"
title="work machine repo" target="_blank">workstation</a> (as described here),
and a second one for my <a href="https://bitbucket.org/jasonwryan/eeepc/src/"
title="Netbook repo" target="_blank">EeePC</a> – which I did successfully
create in <span class="file">/home</span> without any apocalyptic consequences.

