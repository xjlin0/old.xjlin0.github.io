---
layout: post
title: "Babun: the new Cygwin for Ruby, Rails, Sinatra and Node.js"
description: "The best POSIX environment on MS Windows"
category: tech
tags: [Ruby, Rails, Node]
---
{% include JB/setup %}
### Now you can easily develop Rails apps on MS Windows like unix<img src="/assets/imgs/users.jpg"  alt="major incredible roles" width="20%"/>

##Installation steps:
- Download Babun distribution from <a href="http://babun.github.io">Babun official site</a> by hitting the big <code>Download Now</code> button on the top of the landing page and extract it.

- Enter MS Windows command line mode (<a href="https://www.youtube.com/watch?v=JOrY5PEo-iE">a.k.a cmd</a>) and go to the extracted folder to start. I recommend to install it under the top folder of a drive to avoid hitting the max path length of the MS Windows file system. Remember, the slash of MS Windows and Unix are different.
{% highlight console %}
install.bat /t "c:/"
{% endhighlight %}
Babun system will finish the installation within a few minutes, and bring you its default terminal under zsh.  One of my favorite things is X-window style mouse copying/pasting, <em><strong>in Babun the mouse selected text in shell is immediately copied, you can then paste by mouse right-clicking</strong></em>.
<hr>
### Quick and short steps for beginners
If you don't have time and need Ruby and Node.js now (especially for phase 0 DBC students), just execute the following commands to have Ruby 2.0, Rails 4.0, the latest Sinatra and Node 0.4.12. It's enough to cover the entire DBC phase 0~2.  Under Babun shell, please enter:
{% highlight sh %}
babun update
pact install ruby ruby-nokogiri ruby-rails ruby-pg libxml2-devel libxslt-devel gcc-g++ patch sqlite3 postgresql
gem install sinatra
gem install shotgun
{% endhighlight %}

Finally, install the old faithful Node.js 0.4.12, according to <a href="https://github.com/babun/babun/issues/216">Lukasz P</a> and <a href="https://github.com/joyent/node/wiki/Installation#building-on-cygwin">Joyent's suggestion</a>.
{% highlight sh %}
wget http://nodejs.org/dist/node-v0.4.12.tar.gz
tar xvfz node-v0.4.12.tar.gz
cd node-v0.4.12/
./configure
make
make install
{% endhighlight %}

Later (say, phase 3) you may need more updated version of Rails (>=4.2.1) and Heroku toolbelt:
{% highlight sh %}
gem install nokogiri -- --use-system-libraries
gem install rails
wget -qO- https://toolbelt.heroku.com/install.sh | sh
echo 'PATH="/usr/local/heroku/bin:$PATH"' >> ~/.zshrc
{% endhighlight %}
That's it! enjoy your free, full-functional POSIX environment under MS windows without deleting any of your files, in minutes.
<hr>
## Installation of rbenv and Ruby 2.2
Please follow the official guide of rbenv to install <a href="https://github.com/sstephenson/rbenv">rbenv</a> and <a href="https://github.com/sstephenson/ruby-build">ruby-build plugin</a>.  It's a pity that I haven't figure out how to use <a href="http://getrbenv.com/">getrbenv.com</a> on Babun/zsh to replace the following steps.
{% highlight sh %}
git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(rbenv init -)"' >> ~/.zshrc
git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
{% endhighlight %}
Please finish the installation of rbenv and ruby-build by closing/exiting and restarting Babun shell.  There is a bug in the bleeding edge of ruby 2.2 under test folder and may stop the installation. Thus <a href="/assets/imgs/uutoa_printf.patch">I made a patch for ruby 2.2</a> based on <a href="https://github.com/babun/babun/issues/93">dmattes's suggestion</a>. If the compilation stop you at printf.c, the patch won't hurt ruby's function since it's in test folder. When ruby core team fix it in the future, we will be able to enter <code>rbenv install 2.2</code> without patching.
{% highlight csh %}
rbenv install --patch 2.2.0 < patch_path/patch_file
rbenv rehash
{% endhighlight %}

<hr>
## Installation of newer Node.js
The last version of Node.js with official support for Cygwin was 0.4.12, but <a href="https://github.com/joyent/node/issues/1734">bnoordhuis's patch works for v0.5.8</a>.  <a href="/assets/imgs/node-v0.5.8-patched4cygwin.tar.gz">I applied the patch and attach here for you to download</a>. You can use the same steps described above.
{% highlight sh %}
tar xvfz node-v0.5.8-patched4cygwin.tar.gz
cd node-v0.5.8/
./configure
make
make install
{% endhighlight %}

If you need more modern versions, <a href="http://soyuka.me/using-nodejs-with-cygwin-v0-10-25/">soyuka's methods works by connecting windows native binaries to Cygwin</a>, and <a href="https://github.com/babun/babun/issues/216">it works for Babun</a>, not sure if Console 2 is still required in the fabulous Babun 1.1.