---
layout: post
title: "The journey of static page generator"
description: "Writing blog in a different style"
category: tech
tags: [DBC, jekyll, theme]
---
{% include JB/setup %}
### Writing blog in a different style

Making blogs with manually starting from scratch is great for learning HTML/CSS, however, doing it for every article would be tedious and distracting from writing content. My site is hosted on Github, so I began to search the theme that can be applied without site migration. Many framworks are outstanding: Jekyll, Bootstrap, etc, so there's no need to reinvent wheels.

I spent a lot of time with Skinny-bones theme, a pretty Jekyll single pager.  However, even after fixing yaml parsing, due to a minor bug released just before the Christmas, Jekyll got some problems to locally build pages successfully. Learned some Jekyll, I then switch to Jekyll-Bootstrap, an easy starter package. It work at my first try, so simple and I wonder why it stopped development.

After applying the Yuya Saito's the_program theme I moved all my blog over the new platform. Apperently I still need to tweak the jekyll yaml config to set variables.  Jekyll-Bootstrap scaffold conventionally use rake to create posts and pages, so input in Markdown and HTML are both accepted.  But some of my previous layout got whacked, to name a few, image/assets path, sprites, tabs ( used <dd> and <blockquote> ).  I also got the probles of pushing to Github due to the forking of different repo, and had to remove my original blog repo completely and recreate. But finally the theme/Jekyll/rake all worked locally and remotely, and here is my first new post on the renewed blog.


All of my cohurt mates are great for making Github pages. For example, in the manually crafting stage, Lukas(great CSS design) and Yumiko(external storage by myjason.com) already baked cool layout. Now in the middle of revision, Ty(Whitespace theme for Octopress, which Chase also used), Evan(Landing Page theme for Start Bootstrap) and Sam(Bootstrap+Prism.js) all made impressive works.