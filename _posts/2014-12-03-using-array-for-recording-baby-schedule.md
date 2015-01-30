---
layout: post
title: "Using array for recording baby schedule"
description: "find baby sitting gaps by Ruby"
category: tech
tags: [Ruby, array, hash]
---
{% include JB/setup %}
### find baby sitting gaps by Ruby<img src="/assets/imgs/exhausted.jpg"  alt="BabyFacingKariFocused" width="20%"/>

<div id="article">
<p>How does the Incredibles arrange baby sitting? This demanding baby needs <em>non-stop</em> care 24 hours a day, and their life would <a href="https://www.youtube.com/watch?v=0dQpFu8uRP0">go crazy if there's a gap</a>. Let's take a look at the baby's schedule and find the gap:</p>
<img src="/assets/imgs/t3.jpg" alt="baby sitting schedule" width="97%" />
  <p>"That's ridiculous", you may think, "why would we need a program to find this?".  Well it's very easy to spot the gap if there's only one baby with several babysitters, but when there're hundreds of dying patients and busy nurses it's definitely a nightmare to find gaps in a highly dynamic schedule. It's not a joke, it's a real situation my sister's facing and as a developer <a href="https://github.com/xjlin0/ReFormatter">I got to write a program to help this</a>.  But how?</p>
  <p>My first thought was to write a method, to loop every hour of the day and check if it's within the shift of the baby sitter on the schedule. However, loops and flow controls are slow.  Is there a better way? Fortunately in Ruby we have two flexible data structures: Array and Hash.  They can be sets of any available objects, and marvellously there's a wonderful property of Array when it comes to the subtraction:</p>

{% highlight ruby %}
Welcome to IRB. You are using ruby 2.0.0p576 (2014-09-19) [i386-cygwin]. Have fun ;)
> [1,2,3,4,5] - [4,5,6] #=> [1, 2, 3]
# It's fine to add/subtract among arrays, but it's NOT OK to add/subtract among hashes!
{% endhighlight %}

<p>Apparently, the subtraction of a array from another array will take away all overlapping objects. So we can use array to represent shifts, the gaps can be obtained by simple subtraction, and the program won't go nuts if a section of the time is already assigned. The gap of the baby can be simply calculated by Array(whole day) - Array(babysitter 1) - Array(babysitter 2) - Array(babysitter 3), etc.  Let's start a 24 hr whole day for the baby, and put the array into the Hash.</p>

{% highlight ruby %}
> Array(0..23)
#=> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]  #cool, that's a whole day representing every hour.
>
> incredibles={} #=> {}              #let's create an empty Hash for the family first!
> incredibles["baby"]=Array(0..23)   #We just assign a whole empty day for the baby
{% endhighlight %}

  <p>Now let's record everybody's shift in the unit of 24-hour clock, using the exact same way we created the entire day Array. The two shifts of Rob, the Mr.Incredible, can be created by adding Arrays.</p>

{% highlight ruby %}
> incredibles["Helen"]=Array(6..15)            #Elastic girl covers 6AM ~ 3PM
> incredibles["Violet"]=Array(17..21)          #The daughter covers 5PM ~ 9PM
> incredibles["Dash"]=Array(19..23)            #The son covers 7PM ~ midnight
> incredibles["Rob"]=Array(0..7)+Array(22..23) #Mr.Incredible covers 0 ~ 7AM &amp; 10PM ~ midnight.
{% endhighlight %}

  <p>Now Kari, the babysitter asks: what time do you want me to fill? We can get the answer by a quick subtraction:</p>

{% highlight ruby %}
> incredibles["baby"] - incredibles["Helen"] - incredibles["Violet"] - incredibles["Dash"] - incredibles["Rob"]  #=> [16]
# the answer is 4pm!
{% endhighlight %}

<p>Yes, that's right, no flow controls or loops at all. It took me almost a week to comes to this solution. And theoretically speaking this algorithm doesn't even need to load everybody's shifts all at once.  Fast and easy, let's enjoy the magic power of Ruby!</P>
</div>