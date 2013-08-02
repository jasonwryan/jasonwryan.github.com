---
layout: post
title: "Command Line Notes"
date: 2010-09-28 17:56
comments: true
categories: [linux, hack, bash]
---
![image](http://miromiro.com/Blog-images/cli-notes-1.png)

A couple of weeks ago, I came across these 
[simple bash functions](http://lifehacker.com/5592047/turn-your-command-line-into-a-fast-and-simple-note+taking-tool)
written by Jack Mottram of [One Thing Well](http://onethingwell.org/ "One Thing Well") for managing text
notes.

The functions as described in the article are so:

{% codeblock lang:sh %}
n() { $EDITOR ~/notes/"$*".txt } 

nls() { ls -c ~/notes/ | grep "$*" }
{% endcodeblock %}

Unfortunately, the second function – for retrieving the list of files in
the notes directory has two significant shortcomings — it uses `ls`<sup>1</sup>
and it will only list files in the top level directory. If you are an
inverterate note taker, this plainly won’t scale.

My first attempt at hacking a solution yielded this little beauty:

{% codeblock lang:sh %}
nls() { tree -Cu --noreport ~/.notes | awk '{print $2,$3}' | tr -d [:digit:] | sed 's/]//'\ 
| cut -d"." -f1 ; }
{% endcodeblock %}

Which removes `ls` from the function and allows nested directories, but
is needlessly complex and, with four pipes, hideously ~~inefficient~~
inelegant.

After reading up on Awk, particularly 
[Bruce Barnett’s primer](http://www.grymoire.com/Unix/Awk.html), 
I arrived at something marginally longer but relying solely on the 
awesome power of awk to make the various transformations I was seeking…

{% codeblock lang:sh %}
nls() { tree -CR --noreport ~/.notes | awk '{ if (NF==1) print $1; \
else if (NF==2) print $2; else if (NF==3) print "  "$3 }' ; }
{% endcodeblock %}

An added advantage is that it is scalable irrespective of how deep the
directories go. I’m sure that with some more awk-foo I could write a
more effective set of conditionals, but this seemed a pretty good start
for a simple note taking utility.

### Updated 3/11/10
To remove a little of the visual clutter, I amended the awk script to
strip the `.txt` extensions from the output.<sup>2</sup>

{% codeblock lang:sh %}
nls () { tree -CR --noreport ~/.notes | awk '{ if ((NR > 1) gsub(/.txt/,"")); \
if (NF==1) print $1; else if (NF==2) print $2; else if (NF==3) printf "  %s\n", $3 }' ;}
{% endcodeblock %}

And if you really want this to work well, create your <span class="file">.notes</span> directory
in a [dropbox folder](http://dropbox.com/ "File synching utility") and
symlink to it from <span class="file">~/.notes</span>.

##### Notes
1. See <a href="http://mywiki.wooledge.org/ParsingLs" title="THE Bash Wiki">Parsing ls on Wooledge Wiki</a>.
      
2. Obligatory “after” screenshot:
      
{% img http://miromiro.com/Blog-images/cli-notes-2.png 'Screenshot of editing text' %}
