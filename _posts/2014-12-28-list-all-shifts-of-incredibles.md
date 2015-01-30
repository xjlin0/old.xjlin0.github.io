---
layout: post
title: "List all shifts of Incredibles"
description: "The composition and inheritance in Ruby class"
category: tech
tags: [Ruby, inheritance]
---
{% include JB/setup %}
### The composition and inheritance in Ruby class<img src="/assets/imgs/family.jpg"  alt="BabyFacingKariFocused" width="20%"/>

<p>In the previous blog I showed how to use undefined methods in class by attr_accessor and method_missing. Let's talk more about how to use undefined methods by inheritance and composition since it's useful for the object-oriented programming. Here we define super powers of the Incredibles.</p>
{% highlight ruby %}
Welcome to IRB. You are using ruby 2.0.0p576 (2014-09-19) [i386-cygwin]. Have fun ;)
>> class Supermen
|  def instant_moving
|   "Shuuooo .......... you just moved!"
|  end
|  def shield
|   "The invisible wall is now protecting you!"
|  end  # you can list many super powers on and on....
|  end  #=> nil
>> Supermen.new.instant_moving #=> "Shuuooo .......... you just moved!"
{% endhighlight %}
<p>However, with great power there must also come great responsibility. Let's define their busy life here, and supermen get super power by the inheritance. Notice the <em><strong>"< Supermen"</strong></em> below: </p>
{% highlight ruby %}
>>class Incredibles < Supermen
|  attr_accessor :schedule
|  def add(shift)
|    @schedule << shift
|  end
|  end  #=> nil
>> Incredibles.new.instant_moving #=> "Shuuooo .......... you just moved!"
{% endhighlight %}
<p>See, there's no method definition of super powers, but we can still use it because the inheritance in Ruby class. That's convenient!. But wait, they also got an intensive baby sitting schedule mentioned in my previous blog too:</p>
{% highlight ruby %}
>> Helen_shift, Violet_shift, Dash_shift, Rob_shift = Array(6..15), Array(17..21), Array(19..23), Array(0..7)+Array(22..23)  #This are their shifts in my previous blog.
>> baby=Incredibles.new
>> baby.schedule=[]
>> baby.add(Helen_shift)
>> baby.add(Violet_shift)
>> baby.add(Dash_shift)
>> baby.add(Rob_shift)
{% endhighlight %}
<p>What a busy schedule of the baby sitting! Don't you want to list all their shifts? Let's try it with each method.  But wait, we didn't define the <code>each</code> method so Ruby will reject our request of using <code>baby.each</code>.  Is there a way to list all of their shifts?  It would be so nice if we can use all Enumerable methods here.  In Carlson & Richardson's Ruby Cookbook Chapter 9.4, they showed a pretty <em><strong>mixin</strong></em> by adding the "include" and defining "each" to use all 22 Enumerable methods in one shot.</p>
{% highlight ruby %}
>> class Incredibles < Supermen
|  include Enumerable
|  def each
|    @schedule.each {|a| a.each{|b| yield b}}
|  end  # This each invokes block by calling the yield function.
|  end  #=> nil
>> baby.include?(12) #=> true  # noon shift is covered
>> Array(0..23) - baby.sort #=> 16  #4pm is not covered!
>> baby.include?(16) #=> false #Indeed, 4pm's not covered.
{% endhighlight %}
<p>Now we can marvellously use all core methods defined in the Enumerable module! Because there's no multiple inheritance in Ruby, in one customized class we can use methods from many other classes by <em><strong>mixin</strong></em>.  Isn't that great?</p>
<p>By the <em><strong>inheritance</strong></em> the class got all methods defined in the father class, for example, Incredibles <em><strong>ARE</strong></em> supermen. By mixin we got specific methods from other class, and that's the idea of the <em><strong>composition</strong></em>, say, Incredibles schedules' <em><strong>HAVE</strong></em> Enumerable methods. There are many people hotly debating either inheritance or composition is better. In my point of view, as long as the handling of the repetitive codes is elegant, highly readable and maintainable, either or both ways can be applied in certain conditions.</p>
