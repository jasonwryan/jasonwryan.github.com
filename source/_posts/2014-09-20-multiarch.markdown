---
layout: post
title: "Multiarch Packages in AUR"
date: 2014-09-20 09:16
comments: true
categories: [archlinux, arm, hack]
---
{% img left /images/post_images/agra.jpg Creative Commons image of Agra %}
One of the easiest ways to contribute to Arch is to maintain a package, or
packages, in the 
<acronym title="Arch User Repository">[AUR](https://aur.archlinux.org/ 'AUR homepage')</acronym>;
the repository of user contributed PKGBUILDs that extends the range of packages available 
for Arch by some magnitude. Given that PKGBUILDs are just shell scripts, the barrier
to entry is relatively low, and investing the small amount of effort required to clear
that barrier will not only give you a much better understanding of how packaging works
in Arch, but will scratch your own itch for a particular package and hopefully assuage
someone else's similar desire at the same time.

Now that I have a 
[Raspberry Pi](http://www.raspberrypi.org/ 'Raspberry Pi site')<sup>1</sup>, I am 
naturally much more interested in packages that can be built for the ARMv6
architecture; especially those that are available in the AUR. It is worth a brief 
digression to note that 
[Arch Linux ARM](http://archlinuxarm.org/ 'AL ARM home page') is an entirely 
*separate* distribution and, while they share features with Arch, support for
each is restricted to their respective communities. It is with this
consideration in mind that I had begun to think about multiarch support in
PKGBUILDs, particularly in the packages that I maintain in the AUR. 

I have
[previously posted](http://jasonwryan.com/blog/2014/05/10/syncthing/ 'Post on Syncthing')
about using [Syncthing](http://syncthing.net/ 'Open source synching: genius…')
across my network, including on a Pi as one of the nodes. As the Syncthing
developer pushes out a release at least weekly, I have been maintaining my own
PKGBUILD and, after Syncthing was pulled into [community], I uploaded it to the
AUR as
[syncthing-bin](https://aur.archlinux.org/packages/syncthing-bin 'AUR package'). 

Syncthing is a cross platform application so it runs on a wide range of
architectures, including ARM (both v6 and v7). Initially, when I wrote the
PKGBUILD, I would `updpkgsums` on my x86_64 machine, build the package and
then, on the Pi, have to regenerate the integrity checks. This was manageable
enough for my own use across two architectures, but wasn't really going to
work for people using other architectures (especially if they are using
[AUR helpers](http://jasonwryan.com/blog/2013/04/09/helpers/ 'My post on Yaourt')). 

Naturally enough, this started me thinking about how I could more effectively
manage the process of updating the PKGBUILD for each new release, *and* have it
work across the four architectures—without having to manually copy and paste or
anything similarly tedious. Managing multiple architectures in the PKGBUILD
itself is not particularly problematic, a case statement is sufficient:

{% codeblock lang:sh PKGBUILD %}
case "$CARCH" in
    armv6h) _pkgarch="armv6"
            sha1sums+=('a94e5d00cec32956eb27bc12dbbc4964b68913f9')
           ;;
    armv7h) _pkgarch="armv7"
            sha1sums+=('9b782abf95668a906bfe76ad5ceb4cda17ec2289')
           ;;
    i686) _pkgarch="386"
          sha1sums+=('b2e1961594a931201799246f5cf61cb1e1700ff9')
           ;;
    x86_64) _pkgarch="amd64"
            sha1sums+=('035730c09ca5383c90fdd9898baf66b90acdef24')
           ;;
esac
{% endcodeblock %}

The real challenge, for me, was to be able to script the replacement of each of
the respective 
[sha1sums](http://en.wikipedia.org/wiki/Sha1sum 'Wikipedia entry'), 
and then to update the PKGBUILD with the new arrays.  Each release of
Syncthing is accompanied by a text file containing all of the sha1sums, each on
its own line in a conveniently ordered format, like so:

{% codeblock lang:text sha1sums.txt.asc %}
b2e1961594a931201799246f5cf61cb1e1700ff9    syncthing-linux-386-v0.9.16.tar.gz
035730c09ca5383c90fdd9898baf66b90acdef24    syncthing-linux-amd64-v0.9.16.tar.gz
d743b64204f0ac7884e4b42d9b1865b2436f5ecb    syncthing-linux-armv5-v0.9.16.tar.gz
…
{% endcodeblock %}

This seemed a perfect job for Awk, or more particularly, `gawk`'s 
[switch statement](https://www.gnu.org/software/gawk/manual/html_node/Switch-Statement.html 'Gawk manual'),
and an admittedly rather convoluted `printf` incantation.

{% codeblock lang:awk %}
    switch ($2) {
      case /armv6/:
        arm6 = $1
        break
      case /armv7/:
        arm7 = $1
        break
      case /linux-386/:                                                                       
        i386 = $1
        break
      case /linux-amd64/:
        x86 = $1
        break
      }
  }
END {
  printf "case \"$CARCH\" in\n\t"\
         "armv6h) _pkgarch=\"armv6\"\n\t\tsha1sums+=(\047%s\047)\n\t\t;;\n\t"\
         "armv7h) _pkgarch=\"armv7\"\n\t\tsha1sums+=(\047%s\047)\n\t\t;;\n\t"\
         "i686) _pkgarch=\"386\"\n\t\tsha1sums+=(\047%s\047)\n\t\t;;\n\t"\
         "x86_64) _pkgarch=\"amd64\"\n\t\tsha1sums+=(\047%s\047)\n\t\t;;\n"\
         "esac\n",
         arm6, arm7, i386, x86
}
{% endcodeblock %}

The remaining step was to update the PKGBUILD with the new sha1sums. Fortunately,
[Dave Reisner](http://blog.falconindy.com/ 'Dave's blog') had already written the code
for this in his `updpkgsums` utility; I had only to adapt it slightly:

{% codeblock lang:awk excerpt from updpkgsums %}
{
  rm "$buildfile"
  exec awk -v newsums="$newsums" '
    /^case/,/^esac$/ {
      if (!w) { print newsums; w++ }
        next
      }; 1
      END { if (!w) print newsums }
  ' > "$buildfile"
} < "$buildfile"
{% endcodeblock %}

Combining these two tasks means that I have a script that, when run, will download
the current Syncthing release's <span class="file">sha1sum.txt.asc</span>
file, extract the relevant sums into the replacement case statement and then
write it into the PKGBUILD. I can then run `makepkg -ci && mkaurball`, upload
the new tarball to the AUR and the two other people that are using the PKGBUILD
can download it and not have to generate new sums before installing their
shiny, new version of Syncthing. You can see the full version of the script in 
[my bitbucket repo](https://bitbucket.org/jasonwryan/shiv/src/tip/Scripts/upsync 'Tip…').

#### Notes
1. See my other [posts about the Pi](http://jasonwryan.com/blog/categories/arm/)…

Creative Commons image of the Mosque at Agra, by 
[yours truly](https://www.flickr.com/photos/jasonwryan/14986166957/). 
