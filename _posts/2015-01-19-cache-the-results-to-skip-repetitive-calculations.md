---
layout: post
title: "Cache the results to skip repetitive calculations"
description: "Memoization for the functional programming"
category: tech
tags: [Ruby, JavaScript, cache, Memoization]
---
{% include JB/setup %}
### Memoization for the functional programming<img src="/assets/imgs/megamemory.jpg"  alt="BabyFacingKariFocused" width="20%"/>

<div id="article">
<p><a href="http://en.wikipedia.org/wiki/Memoization#Functional_programming">Memoization</a>, storing something in the memory, is very important in the functional programming to avoid repetitive calculation.  Such process, sometimes can be used as the cache mechanism, can save a lot of time, since the memory can be used as containers for expansive calculations or slow I/O results. Yesterday I read <a href="http://www.quora.com/What-are-some-cool-Ruby-tricks">a magic Ruby trick to realize the memoization</a> and would like to try here:</p>
{% highlight ruby %}
Welcome to IRB. You are using ruby 2.0.0p576 (2014-09-19) [i386-cygwin]. Have fun ;)
>> fact = Hash.new{ |h,k| h[k] = ( k<=1 ? 1 : k * h[k-1] ) }
>> fact[400]  #=> instant result of factorial!

>> fibo = Hash.new{ |h,k| h[k] = ( k<=2 ? 1 : h[k-2] + h[k-1] ) }
>> fibo[500]  #=> instant result of Fibonacci !

>> def fib(n)  # let's compare with recursive
    return 1 if n <= 2
    fib( n - 1 ) + fib( n - 2 )
>> end
>> fib(500)  #=>  This will probably freeze your computer, please don't try it.
{% endhighlight %}
<p>See, when the new hash is declared here, the hash value is calculated only to the previous 1 or 2 elements.  Since most of the calculation results are stored in the hash, the complexity of calculating Fibonacci here is only <code>O(n)</code>, or even less if repeat.  However, using recursive to to calculate Fibonacci will get much higher <code>O(2^n)</code> for each calling, what a difference!  Another cool way is to utilize Ruby's <code>Fiber</code> class since it 'freezes' upon yield, shown in Mat's <a href="https://books.google.com/books?id=jcUbTcr5XWwC">The Ruby Programming Language</a></p>
{% highlight ruby %}
Welcome to IRB. You are using ruby 2.0.0p576 (2014-09-19) [i386-cygwin]. Have fun ;)
>> fib = Fiber.new do
>>   x, y = 0, 1
>>   loop do
>>     Fiber.yield y
>>     x,y = y,x+y
>>   end
>> end
>>
>> 500.times { puts fib.resume }  #=> instant result of Fibonacci!
{% endhighlight %}
<p>Of course we can implement the memoization in JavaScript. Here are the same factorial and Fibonacci using memoization, by the same concept:</p>
{% highlight javascript %}
node v0.10.13 console
> var facto = { 0:1, 1:1 };
> function fac(n){
  if (!facto[n])  facto[n] = n * fac(n-1);

  return facto[n]       }
> fac(170) // instant result of factorial!

> var fibon = { 0:0, 1:1, 2:1};
> function fib(n){
  if (!fibon[n])  fibon[n] = fib(n-2) + fib(n-1);

  return fibon[n]  }
> fib(170) // instant result of Fibonacci!
{% endhighlight %}
<p>What does it compare to conventional looping method? You would think?  Well, for new calculations, a looping method calculating Fibonacci also gets the complexity of <code>O(n)</code> as well, but if more calculations on the way, the stored results in the memory will function as the <strong><em>cache</em></strong> and less processor power or time will be needed, compared to the looping without memoization shown below.</p>
{% highlight javascript %}
  node v0.10.13 console
  > function fibl(n){
    if (n < 3)  return 1;
    var last1 = 1, last2 = 0, sum = 0;
    for (var i=2; i < n+1; i++){
      sum = last1 + last2, last2 = last1, last1 = sum;
    }
   return sum;      }
  > fibl(170)  // calculation result of Fibonacci without the cache.
{% endhighlight %}

</div>