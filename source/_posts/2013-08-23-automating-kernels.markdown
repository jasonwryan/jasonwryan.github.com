---
layout: post
title: "Automating Kernel Builds"
date: 2013-08-24 10:30
comments: true
categories: [archlinux, bash, hack, kernel]
---
{% img left /images/post_images/robot.jpg I Robot %}
I posted a couple of weeks ago about 
[compiling a custom kernel](http://jasonwryan.com/blog/2013/08/03/kernels/ 'Check it if you haven't...')
using [modprobed_db](https://github.com/graysky2/modprobed_db 'graysky's github repo')
and at the time I talked about automating the process, essentially so that every
kernel update, be it the standard Arch kernel or the stripped down custom one,
would be a simple, streamlined process. I updated that post with 
[a PKGBUILD](https://bitbucket.org/jasonwryan/shiv/src/tip/Build/linux-jwr/PKGBUILD 'bitbucket repo'),
which was the first step. This completes that process.

There is not a lot to it. A 
[short bash wrapper](https://bitbucket.org/jasonwryan/shiv/src/tip/Scripts/new_kernel 'bitbucket repo')
that sets up the required files, updates the aforementioned PKGBUILD and then
triggers `makepkg` (providing nothing else fails). All it requires is that you
have 
[the necessary files](https://bitbucket.org/jasonwryan/shiv/src/tip/Build/linux-jwr 'You guessed it…) 
in a separate directory for the script to reference and
you are set. If anything does go wrong with the new kernel, you still have the
previous working custom kernel `.tar.xz` you can reinstall, or you can of course
just boot into the vanilla Arch kernel.

{% codeblock lang:bash new_kernel %}
#!/usr/bin/env bash
# install new kernel

vers=$1
cyn=$'\e[1;36m'
ylw=$'\e[1;33m'
red=$'\e[1;31m'
end=$'\e[0m'

if [[ $# != 1 ]]; then
	printf '%s\n' "Usage: "${0##*/}" 3.10.8"
	exit 1
fi

# back up last dir
cd ~/Build
rm -rf linux-jwr/{pkg,src}
mv linux-jwr linux-${vers%.*}.$((${vers##*.} - 1))

# set up clean files
cp -r kernel_files linux-jwr && cd "$_"

sed -i "s:pkgver=[0-9].*:pkgver=${vers}:" PKGBUILD
newvers=$(awk -F= '/pkgver=/ {print $2}' PKGBUILD)
printf '%s\n' "Package version updated to ${ylw}$newvers${end}"

printf '%s\n' "${cyn}Updating checksums…${end}"
/usr/bin/updpkgsums

if [[ $? -eq 0 ]]; then
	printf '%s\n' "${cyn}Starting build…${end}"
	/usr/bin/time /usr/bin/makepkg -i
else
	printf '%s\n' "${red}Checksum failed${end}"
	exit 1
fi
cd
{% endcodeblock %}

That's all there is to it. I see in my RSS reader that a new stable kernel is
available, open a terminal and run the script specifying which version to use,
for example, `new_kernel 3.10.9`.  Approximately 10 minutes later, my new kernel
is compiled and installed and I can reboot into it. Simple.

Undoubtedly, there is probably a cleaner way to do this, but it has worked well
for the last couple of kernels. It will probably also require tweaking for the
next series, but until that time, I am quite happy with it.

#### Notes
Flickr creative commons image 
[My robot](http://www.flickr.com/photos/fringley/4244586360/) by fringley.
