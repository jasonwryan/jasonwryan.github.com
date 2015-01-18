---
layout: post
title: "Playing with overlayfs"
date: 2015-01-19 08:20
comments: true
categories: [archlinux, bash, hack, kernel]
---
{% img left /images/post_images/jello.jpg Creative Commons Image %}
Around this time last year, I posted about setting up a
[udev rule to run a script](http://jasonwryan.com/blog/2014/01/20/udev/ 'Post in Jan 14')
when I plugged my USB drive containing all of my music into one of my
laptops; the script, a 
[couple of lines of bash](https://gist.github.com/jasonwryan/0df98a396af89bf82eb6 'A gist…'),
removes all pre-existing symlinks to <span class="file">$HOME/Music</span> and
repopulates the directory with an updated set. Almost. The one flaw that has
been an irritant of variable intensity, depending on what I felt like listening
at any given time, is that the symlinks aren't written for directories that
already exist on the target filesystem.

In order that I am able to play some music if I forget the USB drive, each of 
the laptops has a subset of albums on it, depending on the size of their
respective hard drives. If I add a new album to the USB drive, then that change won't
get written to either of the laptops when the drive is plugged in. Not entirely
satisfactory. I had tinkered around with 
[globbing](http://mywiki.wooledge.org/glob 'Wooledge wiki entry'), or with
having [`find(1)`](http://mywiki.wooledge.org/UsingFind 'Wooledge again, because it is so great…')
scan deeper into the tree, or even a loop to check for the presence of directories in an array…

It just got too hard. My rudimentary scripting skills and the spectre of recursion,
I am sorry to admit, conspired to undermine my resolve. So, rather than concede
unconditional surrender, I
[asked for help](http://unix.stackexchange.com/q/179397/6761 'Question on Unix & Linux SE).
As is almost always the case in these situations, this proved to be a particularly
wise move; the response I received was neither what I expected, nor was it anything I
was even remotely familiar with: so in addition to an excellent solution (one far 
better suited to what I was trying to achieve), I learned something new.

The first comment on my question proved singularly insightful.

{% blockquote muru on U&L https://unix.stackexchange.com/questions/179397/create-symlink-tree-in-existing-directories#comment298386_179397 %}
Care to use union mounts, for example via overlayfs?
{% endblockquote %}

A union mount, something until now I was blissfully unaware of, is according to Wikipedia,

{% blockquote https://en.wikipedia.org/wiki/Union_filesystem %}
a mount that allows several filesystems to be mounted at one time, appearing to be one filesystem.
{% endblockquote %}

Union mounting has a long and storied history on Unix, beginning in 1993 with the 
[Inheriting File System (IFS)](http://icapeople.epfl.ch/almesber/ifs.html 'IFS page').
The genealogy of these mounts has been well covered in this 2010 LWN
[article by Valerie Aurora](http://lwn.net/Articles/396020/ 'LWN.net'). However, it is only
in the current kernel, 3.18, that a union mount has been accepted into the kernel tree.

After reading the
[documentation for overlayfs](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/Documentation/filesystems/overlayfs.txt 'Kernel docs'), it seemed this was *exactly* what I 
was looking for. Essentially, an overlay mount would allow me to “merge" 
the underlying tree (the Music directory on the USB drive) with an “upper” 
one, <span class="file">$HOME/Music</span> on the laptop, *completely 
seamlessly*. 

{% blockquote Kernel docs %}
Then whenever a lookup is requested in such a merged directory, the lookup is performed in each actual directory and the combined result is cached in the dentry belonging to the overlay filesystem.
{% endblockquote %}

It was the just a matter of adapting my script to use `overlayfs`, which was 
trivial:

{% codeblock lang:bash %}
#!/usr/bin/env bash
# union mount Music when Apollo plugged in

low=/media/Apollo/Music
upp=/home/jason/Music
wod=/home/jason/.local/tmp
export DISPLAY=:0
export XAUTHORITY=/home/jason/.Xauthority

# overlayfs mount
mount -t overlay -o lowerdir="$low",upperdir="$upp",workdir="$wod" overlay "$upp"
status1=$?

mpc update &>/dev/null
status2=$?

if [[ "$status1" -eq 0 && "$status2" -eq 0 ]]; then
    printf "^fg(#BF85CC)%s\n" "Music directory updated" | dzen2 -p 3
fi
{% endcodeblock %}

And now, when I plug in the USB drive, the contents of the drive are merged
with my local music directory, and I can access whichever album I feel inclined
to listen to. I can also copy files across to the local machines, knowing if I
update the portable drive, it will no longer mean I have to forego listening to
any newer additions by that artist in the future (without manually intervening,
anyway).

Overall, this is a lightweight union mount. There is neither a lot of functionality,
nor complexity. As the 
[commit note](https://github.com/torvalds/linux/commit/e9be9d5e76e34872f0c37d72e25bc27fe9e2c54c 'Git commit')
makes clear, this “simplifies the implementation and allows native performance
in these cases.” Just note the warning about attempting to write to a mounted
underlying filesystem, where the behaviour is described as “undefined”.

#### Notes
Creative Commons image, [mulitlayered jello](https://flic.kr/p/5RRRXP) by Frank Farm.
