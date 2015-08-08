---
layout: post
title: "Faster Parsing CSV With Parallel Processing"
description: "The best POSIX environment on MS Windows"
category: tech
tags: [Ruby, Rails, csv, Parallel]
---
{% include JB/setup %}
### Speed up parsing csv with smarter_csv and parallel gem<img src="/assets/imgs/dash5.jpg"  alt="five Dashes" width="20%"/>

One hard moment of my last hackathon project was to read <a href="http://ruby-doc.org/stdlib-2.2.2/libdoc/csv/rdoc/CSV.html">CSV</a> files containing millions of records and store in database, and it took very long time. Are there a faster way?

Any version after Ruby 1.9 or Rails 2.2 is ready for <a href="http://ruby-doc.org/core-2.2.2/Thread.html">Thread</a>, which is a light-weight process.  What is Thread for?  If a job needs a person working for 10 hours to finish, ideally this job can be finished by 10 person working for 1 hour.  A thread can be regard as the individual running environment for a worker. We can assign jobs for many workers in different threads (or processes) to do things simultaneously. Therefore, time consuming processing could be speed up by proper coding, because all jobs are executed simultaneously instead of waiting one by one.

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

However, these threads might step in each other's foot, if accessing shared resources.  Such scenario, named <a href="http://en.wikipedia.org/wiki/Race_condition">Race Conditions</a>, need to be considered. For example, if we setup 10 workers to read or write the same file, these workers wouldn't know other workers' progress and might process the same line or write the same files to waste time or corrupt data.  You can use the <a href="http://ruby-doc.org/core-2.2.2/Mutex.html">Mutex</a>, a locking mechanism, to avoid this, or to split jobs to ensure each workers doing different jobs.

How to split huge CSV files for many workers to parse?  There is a amazing gem called <a href="http://github.com/tilo/smarter_csv">smarter_csv</a>, which can parse and split CSV to array of hashes by CSV headers.  Its splitting is ideal for parallel processing, such as <a href="http://github.com/resque/resque">Resque</a> or <a href="http://github.com/mperham/sidekiq">Sidekiq</a>.  Here is how I split CSV and assign for simultaneously parsing huge CSV files.  It shortened days of work to hours by using all 8 Threads on my Mac!

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

In the above example, the records in the huge csv file are parsed as hashes, with the CSV header as keys.  Smarter_csv actually group these hashes into arrays, 1000 hashes/records per array in this example. I then pass the array (chunk) using another fabulous gem <a href="http://github.com/grosser/parallel">parallel</a>. In such way a huge CSV is sliced into chunks of 1000 records and assign to workers for parallel processing, without queuing by <a href="http://redis.io">Redis</a>. One trick mentioned by <a href="http://ruby.zigzo.com/2012/01/29/the-parallel-gem-and-postgresql-oh-and-rails/">Mario</a>, is to reconnect the database since PostgreSQL does not allow using the same connection for more than one thread.

Also, if you just need to update some attributes for the model, you can also utilize all processors cores by doing the following steps.  In ActiveRecord, I tend to use <code>find\_each</code> or <code>find\_in\_batches</code> to process many records to avoid choking my server.

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
<hr>

By the way, wanna go multi-threading in the browser side too?  If you got a CPU intensive JavaScript to run, please utilize <a href="http://www.w3schools.com/html/html5_webworkers.asp">HTML5 Web Workers</a> to free up browsers!