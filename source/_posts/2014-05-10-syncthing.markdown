---
layout: post
title: "Using Syncthing"
date: 2014-05-10 09:30
comments: true
categories: [archlinux, backup] 
---
{% img left http://miromiro.com/Blog-images/syncthing.png Syncthing logo %}
Just over a year ago, I was invited to participate in an exciting Alpha, for
[BitTorrent Sync](http://www.bittorrent.com/sync 'Sync website), an application
that allows you to sync selected directories across devices using a 
[peer-to-peer protocol](https://en.wikipedia.org/wiki/Bittorent 'Wikipedia article').
I [wrote up my experience](http://jasonwryan.com/blog/2013/03/16/sync/ 'Click the damn thing…')
and, with one reservation, found it to be a fantastic tool that did pretty much everything I
needed, including letting me finally get rid of Dropbox. That one reservation, although not
entirely clear at the time, was clarified soon after and was crushingly disappointing: 
the programme was closed source.

Due to a combination of inertia (it was installed on all my devices and working well) and
the lack of an alternative that didn't involve installing and running a 
[LAMP stack](https://en.wikipedia.org/wiki/Lamp_stack 'Wikipedia page') just to sync
my files, I have persisted with it; even writing a 
[simple Awk script](http://jasonwryan.com/blog/2013/11/14/api/ 'Post on said script')
to interact with the <acronym title="Application Program Interface">API</acronym>.
For the last twelve months I have been pretty happy with it. 

In addition to the licensing choice, there are some other small issues that I have discovered: the 
[Android App](https://play.google.com/store/apps/details?id=com.bittorrent.sync 'On Google Play')
*chews* through your battery if it is left on (which sort of defeats the purpose, but I
learned to live with it), the syncing can take a little while to pick up changes, and 
nodes that don't see much action may flake out completely after a while.

Then, a few weeks ago I read about [clearskies](https://github.com/jewel/clearskies 'On Github'),
an open source implementation of this concept. Sadly, it is still a proof of
concept. This discovery, however, led to an extremely fortuitous exchange
[on Twitter](https://twitter.com/csantosb/status/460714748322258944 'Twitter conversation')
with [cayetano santos](https://twitter.com/csantosb 'His profile on Twitter') who pointed me
at [Syncthing](http://syncthing.net/ 'Syncthing homepage'). Described thus (cue 
[background music](http://www.youtube.com/watch?v=7YaGwI7GjlA 'G.F. Handel on Youtube')):

{% blockquote %}
Syncthing replaces Dropbox and BitTorrent Sync with something open, trustworthy and decentralized. Your data is your data alone and you deserve to choose where it is stored, if it is shared with some third party and how it's transmitted over the Internet.
{% endblockquote %}

Written in [Go](http://golang.org/ 'Go language site'), Syncthing is still in 
early development, but judging by the 
[release history](https://github.com/calmh/syncthing/releases 'Releases on Github'),
it is moving ahead quickly. The developer, [Jakob Borg](https://nym.se/ 'Jakob's site'), 
is obviously both talented and productive. After using it for the last 
couple of weeks, I have uninstalled BitTorrent Sync from all my machines 
and have Syncthing set up and running nicely. It is simple, elegant and 
works wonderfully; *for what it does*.

Which is to say that it is not all of the things that either of its
predecessors are: it isn't intended to distribute files, or to host images or
simple websites. It doesn't include versioning or have a built-in editor. It is
for securely syncing your files independent of any third parties. And there is no 
Android app, so your battery is safe<sup>1</sup>.

Setup is through a tidy, [bootstrap](http://getbootstrap.com/ 'Bootstrap site')
built web interface, by default running on `127.0.0.1:8080`. I subsequently found
that once I understood how the configuration worked, editing the config file at
<span class="file">$XDG_CONFIG_HOME/syncthing/config.xml</span> was much
simpler.  When setting up, you will also do a lot of stopping and starting of
`syncthing`; if you use the service file that comes with the 
[AUR package](https://aur.archlinux.org/packages/syncthing/ 'Arch User Repo'), 
that won't work so well from the web interface… Run it from the command line 
with one of the `STTRACE` variables to debug and then, once it is running to
your liking, hand it off to systemd<sup>2</sup>.

With a reasonably tricky cluster setup (six machines; two outside the 
<acronym title="Local Area Network">LAN</acronym> sharing half a dozen 
repositories with three different topologies), there is a fair bit of trial and
error involved getting everything set up and working correctly. But when you
do, it is much faster to pick up changes than either of the other two and there
is none of the risk around privacy and security presented by Dropbox and—to a
(much, I would hazard to guess) lesser extent—BitTorrent Sync.

I have also found that it is much quicker for all of the nodes connect if,
for the LAN connections in your cluster, you specify the 
<acronym title="Internet Protocol">IP</acronym> addresses and port and disable
global discovery for those nodes: this speeds up the discovery time when
the nodes are started. Nodes still, from time-to-time, flake out, particularly
if one of them is rebooted. Restarting the offending node is usually sufficient
to bring the cluster back into sync.

That isn't to say that there aren't any issues with it. The documentation is in
need of some work; your best bet at the moment is a combination of the rather
terse `--help` message and reading the source. There are also some hidden gems
in the Github commits; for example, details on using 
<span class="file">.stignore</span> files, a very handy feature, are buried in
an earlier 
[commit to README.md](https://github.com/calmh/syncthing/commit/986b15573a66f95beae53b10371ba50ab147124a 'Github commit') that didn't survive the move of the documentation to the 
forum.

The only other significant issue I have encountered is that getting all of the
nodes across the cluster to connect to each other at the same time seems to be
inexplicably difficult. It may just be my setup, but more often than not, nodes
(on the LAN) will timeout while trying to connect to at least one of their peers.
This isn't a huge issue as you only really need one chained connection to bring
all the data up to date, but it does cause a something of a minor
<acronym title="Obsessive Compulsive Disorder">OCD</acronym> flare up. 

Given the early stages of the project, though, and the fact that it is open
source, these anomalies are trivial. Overall, Syncthing is a delight. It does
exactly what I need it to do, and it does it securely. Given the promise it is
showing this early, I can only imagine it getting a lot better over time as
more 
[bugs are squashed](https://github.com/calmh/syncthing/issues?state=open 'Issues on Github')
and features rolled out.

#### Notes
1. Naturally enough, someone is [on the case](https://github.com/calmh/syncthing/issues/177)…
1. I added a debian daemon script to a thread on 
[the Syncthing boards](http://discourse.syncthing.net/t/keeping-syncthing-running/30/2) 
as there is no built in daemon functionality (yet).
