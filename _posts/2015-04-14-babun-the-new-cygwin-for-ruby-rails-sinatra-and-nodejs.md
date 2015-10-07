---
layout: post
title: "Babun: the new Cygwin for Ruby, Rails, Sinatra and Node.js"
description: "The best POSIX environment on MS Windows"
category: tech
tags: [Ruby, Rails, Node, Cygwin]
---
{% include JB/setup %}
### Now you can easily develop Rails apps on MS Windows like unix<img src="/assets/imgs/baboon.jpeg"  alt="baboon" width="20%"/>

It's hard to setup POSIX compatible environment on MS windows PC for your coding setup, here is the way I found feasible, on Windows 7 64 bit with Babun 1.2.0-dist, updated on Sep 27 2015.

##Installation steps:
- Download Babun distribution from <a href="http://babun.github.io">Babun official site</a> by hitting the big <code>[Download Now](http://projects.reficio.org/babun/download)</code> button on the top of the landing page and extract it.

- Enter MS Windows command line mode ([a.k.a cmd](https://www.youtube.com/watch?v=JOrY5PEo-iE)) and go to the extracted folder to start. I recommend to install it under the top folder of a drive to avoid hitting the max path length of the MS Windows file system. Remember, the slash of MS Windows and Unix are different.
{% highlight console %}
install.bat /t "c:/"
{% endhighlight %}
Babun system will finish the installation within a few minutes, and bring you its default terminal under zsh.  One of my favorite things is X-window style mouse copying/pasting, <em><strong>in Babun the mouse selected text in the terminal is immediately copied, you can then paste by mouse right-clicking</strong></em>.
<hr>
### Quick and short steps for starters
If you don't have time and need Ruby and Node.js now (especially for phase 0 DBC students), just execute the following commands to have Ruby 2.0, Rails 4.0, the latest Sinatra and Node 0.4.12. It's enough to cover the entire DBC phase 0~2.  Under Babun terminal, please enter the following three lines of commands:
{% highlight sh %}
babun update
pact install ruby ruby-nokogiri ruby-rails ruby-pg libpq-devel libxml2-devel libxslt-devel gcc-g++ patch sqlite3 postgresql curl wget libgmp-devel
gem install pg sinatra shotgun rspec rails
{% endhighlight %}
(Note: Some gems, such as Puma or Turbolinks are not compatible with these settings as of now. Solution will be provided if there's a working one.)

As of Sep 2015, Rails 4.2.4 and Ruby 2.2.2 should be installed flawlessly by the commands described above. If you unfortunately got some errors about nokogiri error, or you want more updated version of Rails (>4.2.4), or need Heroku tool belt later (say, phase 3), here is what to do:

{% highlight sh %}
gem install nokogiri -- --use-system-libraries
gem install rails
wget -qO- https://toolbelt.heroku.com/install.sh | sh
echo 'PATH="/usr/local/heroku/bin:$PATH"' >> ~/.zshrc
{% endhighlight %}

Next let's setup node.js. Please download the latest installer for windows at [node official site](https://nodejs.org) (v4.1.1 for win64 as the example here), install it. Close and relaunch Babun shell, node REPL console should be available by the following commend.

{% highlight sh %}
node -i
{% endhighlight %}

That's it! enjoy your free, full-functional POSIX environment under MS windows without deleting any of your files or repartitioning your drive, in minutes.
___
## Installation of rbenv and Ruby 2.2

[rbenv](https://github.com/sstephenson/rbenv) is a fantastic Ruby version manager, but its installation, with building plugins, requires several steps. To make it simpler, a super great tool, [getrbenv installer](http://getrbenv.com), was developed. However its current master might need a subtle update to fit zsh used in Babun.  Here is my workaround to install rbenv with ruby-build and rbenv-update plugin painlessly. Let's first start without Ruby version assigned. (The following command is ONE line)
{% highlight sh %}
curl -sSL https://raw.githubusercontent.com/xjlin0/getrbenv-installer/feature/fix_zshrc_install_dir_pathfix/bin/install.sh | bash -s -- --plugins sstephenson/ruby-build,rkh/rbenv-update
{% endhighlight %}
Success? Please close Babun shell and restart Babun to get the path works. Now how do you install newer version of Ruby? For Ruby 2.2.3, just need to type:

{% highlight sh %}
rbenv install 2.2.3
rbenv rehash
rbenv global 2.2.3
{% endhighlight %}

You can also install older Ruby 2.1.7 or 1.9.3 by `rbenv install 2.1.7` or `rbenv install 1.9.3-p551`. How about 2.0.0-p647? Currently there is [a naming bug](https://bugs.ruby-lang.org/issues/11065) in some versions of Ruby under ext/-test- folder and may stop the compilation in installation. Thus [I made a patch](/assets/imgs/uutoa_printf.patch) based on [dmattes's suggestion](https://github.com/babun/babun/issues/93) to prevent the compilation stopping you at printf.c. The patch won't hurt ruby's function since it's in test folder.

{% highlight csh %}
 curl http://xjlin0.github.io/assets/imgs/uutoa_printf.patch | rbenv install --patch 2.0.0-p647
{% endhighlight %}

___
## Compile your own Node.js (NOT recommended)
The windows native binaries by node.js installer should be good for use. However here is some other non-required adventure for those likes experimenting. The last version of Node.js with official support for Cygwin was 0.4.12, according to [Lukasz P](https://github.com/babun/babun/issues/216) and [Joyent's suggestion](https://github.com/joyent/node/wiki/Installation#building-on-cygwin). If you enjoy waiting for compile, here is how to compile and install the old faithful Node.js 0.4.12.

{% highlight sh %}
wget http://nodejs.org/dist/node-v0.4.12.tar.gz
tar xvfz node-v0.4.12.tar.gz
cd node-v0.4.12/
./configure
make
make install
{% endhighlight %}

How about newer version? Well, [bnoordhuis's patch](https://github.com/joyent/node/issues/1734) works for v0.5.8.  I [applied the patch and attach here for you to download/install](/assets/imgs/node-v0.5.8-patched4cygwin.tar.gz). I have not found ways to compile newer versions.

{% highlight sh %}
wget http://xjlin0.github.io/assets/imgs/node-v0.5.8-patched4cygwin.tar.gz
tar xvfz node-v0.5.8-patched4cygwin.tar.gz
cd node-v0.5.8/
./configure
make
make install
{% endhighlight %}
___

### Some tips for PostgreSQL service under Babun / Cygwin
PostgreSQL service is slow but still works under Babun if you follow the following steps to start the service. Basically these are just the steps described in /usr/share/doc/Cygwin/postgresql.README, and you may want to include /usr/sbin in the path.
{% highlight sh %}
cygrunsrv -S cygserver
/usr/sbin/initdb.exe -D /usr/share/postgresql/data
/usr/sbin/pg_ctl start -D /usr/share/postgresql/data -l /var/log/postgresql.log
createdb
{% endhighlight %}
You may now use psql to check your PostgreSQL service.  In case you want to stop the PostgreSQL service someday:
{% highlight sh %}
/usr/sbin/pg_ctl -D /usr/share/postgresql/data -l logfile stop
cygrunsrv -E cygserver
{% endhighlight %}
And you may also want to remove the data (/usr/share/postgresql/data) when the PostgreSQL service no longer needed.  Please be noted that by such method, even cygserver running is persistent in windows, the closing and restarting of Babun shell will terminate postgresql service so you may need to manually restart it every time.
