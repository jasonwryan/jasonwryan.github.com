---
layout: post
title: "Hacking PKGBUILDs"
date: 2013-05-18 10:10
comments: true
categories: [archlinux, hack]
---
{% img left http://dl.dropbox.com/u/261312/Blog-images/woodcutting.jpg 'Cutting wood' %}
I posted a couple of weeks ago about 
[Building Vim](http://jasonwryan.com/blog/2013/03/29/vim/ 'Post on Vim') and how, 
using [ABS](https://wiki.archlinux.org/index.php/Abs 'Arch Build System on the Wiki')
and `makepkg` it is possible to customize packages in the repositories to suit your
individual requirements, in that case with a specific feature set.

One of Arch's real strengths is in the flexibility that `makepkg` and PKGBUILDs provide
the community; the ability to adapt official packages—or unofficial ones in the
[AUR](https://aur.archlinux.org/ 'Arch User Repository')—as you see fit. As PKGBUILDs
are just shell scripts, the entry level to start playing around with them is quite
low<sup>1</sup>.

A fairly standard, and simple, example of the type of customization that I might make
is with [dmenu](http://tools.suckless.org/dmenu/ 'dmenu page'), the suckless dynamic
menu, where the 
[standard package](https://www.archlinux.org/packages/community/x86_64/dmenu/ 'Arch package') 
in the Arch repositories is not patched for Xft support. There is a patch for this on the 
[suckless wiki](http://tools.suckless.org/dmenu/patches/xft 'Xft patch on suckless.org'), so it
is just a case of making the requisite changes in the PKGBUILD from ABS and building it.

As you can see from the `diff` below, there is not a lot involved in this exercize; essentially,
adding `libxft` as a dependency, sourcing the patch from the suckless site (and including the
hash for it), and then in the `build` function ensuring that the patch is applied and the
Makefile updated with the new library:

{% codeblock lang:diff %}
--- PKGBUILD   2013-05-18 09:33:07.156328812 +1200
+++ PKGBUILD   2012-11-14 09:25:15.915335588 +1300
@@ -11,16 +6,22 @@
 pkgdesc="A generic menu for X"
 url="http://tools.suckless.org/dmenu/"
 arch=('i686' 'x86_64')
+groups=('modified')
 license=('MIT')
-depends=('sh' 'libxinerama')
-source=(http://dl.suckless.org/tools/$pkgname-$pkgver.tar.gz)
-md5sums=('9c46169ed703732ec52ed946c27d84b4')
+depends=('sh' 'libxinerama' 'libxft')
+source=(http://dl.suckless.org/tools/$pkgname-$pkgver.tar.gz
+http://tools.suckless.org/dmenu/patches/$pkgname-$pkgver-xft.diff)
+md5sums=('9c46169ed703732ec52ed946c27d84b4'
+         'd448ec9120718b0aedbdb338f4fa69ba')

 build(){
   cd $srcdir/$pkgname-$pkgver
+  patch -p1 < ../$pkgname-$pkgver-xft.diff
+  sed -i 's:-I/usr/local/include/freetype2:-I/usr/include/freetype2:' config.mk
+
   make \
{% endcodeblock %}

Running `makepkg -i` will build and install dmenu with Xft support. This is the most
straightforward approach. I also, primarily by way of experimentation and in an effort
to try an understand how this actually works, have slightly more convoluted examples.
[msmtp](https://www.archlinux.org/packages/extra/x86_64/msmtp/ 'Arch package'), the
<acronym title="Simple Mail Transfer Protocol">SMTP</acronym> client has a couple
of makedepends in 
[libgnome-keyring](https://www.archlinux.org/packages/extra/x86_64/libgnome-keyring/ 'Arch package')
and [texlive-core](https://www.archlinux.org/packages/extra/any/texlive-core/ 'Arch package'); the former
I have zero use for and the latter is only installed on my desktop, so I have no wish to install it on
my laptop just to be able to send emails…

In this case, I modified the PKGBUILD to completely remove the `libgnome-keyring` dependency
and to only build the `msmtp` documentation in `.pdf` and `.html` if `texlive-core` was
already installed on the machine. Unfortunately, I wasn't able to test for the presence of
`texlive-core` with the standard utilities like `type` or `which`, so—as it is installed
on all my boxes—I went with `expac` (`pacman -Q` would also work):

{% codeblock lang:diff %}
--- PKGBUILD    2013-05-18 09:32:07.393095131 +1200
+++ PKGBUILD    2013-05-18 09:31:55.449986364 +1200
@@ -1,7 +1,8 @@
 arch=('i686' 'x86_64')
+groups=('modified')
 license=('GPL3')
 url="http://msmtp.sourceforge.net"
-makedepends=('texlive-core' 'gsasl' 'libgnome-keyring')
+makedepends=('gsasl')
 source=(http://download.sourceforge.net/sourceforge/msmtp/${pkgbase}-${pkgver}.tar.bz2)
 sha1sums=('c0edce1e1951968853f15209c8509699ff9e9ab5')

@@ -12,19 +13,24 @@

 build() {
   cd ${pkgbase}-${pkgver}
-  ./configure --prefix=/usr --sysconfdir=/etc --with-ssl=gnutls
+  ./configure --prefix=/usr --sysconfdir=/etc --with-ssl=gnutls --without-gnome-keyring
   make
-  make -C doc html pdf
+  if [[ -n $(expac -Q '%n' texlive-core) ]]; then
+      make -C doc html pdf
+  fi
 }

 package_msmtp() {
   pkgdesc="A mini smtp client"
-  depends=('gsasl' 'libgnome-keyring')
+  depends=('gsasl')
   install=msmtp.install

   cd ${pkgbase}-${pkgver}
   make DESTDIR="${pkgdir}" install 
+
+  if [[ -n $(expac -Q '%n' texlive-core) ]]; then
   make DESTDIR="${pkgdir}" -C doc install-html install-pdf
+  fi

 # Installing example configs and scripts to /usr/share/doc/msmtp
{% endcodeblock %}

It isn't necessarily an attractive solution, but it works for me…
On the subject of unattractive solutions, as of pacman 4.1, released 
last month, the packaging standards for 
[VCS PKGBUILDs](https://wiki.archlinux.org/index.php/VCS_PKGBUILD_Guidelines 'Arch Wiki page')
have been changed, principally around how sources and versioning is handled. For
the couple of VCS packages I maintain in the <acronym title="Arch User
Repository">AUR</acronym><sup>2</sup>, I have been 
experimenting with how to capture the `pkgver` in a way that conforms to the
standards and provides people with a meaningful version number.

By default, the version number for these projects from their git repos 
is not that helpful:

{% codeblock lang:sh %}
git describe --always
4861046
{% endcodeblock %}

After looking through the git logs, and playing around with `awk` to filter
the results, I came up with this:

{% codeblock lang:sh %}
pkgver() {
  cd "$_gitname"
  printf '%s\n' "$(awk '/^ / {print $2}' <(git log --grep=version -1))_\
  $(git describe --always)"
}
{% endcodeblock %}

This prints a more, for me anyway, intelligble package version: `vimprobable2-git
1.2.1_c5936cc-1` that relates back to the last stable release and appends the 
current commit. I'm sure that this could be improved upon; suggestions are 
welcome.

The other change to note in all of these PKGBUILDs is the inclusion of the
`groups` variable. By adding all of the modified packages from the official
repositories to the—imaginatively titled—`modified` group, I can then add a line
to `/etc/pacman.conf` that prevents those packages from being overwritten on
upgrade<sup>3</sup>:

{% codeblock lang:sh %}
IgnoreGroup = modified
{% endcodeblock %}

Issuing `pacman -Syu`, or running `checkupdates` from a `cron` job, will
notify you that the packages have had a version bump and that they need 
to be rebuilt. The PKGBUILDs for all of these packages can be found in
[my bitbucket repo](https://bitbucket.org/jasonwryan/centurion/src/ff3b3c9d805e197f35aa28dbeb6a9a6555ee7b63/Build?at=default
'Files in bitbucket').

#### Notes
1. As my experiments attest…
2. [Vimprobable2-git](https://aur.archlinux.org/packages/vimprobable2-git/),
[Surfraw-git](https://aur.archlinux.org/packages/surfraw-git/) and
[ruby-build-git](https://aur.archlinux.org/packages/ruby-build-git/).
3. All credit to ataraxia [for this idea](https://bbs.archlinux.org/viewtopic.php?pid=623841#p623841).

Creative Commons image on Flickr by 
[Wells P. Wilson](http://www.flickr.com/photos/wellspwilson/6481217091/ 'Wood
Cutting Wood')
