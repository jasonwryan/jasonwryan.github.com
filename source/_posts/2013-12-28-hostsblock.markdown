---
layout: post
title: "Ad Blocking with Hostsblock"
date: 2013-12-28 10:09
comments: true
categories: [archlinux, bash]
---
{%img left /images/post_images/ad_free.png Flickr CC Image %}
The relentless commercialization of traditional holidays, not just the December
variety but all of them now, means that what was ostensibly an occasion for
celebrating your particular flavour of $deity, pagan ritual or just an
opportunity to reconnect with your wider family, has been co-opted to the
worship of the most pernicious of all cults, consumerism.

Concomitant with this is the increasing corporatisation of the Internet; the
deliberate and seemingly ineluctable effort by a relatively small number of
global interests to turn most of the Internet into something more like cable
television. As someone who has not lived with a television for decades
precisely because I don't want to live in a 
[Skinner box](http://en.wikipedia.org/wiki/Operant_conditioning_chamber 'Wikipedia page')
that is solely designed to condition me to compliantly purchase more product,
I find this rankly offensive (in the sense of morally repugnant as well as a
coordinated and remorseless assault).

With the escalation of both the level and nauseating intensity of advertising
around the “holiday season” and the almost hysterical exhortations to purchase
more happiness, I decided that the one thing that I would be really thankful for
this Christmas was better ad blocking.

I had been using 
[Privoxy](https://wiki.archlinux.org/index.php/Privoxy 'Arch wiki entry') and an
<acronym title="Arch User Repository">AUR</acronym> script,
[blocklist-to-privoxy](https://aur.archlinux.org/packages/blocklist-to-privoxy/?ID=63431 'AUR page')
but I was still experiencing a lot of ads sneaking through (particularly local
ones) and some unintended side effects of using Privoxy for this job, so I
decide to give Jake VanderKolk's
[Hostsblock](http://gaenserich.github.io/hostsblock/ 'Homepage for Hostsblock')
a shot.

I went with the basic, entry level setup: 
[kwakd](https://aur.archlinux.org/packages/kwakd/ 'AUR page') for serving blank 
<acronym title="HyperText Markup Language">HTML</acronym> in place of ads and 
[hostsblock](https://aur.archlinux.org/packages.php?ID=58976 'AUR package') to
write to my <span class="file">/etc/hosts</span>. I didn't see the need for 
[dnsmasq](https://wiki.archlinux.org/index.php/Dnsmasq 'Arch Wiki page') and, 
after a week or so or using it haven't seen the need to revisit that decision.

Suffice to say, it is working brilliantly. Where once web pages were festooned
with garish advertisements promising me ripped abdominals, tropical holidays and the
lasting serenity that only Apple products can really truly deliver, I now have
[glorious whitespace](/images/post_images/herald.png 'Screenshot of the local rag…')

There are a couple of other factors to consider with Hostsblock. There is a very
simple command line interface for managing black and white listing of websites,
and the various “content distribution networks” that infest most commercial
sites like fleas. You can easily allow advertisements on or from sites and
organizations that you want to support, while forever muting the inane drivel
from the likes of Failbook *et al*.

Installation and setup are very straightforward, with simple instructions on the 
[Hostsblock site](http://gaenserich.github.io/hostsblock/ 'Two or three steps, max…').
There is also an active thread on the 
[Arch boards](https://bbs.archlinux.org/viewtopic.php?id=139784 'Arch forum thread') 
where Jake and a couple of others are extremely helpful.

If you are feeling listless, run-down and lacking in energy, why not try
Hostsblock? It will make your web pages brighter, speed up your page loads,
protect your privacy and make you insanely popular. Try Hostsblock today!

#### Notes
ad free, a Creative Commons image by 
[Louisa Billeter on Flickr](http://www.flickr.com/photos/louisa_catlover/2875951548/).
