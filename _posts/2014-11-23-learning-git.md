---
layout: post
title: "Surviving the first week at DBC: learning git"
tagline: "a whole new world to me"
description: "The essence of the software version control"
category: tech
tags: [DBC, git]
---
{% include JB/setup %}

### a whole new world to me<img src="/assets/imgs/t1.jpg"  alt="BabyFacingToysConfused" width="20%"/>
We all need version control in our life. To accomplish something one need time to make efforts or try and error in a period of time. During this process we need to track changes so remind ourself what's good. It's so great that in a programmers life the time travel is possible via git commands especially in teamwork environment. Everybody needs to track changes, such as time line on Facebook. Too bad we can't do that in the reality.
`git` is a sophisticated software for version control. It can automatically monitor and generate hashes, "fingerprints" of files all the times. Every bit changes on the files will essentially result in different fingerprints; for instance, the hash of files containing `Apple` and `Apples` are different. In this way `git` can track a files along all the changes, and it can help us to unlimitedly go to any history in file changes.
Such wonderful tool not just work locally in our computers. In addition, everybody can share and track changes on GitHub, a Facebook-like website for program(mer)s. Since code is just a set of exact orders to make computers processing our requests, we can always discuss to each other, work on it together and find a better way to improve it. GitHub is the place to polish our codes since it takes the uploads from git and let developers around the world copy/work on your code(fork) under well regulated rules. That's why GitHub is the best place for us to combine(merge) each other's work or create projects/branches, without the worry of losing codes by the power of `git`.
It's my privilege to learn all of these with my cohort, your help really makes me grow and I cannot finish these challenges without you. Wish all of us become the Neo and live peacefully in the Matrix someday.


PS. the type of the "Git Workflow in a Group Project" submitted 11 months ago by CoffeeSwirl http://i.imgur.com/zLxTjn9.jpg
<img src="http://i.imgur.com/zLxTjn9.jpg"  alt="git flow"/>

```
[Git Workflow in a Group Project]

On Local machine
-------------
git pull master from Github
create your feature branch
work on your branch, commit.
Ready to push? STOP!

Switch to master branch
git pull origin master
switch to feature branch
git merge master
resolve merge conflict
git add + commit
git push origin feature branch

On Github
-----------
feature_branch   pull request
Ask someone to merge
Merge featyre_branch into Master


*Master is always changing!
Always pull before your push!
```

___
Don't forget to check out <a href="https://www.linkedin.com/pulse/rebasing-git-jeremy-gagon">Jeremy's great post about Git rebase.</a>