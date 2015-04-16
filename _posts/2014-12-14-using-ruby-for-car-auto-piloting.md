---
layout: post
title: "Using Ruby for car auto piloting"
description: "find passing routes by Enumerable methods"
category: tech
tags: [Ruby, enumerable]
---
{% include JB/setup %}
### find passing routes by Enumerable methods<img src="/assets/imgs/bobcar.jpg"  alt="BabyFacingKariFocused" width="20%"/>

<div id="article">
<p>Nobody is happy in the traffic. It'd be great if cars can drive by themselves. Nowadays some new cars come with "traffic jam assistance" for <a href="https://www.youtube.com/watch?v=GBl_V-J56zI">auto-turning</a> or <a href="https://www.youtube.com/watch?v=lf89nOE8W0U">stop&amp;accelerate</a>. But it's not enough for auto-driving. <a href="https://www.youtube.com/watch?v=4NkqY4AucYQ">Finding path with obstacle avoidance</a> is critical in addition to GPS routing. Let's take a look at the road and find the route for steering behaviour of an autonomous car.</p>
<img src="/assets/imgs/c8.jpg" alt="road radar map and matrix" width="97%" />
<p>Let's say the magic radar detect all obstacles on the road, how can we figure out the driving path from the left to the right? As an Array lover I'd put the radar readings into 3 arrays corresponding to 3 lanes:</p>

{% highlight ruby %}
Welcome to IRB. You are using ruby 2.0.0p576 (2014-09-19) [i386-cygwin]. Have fun ;)
> a = [0,1,2,"c","c",5,6,7,"c","?","10"]
> b = [0,"c","c","c",4,5,6,7,8,9,"c"]
> c = [0,1,2,3,4,5,"c","c",8,9,10]
> a.chunk{|x|x.is_a?(Fixnum)}.map{|y|p y}
[true, [0, 1, 2]]
[false, ["c", "c"]]
[true, [5, 6, 7]]
[false, ["c", "?", "10"]]
{% endhighlight %}

<p>"Wait a second!" you might think "What's that <strong><em>chunk</em></strong>?". It's a wonderful Enumerable Method told by one of our great cohort mate, Yumiko. <code>chunk</code> finds consecutive elements by the assigned definition. In this case it finds consecutive integers and we can list all "true" elements it found as the identified cleared path for autonomous driving:</p>

{% highlight ruby %}
> apath=a.chunk{|x|x.is_a?(Fixnum)}.map{|y|y.last if y.first}.compact
=> [[0, 1, 2], [5, 6, 7]]
> bpath=b.chunk{|x|x.is_a?(Fixnum)}.map{|y|y.last if y.first}.compact
=> [[0], [4, 5, 6, 7, 8, 9]]
> cpath=c.chunk{|x|x.is_a?(Fixnum)}.map{|y|y.last if y.first}.compact
=> [[0, 1, 2, 3, 4, 5], [8, 9, 10]]
{% endhighlight %}

<p>Now with the power of the <code>chunk</code> method we store all drivable sections into 2D arrays. However, the car cannot proceed without changing lanes, how can we find the path for passing? For clarity here I show only from land C but the same procedure can be applied into any lanes. So we start to drive on cpath[0], let's check where could we switch to lane B by <code>.include?</code> method:

{% highlight ruby %}
> cpath[0] #=> [0, 1, 2, 3, 4, 5]    #drive on cpath[0] to the right (max)!
> bpath.each{ |y| p y if y.include?cpath[0].max }  #check lane B
[4, 5, 6, 7, 8, 9]    #Yes, let's change to lane B!
> bpath.index([4, 5, 6, 7, 8, 9]) #=> 1  #We are now driving on bpath[1]
{% endhighlight %}

<p>Cool, we just drive through land C cpath[0] to land B bpath[1]. When drive to the end of bpath[1], should we change to lane A or lane C? Let's make a decision now:</p>

{% highlight irb %}
> apath.each{ |x| p x if x.include?bpath[1].max }   #nothing! don't go lane A
> cpath.each{ |z| p z if z.include?bpath[1].max }
[8, 9, 10]    #Great! there's a way on lane C
> cpath.index([8,9,10]) #=> 1 #now let's switch to lane C to the section cpath[1]
{% endhighlight %}

<p>See now we can drive on cpath[1] to the right. Thus our autopilot route is cpath[0]→bpath[1]→cpath[1]. Of course this is very simple case, but we can use this procedure to calculate the weighing of paths and apply to a much complicate situation.  What a great method!</P>
</div>