---
layout: post
title: "Improved Notes Utility"
date: 2012-09-01 10:09
comments: true
categories: [archlinux, bash, hack]
---
{% img left /images/post_images/notes.jpg 'Notepad image on Flickr' %}
Nearly two years ago, I posted about my adaption of a simple
[command line note utility](http://jasonwryan.com/blog/2010/09/28/command-line-notes/ 'My post in 2010').
I have used this setup on all of my machines on a daily basis since and it has worked
marvellously. Symlinking to a folder in 
[Dropbox](https://www.dropbox.com/home 'Dropbox home') means that the notes are accessible
from all my machines, including my phone. There has only really been one aspect of this setup
that has been sub-optimal.

As an inveterate note-taker (this is one of the “benefits” of ageing; the speed with
which you forget information outpaces the acquisition of newer material) I have—in
those intervening years—built up quite a store of notes. Consequently, in order to
maintain a semblance of order, I have arranged them in a series of directories. There
is a minor flaw with this approach: retreiving a note depended on two factors, a) excellent
recall<sup>1</sup> and, b) accurately typing out the full path. Neither of these are things that I
am inherently good at or inclined to master.

This had been irritating me for some time before I came across this
[question on Unix &amp; Linux StackExchange](http://unix.stackexchange.com/questions/11906/how-to-change-the-target-directory-for-tab-completion 'Tab completion for notes function').
This provided me with a partial solution to the issue but, as I note in my answer, I was not 
able to solve it for nested directories, which was my particular use case. Once I had
muddled my way through the solution on U&amp;L I pushed it to the back of my mind and 
tried to ignore it.

Recently, though, the accumulation of notes and the frustration of trying to access them
without <kbd>Tab</kbd> completion drove me to do something about it. 

The documentation on 
[programmable completion](http://www.gnu.org/software/bash/manual/html_node/Programmable-Completion-Builtins.html 'Bash Manual')
is typically terse and searching the web returns very little in the way of instructions
as to how to accomplish this.<sup>2</sup> Undeterred, I decided to hack up a completion
function that worked for nested directories.

What I arrived at was this:
{% codeblock lang:sh %}
shopt -s globstar
shopt -s progcomp

n() { $EDITOR $HOME/.notes/"$*".txt ;}

 # completion for notes
_notes() {
local cur 
    cur=${COMP_WORDS[COMP_CWORD]}
    files=($HOME/.notes/**)
    file="${files[@]##*/}"
    COMPREPLY=( $( compgen -f "${file[@]}" -- ${cur} ) )
}
complete -o default -F _notes n
{% endcodeblock %}

The best that can be said about it is that it *nearly* works…<sup>3</sup> 

Realising that I was completely out of my depth, I turned to #bash for help, and
I was indeed fortunate that 
[geihra](https://github.com/geirha 'geirha's Gitgub') offered some much needed
assistance. geirha's solution is an elegant one:

{% codeblock lang:sh %}
n() { 
local arg files=(); for arg; do files+=( ~/".notes/$arg" ); done
${EDITOR:-vi} "${files[@]}" 
}

_notes() {
local files=($HOME/.notes/**/"$2"*)
    [[ -e ${files[0]} ]] && COMPREPLY=( "${files[@]##~/.notes/}" )
}
complete -o default -F _notes n
{% endcodeblock %}

In addition to working exactly as I hoped, it had the benefit of introducing me
to a couple more bash concepts that I hadn't encountered; 
[adding elements to an array](http://mywiki.wooledge.org/BashGuide/Arrays 'Bash Guide on Greg's Wiki') 
with `+=()` being one. For posterity, the full script is:

{% codeblock lang:sh %}
n() { 
local arg files=(); for arg; do files+=( ~/".notes/$arg" ); done
${EDITOR:-vi} "${files[@]}" 
}

nls() {
tree -CR --noreport $HOME/.notes | awk '{ 
    if (NF==1) print $1; 
    else if (NF==2) print $2; 
    else if (NF==3) printf "  %s\n", $3 
    }'
}

 # TAB completion for notes
_notes() {
local files=($HOME/.notes/**/"$2"*)
    [[ -e ${files[0]} ]] && COMPREPLY=( "${files[@]##~/.notes/}" )
}
complete -o default -F _notes n
{% endcodeblock %}

#### Notes
1. The alternative to remembering the full path name is to list all of the notes before
each operation with the `nls` function; this is not ideal either…
2. Which means that it is either so straightforward that few have bothered to write up their
experiences (most likely), or so arcane that not many have bothered (how it
feels to me). However, there are a couple of pages that I referenced in addition 
to the official documentation:
    * [Adam Backstrong's post](http://www.gnu.org/software/bash/manual/html_node/Programmable-Completion-Builtins.html)
    * [Fahd Shariff's post](http://fahdshariff.blogspot.co.nz/2011/04/writing-your-own-bash-completion.html)
    * [Gentoo Development Guide](http://devmanual.gentoo.org/tasks-reference/completion/index.html)
3. It fails, as geirha pointed out, because it breaks the filenames on whitespace.

Creative Commons image on Flickr by
[nicholasjon](http://www.flickr.com/photos/nicholasjon/4101203095/ 'Field Notes etc on Flickr').
