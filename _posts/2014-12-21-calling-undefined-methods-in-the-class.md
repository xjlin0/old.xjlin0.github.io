---
layout: post
title: "Calling undefined methods in the class"
description: "never miss a call again!"
category: tech
tags: [Ruby, class, method]
---
{% include JB/setup %}
### never miss a call again!<img src="/assets/imgs/family.jpg"  alt="BabyFacingKariFocused" width="20%"/>

<p>In Ruby there is an excellent way to organize tons of methods we created: Class. We can group some relating methods in class and calling them for easier usage.  Moreover, there is an even better things happening in the class: in certain conditions you can call the methods you have NOT created.</p>
<p>"Cut the crap", you might think: "in Ruby we can  add new methods to existing class (or by mixin/inheritance), say, add my special methods to the ordinary String class, so I can enjoy tons of core methods in String class plus some customized methods, in this way of course I can use the methods not-declared in the new defined String class."</p>
{% highlight ruby %}
Welcome to IRB. You are using ruby 2.0.0p576 (2014-09-19) [i386-cygwin]. Have fun ;)
> class CreditCard
|  attr_accessor :num
|  def check
|   @num.to_s.scan(/\d/).map(&:to_i).each_slice(2).inject(0){|total,obj| total + obj[0]*2 + obj[1]} % 10 == 0
|  end
|  end  #=> nil
> test=CreditCard.new
> test.num=4111111111111111 #=> 4111111111111111
> test.check #=> true
> test.num=4111111111111113 #=> 4111111111111113
> test.check #=> false
{% endhighlight %}
<p>Wait a second, there's more: Ruby is so considerate and prepare the getter and setter methods for everybody.  In the above example of validating credit card numbers by <a href="http://en.wikipedia.org/wiki/Luhn_algorithm">Luhn algorithm</a>, we can put attr_accessor :num so we can dynamically assign the instant variable @num by calling non-defined .num method, which Ruby has taken care of. This is one classical example to use undefined methods.</p>
<p>Another example will be the method_missing. This is a marvellous method to be called when there no such method. We can use this for building user friendly API and make the usage of our methods quite intuitive. Please check the following example:</p>

{% highlight ruby %}
>> class CreditCard
|  def self.method_missing message
|   message.to_s.scan(/\d/).map(&:to_i).each_slice(2).inject(0){|total,obj| total + obj[0]*2 + obj[1]} % 10 == 0
|  end
|  end #=> nil
>> CreditCard.number4111111111111111 #=> true
>> CreditCard.number4111111111111112 #=> false
>> CreditCard.DBC-SF-4563-9601-2200-1999-Bob #=> true
{% endhighlight %}
<p>Splendid!  We certainly didn't define those strange named methods, but when Ruby received the strange names as the methods, it actually delivered the names to method_missing and here processed as credit card numbers.  Of course we will have to pay extra attention since there won't be NoMethod error any more. We can talk more about it for meta-programming. Have a wonderful holiday!</p>
