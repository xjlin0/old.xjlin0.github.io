---
layout: post
title: "The journey of static page generator"
description: "Writing blog in a different style"
category: tech
tags: [DBC, jekyll, theme]
---
{% include JB/setup %}
### Writing blog in a different style <img src="/assets/imgs/game.png"  alt="TheIncrediblesGame" width="20%"/>

Making blogs with manually starting from scratch is great for learning HTML/CSS, however, doing it for every article would be tedious and distracting from writing content. My site is hosted on Github, so I began to search the theme that can be applied without site migration. Many frameworks are outstanding: Jekyll, Bootstrap, etc, so there's no need to reinvent wheels.

I spent a lot of time with Skinny-bones theme, a pretty Jekyll single pager.  However, even after fixing yaml parsing, due to a minor bug released just before the Christmas, Jekyll got some problems to locally build pages successfully. Learned some Jekyll, I then switch to Jekyll-Bootstrap, an easy starter package. It work at my first try, so simple and I wonder why it stopped development.

After applying the Yuya Saito's the_program theme I moved all my blog over the new platform. Apparently I still need to tweak the jekyll yaml config to set variables.  Jekyll-Bootstrap scaffold conventionally use rake to create posts and pages, so input in Markdown and HTML are both accepted.  But some of my previous layout got whacked, to name a few, image/assets path, sprites, tabs ( my previous blog used ```<dd>``` and ```<blockquote>``` for indentation), search function and Github buttons.  I also got the problems of pushing to Github due to the forking of different repo, and had to remove my original blog repo completely and recreate. But finally the theme/Jekyll/rake all worked locally and remotely, and here is my first new post on the renewed blog.


All of my cohurt mates are great for making Github pages. For example, in the manually crafting stage, Lukas(great CSS design) and Yumiko(external storage by myjason.com) already baked cool layout. Now in the middle of revision, Ty(Whitespace theme for Octopress, which Chase also used), Evan(Landing Page theme for Start Bootstrap) and Sam(Bootstrap+Prism.js) all made impressive works.

```
1. tried several times with skinny bones starter  but failed.
2. tried with A-Sam's solution didnt' work http://blog.ssyog.com/blog/jekyll/install-jekyll-based-on-github-pages.html   Doens't even have jekyll commands after gem install jekyll?
3. tried 3 steps and didn't work http://jekyllrb.com/
4. run brew doctor and found I have not update for several month.
5. tried again with jekyllrb.com and work by "jekyll new blog" and "jekyll serve --baseurl '/blog'"
http://127.0.0.1:4000/blog/jekyll/update/2015/01/28/welcome-to-jekyll.html
6. found I might download a differnt source of zip file of skinny bones in step 1.
7. repeat step 1 and copy welcome markdown file to _posts folder under step 6/blog
8. edit _config.yml and found my mis-swap filename for line 5&7. and I may have misunderstand the url & baseurl part.  Realized I might failed again and need to go back to vanilla installation of jekyll in step 3 to get most basic _config.yml to restart.
9. still getting errors of running skinny bones, google "cannot load such file -- jekyll/version" and got the answer "gem cleanup".  Run bundle install again after clean up
https://github.com/jekyll/jekyll/issues/3084
10. can't work it out.  Give up on skinny bones theme
11. Tried The program theme of Jekyll-Bootstrap got the same problems.
12 found there are many gems are installed by root permission
/Users/j/.rbenv/versions/2.0.0-p481/lib/ruby/gems/2.0.0/gems/
13. remove all gems and reinstall jekyll with "gem install jekyll --user-install" and "bundle install --path /Users/j/.gem/ruby/2.0.0/gems"
14.trying the syntax of yaml.  "" every thing in the _config.yml ==> found the problem was _config.yml
15.copy everything from basic yml to skinny bones yml  ==> it's the tab/space before the key in the siteowner part that yml complains!  (testing by irb -ryaml  and YAML.load_file 'filename.yml' )
16 install octopress for publishing
17 octopress new post "testAgain" --dir test to get _posts/test/2015-01-28-testagain.markdown

bundle exec jekyll build
bundle exec jekyll serve

========================Switch to Jekyll Bootstrap=====
http://jekyllbootstrap.com/usage/jekyll-quick-start.html
=====================================================

under username.github.io
A. git clone https://github.com/plusjade/jekyll-bootstrap.git blog
B. cd blog
C. jekyll serve  #==> it works!!! (even with Jekyll 2.5.3)
D. first attemp to install the program theme failed
rake theme:install git="https://github.com/jekyllbootstrap/theme-the-program.git"
/Users/j/.rbenv/versions/2.0.0-p481/bin/rake:23:in `load': cannot load such file -- /Users/j/.rbenv/versions/2.0.0-p481/lib/ruby/gems/2.0.0/gems/rake-0.9.6/bin/rake (LoadError)
  from /Users/j/.rbenv/versions/2.0.0-p481/bin/rake:23:in `<main>'
E.gem install rake  #==> install rake-10.4.2
F. repeat D and it works locally!!!!!!!!!!!!!!!!!!!!!!!!!
G. found it take <img> tags for proper image positioning.
H. change css under asset/ works locally
http://jekyllbootstrap.com/api/theme-api.html
I. found I can't push to github.
H. remove my repo on Github and redo install following fresh repo
http://jekyllbootstrap.com/usage/jekyll-quick-start.html
error:
xjlin0.github.io [master] :> git remote -v
origin  git@github.com:xjlin0/xjlin0.github.io.git (fetch)
origin  git@github.com:xjlin0/xjlin0.github.io.git (push)
xjlin0.github.io [master] :> git push origin master
Permission denied (publickey).
xjlin0.github.io [master] :> git remote set-url origin https://github.com/xjlin0/xjlin0.github.io.git  #==>works for push
I. yaml working format for liquid syntax  {% raw  %}{{ site.author.name }}{% endraw %}
author :   (left blank)
  name : whatever_name
  email :   whatever_email
_includes/themes/the-program/default.html

J.  rake post title="Hello World" or rake page name="about.md"
K.  jekyll serve #to start local server for browsing
```