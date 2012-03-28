---
layout: post
title: "Pinboard Elvis"
date: 2011-10-01 10:29
comments: true
categories: [surfraw, linux, bash]
---
![image](http://dl.dropbox.com/u/261312/Blog-images/pinboard-elvis.png)

After recovering from 
[the shock of the botched delicious relaunch](http://jasonwryan.com/blog/2011-09-28-delicious/ "My post on the delicious fiasco"),
I have switched to
[Pinboard](http://pinboard.in/u:jasonwryan/ "My bookmarks on Pinboard")
for my bookmarking. Previously, I was just synching my bookmarks from
delicious, but now I have abandoned that trainwreck completely.

Two things made the transition painless. The first was the 
[Pinboard Android app](http://www.appbrain.com/app/me.mitnworb.save.to.pinboard/ "Save to Pinboard page")
so I could access and manage bookmarks from my phone; the second was a
little trickier.

As I have previously mentioned, I use bookmarking not just to retrieve
my own saved pages, but as a search repository for material that other
people have decided is interesting or valuable enough to want to save.
And there is no better tool for this sort of searching than
[Surfraw](http://surfraw.alioth.debian.org/ "Surfraw - Shell Users' Revolutionary Front Rage Against the Web")<sup>1</sup>

I had written an elvis to search delicious bookmarks, so to complete the
transition to Pinboard, I needed to do the same. It was a little
trickier than I had anticipated, as some of the URLs (particularly when
searching across multiple tags) were reasonably complex given my modest
scripting skills. But 
[I got there in the end…](http://beta.intuxication.org/jasonwryan/archer/file/tip/.config/surfraw/elvi/pin "Pinboard elvis in mercurial repo")

Now searching for web pages that I or anyone else has saved to Pinboard
is as simple as typing:

{% codeblock lang:sh %}
pin -t=unix+linux+vim
{% endcodeblock %}

Which will return all saved pages that share those three tags. I can
also refine my search to a particular user with the `-u`{ser} switch, or
just browse recent or popular tags. Simple, but ruthlessly efficient…

##### Notes
1. Previous posts on Surfraw:
    -   [Arch Wiki elvis for Surfraw](http://jasonwryan.com/blog/2011-04-13-archwiki-elvis-for-surfraw/ "My post on the Arch Wiki elvis")
    -   [w3m, Surfraw and yanking  URLs](http://jasonwryan.com/blog/2011-05-05-w3m/ "Yanking links in w3m")

