---
layout: post
title: "Tyrs: ncurses Twitter Client"
date: 2011-05-24 20:17
comments: true
categories: [linux]
---
[![image](/images/post_images/tyrs.png)](http://www.flickr.com/photos/jasonwryan/5754227102/in/photostream "Tyrs photo on Flickr")

I have been a solid user of
[TTYtter](http://www.floodgap.com/software/ttytter/ "TTYtter homepage")
for a couple of years now, much preferring the speed and power of a
command line Twitter client. And, after having tried–at one time or
another–most of the Linux or cross-platform Twitter clients, I can
safely say that for me TTYtter is easily the best.

There is, however, a new
[ncurses](http://www.gnu.org/software/ncurses/ "ncurses page") client
that is showing a lot of promise:
[Tyrs](http://tyrs.nicosphere.net/index.html "Tyrs homepage"). Only at
the 0.2.1.1 stage, and with a fraction of the functionality that TTYtter
has, it is nevertheless a very nice client.

Requiring little in the way of dependencies, only python-twitter and
python-oauth2, both available in the
[AUR](http://aur.archlinux.org/ "Arch Linux User Repository") or in the
debian/Ubuntu repos (if you are using debian/Ubuntu, you will need the
newer python-twitter-0.8.2 for it to run).

Tyrs is easy to configure, with good documentation and a simple
configuration file, located by default at
$XDG\_CONFIG\_HOME/tyrs/tyrs.cfg. The default keybinds are
[Vim-centric](http://www.vim.org/ "THE editor"), naturally…

My config is pretty straightforward at this point:

{% codeblock lang:vim %}
[keys]
retweet           = p
retweet_and_edit  = P
mentions          = a
reply             = r

[colors]
highlight         = 1
header            = 6
hashtag           = 4
attag             = 2
text              = 7
warning_msg       = 1
info_msg          = 2

[params]
refresh           = 1
tweet_border      = 0
{% endcodeblock %}

Written in Python, the code is well commented and relatively easy to
find your way around if your are keen on hacking.

The developer, Nicolas Paris, is both approachable and helpful and is
actively developing Tyrs. With some more functionality in the pipeline,
it looks to be a very nice application.
