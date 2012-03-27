---
layout: post
title: "Script to Edit Config Files"
date: 2011-07-19 19:34
comments: true
categories: [linux, bash, hack]
---
![image](http://dl.dropbox.com/u/261312/Blog-images/xdg-script.png)

Even with bash completion, endlessly typing and tabbing through the
directory tree to make a simple change to a config file quickly becomes
tedious, so I concocted this script to make it somewhat more
straightforward.

{% codeblock xdg.sh %}
#!/bin/bash
dir=$XDG_CONFIG_HOME/$1
if [ -d "$dir" ]; then 
    for f in $dir/* ; do
    file=${f##*/}
        case $file in
        conf | config | *.cfg | *rc)  $EDITOR "$f" ;;
        *)  :  ;;
        esac
    done
fi
{% endcodeblock %}

A relatively simple script, with a nice feature or two, it does a couple
of things. First, it
[tests](http://wiki.bash-hackers.org/commands/classictest "Bash Hackers: test")
that the directory actually exists:

{% codeblock lang:sh %}
    if [ -d "$dir" ]
{% endcodeblock %}

and then loops through all of the files in the directory using a
[glob](http://mywiki.wooledge.org/glob "Entry on Greg's Wiki") and
checks for a pattern match.

In order for the 
[case statement](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_07_03.html "Cases on Bash Guide")
to work, however, the directory path has to be stripped from the file
name.

Rather than run another process with
[basename](http://www.gnu.org/software/coreutils/manual/html_node/basename-invocation.html "GNU basename in coreutils"),
I used 
[parameter expansion](http://wiki.bash-hackers.org/syntax/pe#substring_removal "Bash Hackers Wiki")
to remove the substring:

{% codeblock lang:sh %}
    file=${f##*/}
{% endcodeblock %}

which turns, for example, <span class="file">$XDG\_CONGIG\_HOME/newsbeuter/conf</span> into a
simple conf. This can then be evaluated for a match with the file names
in the case statement and passed to the `$EDITOR`.

If no match is found, nothing (`:`) is done.

Now, it is simply a matter of entering `xdg vimprobable` and
<span class="file">$XDG\_CONFIG\_HOME/vimprobable/vimprobablerc</span> is opened in Vim.

#### Updated 19/10/11

After using this script for a while it became clear to me that it has a
significant drawback: too many applications place their config files in
directories other than <span class="file">$XDG\_CONFIG\_HOME</span>. So, 
[with some help](https://bbs.archlinux.org/viewtopic.php?id=128585 "Post on the Arch boards"),
I updated the script. It now covers all of the directories where you are
likely to find dotfiles.

{% codeblock xdg.sh v2 %}
#!/bin/bash
dirs=($HOME/.$1* $HOME/.$1/ $XDG_CONFIG_HOME/$1/)
IFS=$'\n' 
read -r -d '' -a files < \
    <(find "${dirs[@]}" -type f \( \
           -name "*.conf" \
        -o -name "conf" \
        -o -name "config" \
        -o -name "*rc" \
        -o -name "*.cfg" \) 2>/dev/null)

(( ${#files[*]} )) && "$EDITOR" "${files}"
{% endcodeblock %}

If you would prefer a more portable version—which still relies on GNU
find—then you could simplify it like so:

{% codeblock xdg.sh v3 %}
#!/bin/sh
dirs=($HOME/.$1* $HOME/.$1/ $XDG_CONFIG_HOME/$1/)

find "${dirs[@]}" -type f \( \
       -name "*.conf" \
    -o -name "conf" \
    -o -name "config" \
    -o -name "*rc" \
    -o -name "*.cfg" \) \
    -exec "$EDITOR" {} +
{% endcodeblock %}

