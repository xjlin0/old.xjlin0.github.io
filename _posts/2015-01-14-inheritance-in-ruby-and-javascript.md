---
layout: post
title: "Inheritance in Ruby and JavaScript"
description: "OOP in two languages"
category: tech
tags: [Ruby, JavaScript, inheritance]
---
{% include JB/setup %}
### OOP in two languages <img src="/assets/imgs/iclass.jpg"  alt="BabyFacingKariFocused" width="20%"/>
<div id="article">
  <p>Ruby is a class-based language, thus it is straightforward to create objects in classes for inheritance as described in my previous posts. However, it takes some steps to mimic classes in JavaScript, which is a prototype-based language.  Let's do some comparison.  Here is how we do inheritance in Ruby:</p>

 {% highlight ruby %}
  Welcome to IRB. You are using ruby 2.0.0p576 (2014-09-19) [i386-cygwin]. Have fun ;)
  >> class Supermen
    def fast_moving
      "Shuuooo ..........you just moved!"
    end
  >> end
  >> class Incredibles < Supermen
    def wear_uniform
      "The red and black looked so nice!"
    end
  >> end

  >> clark = Superman.new
  >> clark.fast_moving #=> "Shuuooo ..........you just moved!"
  >> clark.wear_uniform  #=> NoMethodError.

  >> rob = Incredibles.new
  >> rob.fast_moving #=> "Shuuooo ..........you just moved!"
  >> rob.wear_uniform #=> "The red and black looked so nice!"
{% endhighlight %}
  <p>In JavaScript a popular way to mimic class is through constructor functions and use new functions to create instances, and inherit such "classes" by the prototype assignment:</p>
{% highlight javascript %}
  node v0.10.13 console
  > function Superman(){
    this.fast_moving = "Shuuooo ..........you just moved!";
  }
  > function Incredibles(){
    this.wear_uniform = "The red and black looked so nice!";
  }
  > Incredibles.prototype = new Superman();

  > var clark = new Superman();
  > clark.fast_moving;
  'Shuuooo ..........you just moved!'
  > clark.wear_uniform;
  undefined

  > var rob = new Incredibles();
  > rob.fast_moving;
  'Shuuooo ..........you just moved!'
  > rob.wear_uniform;
  'The red and black looked so nice!'
{% endhighlight %}
  <p>Functions in JavaScript are special and can be used as classes sometimes. In the above example we start two functions by constructors, and assign prototype to inherit. Therefore Rob can use Superman's methods by such assignment without pre-definition.</p>
  <p>However such prototype assignment will result wrong <code>instanceof</code> results. For example, in the above case the return values of <code>rob instanceof Superman</code> and <code>rob instanceof Incredibles</code> would be both true, because the constructor of the Incredibles points to Superman after the prototype assignment. Some people suggest to use <code>Incredibles.prototype.constructor = Incredibles</code> after the prototype assignment to correct the wrong chaining.</p>
</div>