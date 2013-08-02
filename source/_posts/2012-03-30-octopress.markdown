---
layout: post
title: "Moving to Octopress"
date: 2012-03-30 12:30
comments: true
categories: [archlinux, bash, hack]
---
{% img left http://miromiro.com/Blog-images/octopress_logo.png 240 240 Octopress Logo %}

Over the last week I have been moving my blog over to [Octopress](http://octopress.org/),
a lightweight blogging framework for [Jekyll](https://github.com/mojombo/jekyll),
the static site generator powering [Github Pages](http://pages.github.com/). I had previously
been posting to a [tumblr](https://www.tumblr.com/) page and, over the nearly four years that
I had been doing that I had somehow racked up just over 4000 posts. I was *not*
looking forward to migrating across.

However, the fact that the Jekyll project has a number of scripts for 
[migrating from other platforms](https://github.com/mojombo/jekyll/wiki/blog-migrations)
assuaged my concerns about the difficulty of this task. That sense of relief was shortlived.
Neither of the two tumblr migration scripts were of any assistance: both would die during their
initial runs, probably due to some funky characters in the post titles, or perhaps the posts themselves.

I certainly had no intention of trying to wade through the entire back catalogue identifying the
rogue posts. Rather that admit defeat, and probably more due to a sense of misguided optimism about the 
“straightforward” nature of the task, I saw this setback as an opportunity to cull all of the
cruft<sup>1</sup> from the blog and decided to manually import the fifty posts that I thought were of
some interest.

Being an assiduous record keeper, all of the posts were helpfully bookmarked on 
[Pinboard under one tag](https://pinboard.in/u:jasonwryan/t:jwr/), and therefore it was
simple enough to create a list of the required <acronym title="Unique Resource Locator">URLs</acronym>.
Armed with this list, it was just a matter of cobbling together a script to do the bulk of
the work for me.

The first task was to retrieve the posts from the list:

{% codeblock lang:sh %}
# grab files
while read url; do 
    wget --adjust-extension "${url}"
done < /home/jason/Scripts/list 
{% endcodeblock %}

Then I needed to remove all of the <acronym title="HyperText Markup Language">HTML</acronym>
surrounding the actual posts: an `awk` one-liner took care of that.

{% codeblock lang:sh %}
# strip HTML cruft
for file in *.html; do
  awk '/<h3>/ {flag=1;next} /<\/div>/{flag=0} flag {print}' "$file" > "${file%%.*}"
done
mkdir html && mv *.html html/
{% endcodeblock %}

The final task of this part of the migration was to convert the HTML into
[markdown](http://daringfireball.net/projects/markdown/), 
the format that Octopress uses. [Pandoc](http://johnmacfarlane.net/pandoc/)
the "universal document converter" handled that job:

{% codeblock lang:sh %}
# convert to markdown format
for file in *; do 
    pandoc -f html -t markdown "$file" > "$file".md
done
{% endcodeblock %}

The final result was fifty markdown files holding all of my posts, almost ready to
be committed to github. I say “almost” because the files still required what turned out to 
be a reasonable amount of cleaning up. Pandoc did a great job, for example, but would 
inexpicably break [multi word
hyperlinks](http://www.notareallink.com) over two lines. Similarly all of the internal
links to my other posts pointed to the (meaningless) tumblr URLs<sup>2</sup>.

Setting up Octopress was extremely simple and quick by comparison: the
[documentation is very helpful](http://octopress.org/docs/). There was one slight 
hitch, a [known issue on Arch x86\_64](https://github.com/tmm1/pygments.rb/issues/10),
which was simple enough to deal with.

While the migration was not entirely pain-free, I am pleased that I have done it. Tumblr's service
increasingly [left a lot to be desired](https://twitter.com/#!/jasonwryan/statuses/176543962276954112)
but as it was a free service, I couldn't complain too much. Or, more accurately, when I did complain, 
no-one actually listened…

Indeed, moving to a paid service like Github
(yes, it's free at first, but once you have enough data there you need to pay a small amount
every month) [makes a lot of sense](http://blog.pinboard.in/2011/12/don_t_be_a_free_user/).
The paid services I do use, like [Pinboard](http://pinboard.in/) and 
[Tarsnap](http://www.tarsnap.com/) are both inexpensive and much more 
reliable than their free counterparts<sup>3</sup>; and you get to invest in great
software that is a pleasure to use.

##### Notes
1. Initially, I had set up the site as a simple holding page and dumped a whole lot
of feeds into it: twitter, bookmarks, scrobbled music, etc. Those 4000 posts were
mostly just that sort of internet detritus…
2. For creating redirections (Github pages do *not* support <span class="file">.htaccess</span>)
I can't recommend enough the 
[Jekyll Alias Generator](https://github.com/rawsyntax/jekyll_alias_generator/blob/master/_plugins/alias_generator.rb).
Just. Brilliant.
3. And **much** more scrupulous about how they use your personal data.

