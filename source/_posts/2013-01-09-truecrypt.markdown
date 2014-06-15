---
layout: post
title: "Replacing TrueCrypt"
date: 2013-01-10 16:02
comments: true
categories: [archlinux, bash, hack]
---
{% img left http://miromiro.com/Blog-images/lock.jpg 'Flickr image of lock' %} 
I have used [TrueCrypt](http://truecrypt.org/ 'TrueCrypt homepage') since just before 
I migrated to Linux, so somwehere around six or seven years, dating back to version 3 or 4. 
It is an incredibly handy utility, allowing you to create and manage encrypted volumes on 
the fly and—for the especially paranoid—to hide the existence of those volumes from 
inquisitive others. 

In the intervening years, with all aspects of my personal life 
increasingly mediated digitally, in order to properly safeguard my privacy, 
and that of my family, I have taken to using LUKS to 
[fully encrypt all of my machines](http://jasonwryan.com/blog/2012/02/11/lvm/ 'Post on LVM on LUKS').
And I have used TrueCrypt, both personally and for work, to encrypt some of my USB drives and as 
a container in [Dropbox](http://dropbox.com/ 'Dropbox homepage')
, for as long as 
[I have used that service](http://jasonwryan.com/blog/2010/01/11/using-dropbox-to-share-dotfiles/ 'Post on sharing dotfiles with Dropbox').

Early last year, however, I became aware of concerns that TrueCrypt was not truly open source; that almost
all of the larger distros, including Arch, and the Open Source Initiative did not regard TrueCrypt as
free (as in Freedom) software:

{% blockquote Wikipedia entry on TrueCrypt https://en.wikipedia.org/wiki/Truecrypt %}
The TrueCrypt License has not been officially approved by the Open Source Initiative and is not considered "free" by several major Linux distributions (Arch Linux, Debian, Ubuntu, Fedora, openSUSE, Gentoo), mainly because of distribution and copyright-liability reasons.
{% endblockquote %}

When I initially accessed the page, in February 2012, there was also a paragraph—now removed—that
highlighted further, more alarmist, concerns about the shadowy identity of the people behind
TrueCrypt:

{% blockquote Wikipedia entry retrieved in February 2012 https://en.wikipedia.org/w/index.php?title=TrueCrypt&diff=478623780&oldid=478608477 %}
The anonymity of the developers and the abnormalities mentioned above have led users to raise suspicions about the provenance of the product and speculate about the possible existence of vulnerabilities or backdoors that might exist in the source code or executables. http://www.privacylover.com/encryption/analysis-is-there-a-backdoor-in-truecrypt-is-truecrypt-a-cia-honeypot/ However its open source and it can be check for funerabilities that way. [sic]
{% endblockquote %}

While I don't subscribe to the theory that the CIA have planted a backdoor in the software (if they
had, [it clearly doesn't work](https://en.wikipedia.org/wiki/Truecrypt#Operation_Satyagraha 'FBI operation foiled by TC')), 
I was relieved to see at the end of last year that someone had written a simple utility that allows
you to manage TrueCrypt containers from the command line; [tcplay](https://github.com/bwalex/tc-play 'tcplay on Guthub')
is decribed as:

{% blockquote %}
a free (BSD-licensed), pretty much fully featured (including multiple keyfiles, cipher cascades, etc) and stable TrueCrypt implementation.
{% endblockquote %}

With a [simple, two paragraph license](https://github.com/bwalex/tc-play/blob/master/LICENSE 'tcplay license')
and a brief but comprehensive `man` page, I was sold. I uninstalled TrueCrypt late last year and haven't missed it since. Quite
the contrary. The only “issue” that I have had with tcplay is remembering the commands to map and mount a drive.
Initially, I jotted down some notes, but opening them up several times a week to refer to them quickly seemed
pointless so I eventually wised up and wrote a wrapper script to do the job for me…

The script is quite simple, it finds the first available loop device, maps the encrypted volume to it and
mounts it read-writeable for your user.

{% codeblock lang:sh %}
#!/bin/bash
# manage truecrypt containers using tcplay

user=jason
cryptdev=Safebox
cryptpath=/home/jason/Dropbox/"$cryptdev"
loopdev=$(losetup -f)
mountpt=/media/"$cryptdev"

# must be run as root
if (( $EUID != 0 )); then
  printf "%s\n" "You must be root to run this."
  exit 1
fi

# unecrypt and mount container
if [[ $1 == open ]]; then
  losetup "$loopdev" "$cryptpath"
  tcplay --map="$cryptdev" --device="$loopdev"
    
  # read passphrase
  read -r -s passphrase <<EOF
  "$passphrase"
EOF

  # mount container
  [[ -d $mountpt ]] || mkdir "$mountpt"

  # mount options
  userid=$(awk -F"[=(]" '{print $2,$4}' <(id "$user"))
  mount -o nosuid,uid="${userid% *}",gid="${userid#* }" /dev/mapper/"$cryptdev" "$mountpt"

# close and clean up…
elif [[ $1 == close ]]; then
  device=$(awk -v dev=$cryptdev -F":" '/dev/ {print $1}' <(losetup -a))
  umount "$mountpt"
  dmsetup remove "$cryptdev" || printf "%s\n" "demapping failed"
  losetup -d "$device" || printf "%s\n" "deleting $loopdev failed"
else
  printf "%s\n" "Options are open or close."
fi
{% endcodeblock %}

Once you are done, the script will unmount your volume and clean up. Undoubtedly, the 
script could be improved; patches are welcome.

There is a 
[PKGBUILD in the AUR](https://aur.archlinux.org/packages/tcplay-git/ 'Arch User Repository').
Uninstall TrueCrypt and give tcplay a go, it is a simple, powerful application; and it *is*
free software…

### Update
As cr notes in the comments, the `uid` and `gid` values are because when I created this container I
needed to ocassionally mount it from windows, so it is a FAT32 filesystem. For ext3 or ext4, you would use
`bindfs -u $user -g $group "$mountpt" "$backuppath"`.

#### Notes
Creative Commons image on Flickr by [xserv](http://www.flickr.com/photos/xserve/368758286/ 'Licensed CC by xserv').

