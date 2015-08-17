---
layout: post
title: "Install Apache Spark with Python or Ruby-Spark in Yosemite"
description: "Setup the development environment for in-memory MapReduce computing on your Mac"
category: tech
tags: [Ruby, Spark, Python, OSX]
---
{% include JB/setup %}
### Enjoy the power of the spark in local REPL shell, including Ruby. <img src="/assets/imgs/spark.jpg"  alt="Syndrome making spark" width="20%"/>

Recently joined the <a href="https://www.edx.org/course/introduction-big-data-apache-spark-uc-berkeleyx-cs100-1x">"EdX's BerkeleyX  CS 100.1X: Introduction to Big Data with Apache Spark"</a> , I am excited to know there's so much we can do with this cluster computing framework.   Here is how I followed <a href="http://sourabhbajaj.com/mac-setup/Vagrant/README.html">Sourabh Bajaj's tutorial</a> to setup Virtual Box and Vagrant, to extract and run <a href="https://github.com/spark-mooc/mooc-setup/archive/master.zip">the course Vagrantfile</a>. Then a command like <code>vagrant up --provider=virtualbox</code> will get you the complete environment accessible from <a href="http://ipython.org/notebook.html">IPython Notebook</a> within browsers.

{% highlight console %}
brew tap caskroom/cask
brew install brew-cask
brew update && brew upgrade brew-cask && brew cleanup
brew cask install virtualbox
brew cask install vagrant
{% endhighlight %}

It's great to have virtual box running. But I really like to have a separate How local REPL environment to play. Now it's possible to get latest Spark by <code>brew install scala sbt hadoop apache-spark</code>, <code>dev/change-version-to-2.11.sh</code> then <code>build/sbt -Pyarn -Phadoop-2.6 -Dscala-2.11 assembly</code>, with great help such as <a href="http://amodernstory.com/2015/03/05/installing-and-running-spark-with-python-notebook-on-mac/">Marek's tutorial</a> and <a href="http://blog.prabeeshk.com/blog/2015/04/07/self-contained-pyspark-application/">Prabeesh's tutorial</a> (<a href="http://ramhiser.com/2015/02/01/configuring-ipython-notebook-support-for-pyspark/">John's tutorial</a> didn't worked for me). But <string>Spark 1.3.1 binaries work out of box</string>, deliver its environment painlessly within minutes if you just want to write Spark code immediately, without worrying settings for Hadoop, <a href="http://www.jenv.be/">jEnv</a> or <a href="https://github.com/lieldulev/brew-scala-switcher">scala switcher</a>.

{% highlight console %}
brew install wget python
wget -c http://mirrors.ibiblio.org/apache/spark/spark-1.3.1/spark-1.3.1-bin-hadoop2.6.tgz
tar xvzf ./spark-1.3.1-bin-hadoop2.6.tgz
cd spark-1.3.1-bin-hadoop2.6
bin/pyspark
{% endhighlight %}

Now your local Python Spark REPL is ready!  You can test some codes like this (Clark Updike's example):

{% highlight python %}
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /__ / .__/\_,_/_/ /_/\_\   version 1.3.1
      /_/
>>> sc
<pyspark.context.SparkContext object at 0x10ba5fd10>
>>> rdd=sc.parallelize( [('a b',(1,'gold')), ('a b',(2,'gold')), ('a c',(4,'gold'))] )
>>> rdd.cache()
#ParallelCollectionRDD[1] at parallelize at PythonRDD.scala:392
>>> rdd.aggregate(0, lambda acc, (k, (v, label)): acc + v, lambda a, b : a + b )
# => 7  # Spark RDD works!
{% endhighlight %}





Moreover, now it's possible to use Apache Spark in Ruby by <a href="http://ondra-m.github.io/ruby-spark/">RubySpark gem</a>, in addition to Java, Scala, Python or R. This fabulous gem has not supported DataFrames yet, and its shell is based on pry gem. One of my glitches was pry's incompatibility with pry-debugger in Ruby 2.2.2, so I had to <code>gem uninstall pry-debugger</code> and made sure pry works alone first.

{% highlight console %}
brew install java sbt
echo 'export JAVA_HOME=$(/usr/libexec/java_home)' >> ~/.bash_profile
gem install pry ruby-spark
ruby-spark build
ruby-spark shell
{% endhighlight %}

After compiling for some time, your access to Apache Spark™ is now granted!. Let's write some RDD now. According to Ruby-spark gem author's benchmark, starting it by <code>SPARK\_RUBY\_SERIALIZER="oj" ruby-spark shell</code> would use oj as the serializer and will be faster than default Marshal serializer. And the <a href="https://github.com/ondra-m/ruby-spark/wiki/Passing-function">passing of the anonymous function is different from native Ruby</a>. However I don't care, it just works!  have a lot of fun!

{% highlight irb %}
    Welcome to
                  __           ____              __
        ______ __/ /  __ __   / __/__  ___ _____/ /__
       / __/ // / _ \/ // /  _\ \/ _ \/ _ `/ __/  '_/
      /_/  \_,_/_.__/\_, /  /___/ .__/\_,_/_/ /_/\_\   version 1.2.0
                    /___/      /_/

    Spark context is loaded as $sc
[1] pry(main)> rdd = $sc.parallelize(1..10, 3)
=> #<Spark::RDD:0x70362396061240
  Serializer: "Batched(1024) -> Marshal"
Deserializer: "Batched(1024) -> Marshal">
[2] pry(main)> rdd.cache
#=> #<Spark::RDD:0x70362396061240 (cached) ....
[3] pry(main)> rdd.map( lambda{|x| x+5} ).collect()
#=> [6, 7, 8, 9, 10, 11, 12, 13, 14, 15]    # Spark RDD works!
{% endhighlight %}