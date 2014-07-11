---
layout: post
title: "Vim Colours in the Console"
date: 2011-04-06 21:19
comments: true
categories: [vim, linux, tmux]
---
![image](/images/post_images/vim-colours1.png)

I have spent the last couple of days trying to understand why I couldn’t
get [Vim](http://www.vim.org/ "Vim: the _only_ editor") to display the
correct 16 colours in the console. Not in a terminal emulator in X, like
[xterm](http://invisible-island.net/xterm/xterm.html "XTerm homepage")
or
[rxvt-unicode](http://software.schmorp.de/pkg/rxvt-unicode.html "Homepage for urxvt"),
but in the consoles, TTY1-6.

After a lot of searching, more reading into the 
[arcane documentation](http://tldp.org/HOWTO/Text-Terminal-HOWTO-16.html "The Linux terminal...")
of Linux terminals than I was either prepared for or keen on and some
experimentation, I have it working. With one caveat; I wasn’t prepared
to set the `TERM` environment in my <span class="file">.vimrc</span>. Apart from being 
[cautioned against](http://tldp.org/HOWTO/Text-Terminal-HOWTO-10.html#ss10.5 "Console documentation"),
as I *do* set it for tmux, I was anxious to avoid any clashes when
running Vim in [tmux](http://tmux.sourceforge.net/ "tmux homepage")
(which I do most of the time).

From the top, then. Logging into a console and checking the `$TERM` and
colours, gave me the default:

{% codeblock lang:sh %}
$ echo $TERM && tput colors
linux
8
{% endcodeblock %}

Which, under my current configuration of Vim, rendered colours like so:

![image](/images/post_images/vim-colours2.png)

Hardly satisfactory.

The first step was to remove the line in my <span class="file">.vimrc</span> that was forcing 256
colours:

{% codeblock lang:sh %}
set t_Co=256
{% endcodeblock %}

Then I had to rewrite my colourscheme to change all of the references to
the terminal colour numbers to colour *names*, as apparently that is
what 8-colour terms accept. So, my <span class="file">miromiro.vim</span>, went from looking like:

{% codeblock lang:vim %}
hi Normal          ctermfg=15
hi Ignore          ctermfg=8
hi Comment         ctermfg=7 
hi LineNr          ctermfg=8
{% endcodeblock %}

…to the more literal:

{% codeblock lang:vim %}
hi Normal         ctermfg=white    cterm=bold
hi Ignore         ctermfg=black    cterm=bold
hi Comment        ctermfg=grey 
hi LineNr         ctermfg=black    cterm=bold
{% endcodeblock %}

Once I had made the necessary changes, I saved this new colourscheme as
<span class="file">miro8.vim</span><sup>1</sup>. And, yes, I know I could 
have included it in an `if` condition in my original colourscheme<sup>2</sup>, 
but I was intent on keeping things compartmentalized until I had it working…

Running Vim in the console was now starting to look promising:

![image](/images/post_images/vim-colours3.png)

The final piece of the puzzle was to pass my 
[X colours](https://bitbucket.org/jasonwryan/eeepc/src/241da582a0fd/.colours/dark  "jwr dark colours: mercurial repo")
to the console. Fortunately, 
[Aaron Griffin’s 2006 post](http://phraktured.net/linux-console-colors.html "The Overlord himself…")
made this a trivial exercise. With the following lines in my <span class="file">.bashrc</span>:

{% codeblock lang:sh %}
# Linux console colors (jwr dark) 
if [ "$TERM" = "linux" ]; then
   echo -en "\e]P0000000" #black
   echo -en "\e]P83d3d3d" #darkgrey
   echo -en "\e]P18c4665" #darkred
   echo -en "\e]P9bf4d80" #red
   echo -en "\e]P2287373" #darkgreen
   echo -en "\e]PA53a6a6" #green
   echo -en "\e]P37c7c99" #brown
   echo -en "\e]PB9e9ecb" #yellow
   echo -en "\e]P4395573" #darkblue
   echo -en "\e]PC477ab3" #blue
   echo -en "\e]P55e468c" #darkmagenta
   echo -en "\e]PD7e62b3" #magenta
   echo -en "\e]P631658c" #darkcyan
   echo -en "\e]PE6096bf" #cyan
   echo -en "\e]P7899ca1" #lightgrey
   echo -en "\e]PFc0c0c0" #white
   clear # bring us back to default input colours
fi
{% endcodeblock %}

Then it was just a case of setting the relevant colourscheme in my
<span class="file">.vimrc</span>:

{% codeblock lang:vim %}
if &t_Co < 256
    colorscheme miro8   " colourscheme for the 8 colour linux term
else
    colorscheme miromiro 
endif
{% endcodeblock %}

And you can see 
[a screenshot](http://www.flickr.com/photos/jasonwryan/5594191615/#/photos/jasonwryan/5594191615/lightbox/ "Vim colours in console")
of the finished product on Flickr.

##### Notes
1. <a href="https://bitbucket.org/jasonwryan/eeepc/src/241da582a0fd/.vim/colors/miro8.vim" title="Mercurial repo: miro8 colourscheme">miro8.vim</a>
      
2. <a href="https://bitbucket.org/jasonwryan/eeepc/src/241da582a0fd/.vim/colors/miromiro.vim" title="Mercurial repo: miromiro.vim colourscheme">miromiro.vim</a>
