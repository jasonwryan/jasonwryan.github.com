---
layout: post
title: "Pruning Tarsnap Archives"
date: 2014-10-25 09:38
comments: true
categories: [archlinux, backup, bash, hack] 
---
{% img left /images/post_images/tarsnap-logo.png Tarsnsap Logo %}
I started using [Tarsnap](http://www.tarsnap.com/ 'Tarsnap website') about
[three years ago](http://jasonwryan.com/blog/2011/09/08/tarsnap/ 'Post on Tarsnap')
and I have been nothing but impressed with it since. It is simple to use,
*extremely* cost effective and, more than once, it has saved me from myself;
making it easy to retrieve copies of files that I have inadvertently
overwritten or otherwise done stupid things with<sup>1</sup>.  When I 
[first posted about it](http://jasonwryan.com/blog/2011/09/08/tarsnap/ 'Post on Tarsnap'),
I included a 
[simple wrapper script](https://bitbucket.org/jasonwryan/shiv/src/tip/Scripts/snap 'In Bitbucket repo'), 
which has held up pretty well over that time.

What became apparent over the last couple of months, as I began to consciously
make more regular backups, was that pruning the archives was a relatively
tedious business. Given that Tarsnap 
[de-duplicates data](http://www.tarsnap.com/efficiency.html 'Tarsnap efficiency page'),
there isn't much mileage in keeping around older archives because, if you do
have to retrieve a file, you don't want to have to search through a large
number of archives to find it; so there is a balance between making use of
Tarsnap's efficient functionality, and not creating a rod for your back if your
use case is occasionally retrieving single—or small groups of—files, rather
than large dumps.

I have settled on keeping five to seven archives, depending on the frequency of
my backups, which is somewhere around two to three times a week. Pruning these
archives was becoming tedious, so I wrote a simple script to make it less
onerous. Essentially, it writes a list of all the archives to a 
<span class="file">tmpfile</span>, runs
[sort(1)](http://linux.die.net/man/1/sort 'sort man page')
to order them from oldest to newest, and then deletes the oldest minus whatever
the number to keep is set to.

The bulk of the code is simple enough:

{%codeblock lang:sh snapclean %}
# generate list
tarsnap --list-archives > "$tmpfile"

# sort by descending date, format is: host-ddmmyy_hh:mm
{
  rm "$tmpfile" && sort -k 1.11,1.10 -k 1.8,1.9 -k 1.7,1.6 > "$tmpfile" 
} < "$tmpfile"

# populate the list
mapfile -t archives < "$tmpfile"

# print the full list
printf "%s\n%s\n" "${cyn}Current archives${end}:" "${archives[@]#*-}"

# identify oldest archives
remove=$(( ${#archives[@]} - keep ))
targets=( $(head -n "$remove" "$tmpfile") )

# if there is at least one to remove
if (( ${#targets[@]} >= 1 )); then
  printf "%s\n" "${red}Archives to delete${end}:" 
  printf "%s\n" "${targets[@]#*-}"

  read -p "Proceed with deletion? [${red}Y${end}/N] " YN

  if [[ $YN == Y ]]; then
    for archive in "${targets[@]}"; do
      tarsnap -d --no-print-stats -f "$archive"
    done && printf "%s\n" "${yel}Archives successfully deleted...${end}"

    printf "\n%s\n" "${cyn}Remaining archives:${end}"
    tarsnap --list-archives
  else
    printf "%s\n" "${yel}Operation aborted${end}"
  fi
else
  printf "%s\n" "Nothing to do"
  exit 0
fi
{% endcodeblock %}

You can see the rest of the script in 
[my bitbucket repo](https://bitbucket.org/jasonwryan/shiv/src/tip/Scripts/snapclean 'snapclean in bitbucket').  It even comes [with colour](/images/post_images/snapclean.png 'Screenshot of snapclean').

Once every couple of weeks, I run the script, review the archives marked for
deletion and then blow them away. Easy. If you aren't using Tarsnap, you
really should check it out; it is an excellent service and—for the almost
ridiculously small investment—you get rock solid, encrypted peace of
mind. Why would you not do that?

### Coda
This is the *one hundredth* post on this blog: a milestone that I never
envisaged getting anywhere near. Looking back through the posts, nearly 60,000
words worth, there are a couple there that continue to draw traffic and are
obviously seen at some level as helpful.  There are also quite a few that
qualify as “filler”, but blogging is a discipline like any other and sometimes
you just have to push something up to keep the rhythm going. In any event, this
is a roundabout way of saying that, for a variety of reasons both personal and
professional, I am no longer able to fulfil my own expectations of regularly
pushing these posts out.

I will endeavour to, from time to time when I find something that I genuinely
think is worth sharing, make an effort to write about it, but I can't see that
happening all that often.  I'd like to thank all the people that have read
these posts; especially those of you that have commented. With every post, I
always looked forward to people telling me where I got something wrong or how I
could have approached a problem differently or more effectively<sup>2</sup>; I
learned a lot from these pointers and I am grateful to the people that were
generous enough to share them.

#### Notes
1. The frequency with which this happens is, admittedly, low; but not
low enough to confidently abandon a service like this…
2. Leaving a complimentary note is just as welcome, don't get me wrong…
