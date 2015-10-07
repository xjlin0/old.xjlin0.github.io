---
layout: post
title: "Faster Parsing CSV With Parallel Processing"
description: "The best POSIX environment on MS Windows"
category: tech
tags: [Ruby, Rails, csv, Parallel]
---
{% include JB/setup %}
### Speed up parsing csv with smarter_csv and parallel gem<img src="/assets/imgs/dash5.jpg"  alt="five Dashes" width="20%"/>

One hard moment of my last hackathon project was to read [CSV](http://ruby-doc.org/stdlib-2.2.3/libdoc/csv/rdoc/CSV.html) files containing millions of records and store in database, and it took very long time. Are there a faster way?

Any version after Ruby 1.9 or Rails 2.2 is ready for [Thread](http://ruby-doc.org/core-2.2.3/Thread.html), which is a light-weight process.  What is Thread for?  If a job needs a person working for 10 hours to finish, ideally this job can be finished by 10 person working for 1 hour.  A thread can be regard as the individual running environment for a worker. We can assign jobs for many workers in different threads (or processes) to do things simultaneously. Therefore, time consuming processing could be speed up by proper coding, because all jobs are executed simultaneously instead of waiting one by one.

{% highlight ruby %}
def talk(person)
  puts "Start to talk with #{person}"
  sleep rand (1..10)
  puts "done talking with #{person}"
end

persons = ["Bob", "Helen", "Violet", "Dash", "Jack"]

threads = persons.map do |person|
  Thread.new { talk(person) }
end

threads.each{|t|t.join}
puts "Finished talking with Incredibles!"
{% endhighlight %}

However, these threads might step in each other's foot, if accessing shared resources.  Such scenario, named [Race Conditions](http://en.wikipedia.org/wiki/Race_condition), need to be considered. For example, if we setup 10 workers to read or write the same file, these workers wouldn't know other workers' progress and might process the same line or write the same files to waste time or corrupt data.  You can use the [Mutex](http://ruby-doc.org/core-2.2.3/Mutex.html), a locking mechanism, to avoid this, or to split jobs to ensure each workers doing different jobs.

How to split huge CSV files for many workers to parse?  There is a amazing gem called [smarter_csv](http://github.com/tilo/smarter_csv), which can parse and split CSV to array of hashes by CSV headers.  Its splitting is ideal for parallel processing, such as [Resque](http://github.com/resque/resque) or [Sidekiq](http://github.com/mperham/sidekiq).  Here is how I split CSV and assign for simultaneously parsing huge CSV files.  It shortened days of work to hours by using all 8 Threads on my Mac!

{% highlight ruby %}
require 'parallel'
require 'smarter_csv'

def worker(array_of_hashes)
  ActiveRecord::Base.connection.reconnect!
  #data seeding
end

chunks = SmarterCSV.process('filename.CSV', chunk_size: 1000)

Parallel.map(chunks) do |chunk|
  worker(chunk)
end
{% endhighlight %}

In the above example, the records in the huge csv file are parsed as hashes, with the CSV header as keys.  Smarter_csv actually group these hashes into arrays, 1000 hashes/records per array in this example. I then pass the array (chunk) using another fabulous gem [parallel](http://github.com/grosser/parallel). In such way a huge CSV is sliced into chunks of 1000 records and assign to workers for parallel processing, without queuing by [Redis](http://redis.io). One trick mentioned by [Mario](http://ruby.zigzo.com/2012/01/29/the-parallel-gem-and-postgresql-oh-and-rails/), is to reconnect the database since PostgreSQL does not allow using the same connection for more than one thread.

Also, if you just need to update some attributes for the model, you can also utilize all processors cores by doing the following steps.  In ActiveRecord, I tend to use `find_each` or `find_in_batches` to process many records to avoid choking my server.

{% highlight ruby %}
require 'parallel'

def worker(array_of_instances)
  ActiveRecord::Base.connection.reconnect!
  array_of_instances.each do |instance|
    instance.update(attribute: value) #or any other thread-safe codes
  end
end

Parallel.each(Model.find_in_batches) do |array_of_instances|
  worker(array_of_instances)
end
ActiveRecord::Base.connection.reconnect!

{% endhighlight %}
___

By the way, wanna go multi-threading in the browser side too?  If you got a CPU intensive JavaScript to run, please utilize [HTML5 Web Workers](http://www.w3schools.com/html/html5_webworkers.asp) to free up browsers!

___
Wanna more parallel jobs?  How about speed up your RSpect by running parallel on multiple CPU cores? Take a look of [parallel_tests gem](https://github.com/grosser/parallel_tests).