---
layout: post
title: "Microserver"
date: 2011-11-18 17:13
comments: true
categories: [debian, linux, bash]
---
![image](http://dl.dropbox.com/u/261312/Blog-images/microserver.png)

I recently picked up a 
[HP Microserver](http://h10010.www1.hp.com/wwpc/us/en/sm/WF05a/15351-15351-4237916-4237918-4237917-4248009.html "HP specs page");
a small, silent server to use primarily to back up 
[my other boxes](http://jasonwryan.com/blog/2010-10-04-the-setup "Slightly out-of-date setup post").
I went for the 2GB version without any preinstalled operating system.

As I want this machine to just sit quietly in my garage for the next 5
or 6 years, I opted for 
[Debian stable](http://www.debian.org/releases/stable/ "Debian site") as the
base system. After installing two 1TB drives, I set them up as an
[encrypted](http://www.saout.de/misc/dm-crypt/ "Device mapper homepage")
RAID 1 array, in a i
[logical volume](http://en.wikipedia.org/wiki/Logical_volume_management "Wikipedia entry on LVM").

**Pro tip** when you get to the part of the partitioning where you
configure the volume for encryption, make sure you select *No* for
*erase data*. It took close to 90 hours to scrub both the hard drives.
And, as I am not really expecting 
[a visit from the spooks](http://www.extremetech.com/computing/105931-full-disk-encryption-is-too-good-says-us-intelligence-agency "FDE defeats the NSA"),
that seems a little like overkill…

Once the base install was complete, I set up my SSH keys,
[tmux](http://tmux.sourceforge.net/ "tmux page"), configured the
firewall and forwarded the necessary ports, set up
[NFS](http://en.wikipedia.org/wiki/Network_File_System_%28protocol%29 "Wikipedia page: network file system"),
and that was it. The box is deathly quite, can be mounted locally for
[rsync backups](http://en.wikipedia.org/wiki/Rsync "Wikipedia rsync entry") and
accessed remotely (for a tmux session running
[irssi](http://irssi.org/ "Irssi homepage")), and all for less than
NZD700.

I knocked together a quick script^1 to manage the backing up; currently
I run it manually, but once I have the excludes file tweaked properly,
I’ll set up a 
[cron job](http://en.wikipedia.org/wiki/Cron "Wikipedia page on cron") to
automate the task.

{% codeblock runbackup.sh %}
#!/bin/sh
# Backup to remote server over NFS

if [ $(id -u) -ne 0 ]; then
     printf '%s\n' "You must run this as root. Terminating."
     exit 1
fi
 
if [ -d /media/Sentinel/Backups ]; then 
     printf '%s\n' "Sentinel mounted."
 else
     printf '%s\n' "Mounting Sentinel."
     mount.nfs 192.168.1.200:/home/jason /media/Sentinel || exit 1
 fi
 
# sync directories
 printf '%s\n' "Starting sync..."
 
 rsync -azP --delete --exclude-from=Scripts/excludes.txt \
     --log-file=Documents/rsync.log \
     /home/jason /etc /media/Sentinel/Backups/Centurion
     
 if [ "$?" -eq 0 ]; then
     printf '%s\n' "Synched successfully; now unmounting."
     umount /media/Sentinel
 else
     printf '%s\n' "Fail!"
 fi
{% endcodeblock %}

##### Notes
1. Any faults aside, it should be reasonably portable…
