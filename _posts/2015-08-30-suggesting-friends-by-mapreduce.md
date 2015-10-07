---
layout: post
title: "Suggesting friends by MapReduce"
description: "Find out 'people you may know' by user's friend list"
category: tech
tags: [Parallel, Spark, Python, MrJob, MapReduce, algorithm]
---
{% include JB/setup %}
### Implemented in Apache Spark or Hadoop MrJob. <img src="/assets/imgs/avengers.jpg"  alt="The Incredibles friends from Pixar" width="20%"/>

One interesting example to practice is the classical question: "The people you may know". On apps similar to Facebook, you got users and their friends list as the followed.  How to suggest user A and E to make friend to each other but not to User K? (based on User A and E get several common friends B, C, & D, and we assume only mutual friendship existed.)

```
A     B, C, D, G  #User A has user B/C/D/G as friends
E     B, C, D
K     L, J, G
.....
```
Let's take a look at the input again, since we assume only mutual friendship existed, the real input would extend to the following, how would you solve it?

```
A     B, C, D, G
B     A, E   #Because user B is user A and user E's friend.
C     A, E
D     A, E
E     B, C, D
G     A, K
J     K
K     L, J, G
L     K
```

The most straightforward approach is to iterate all users.  For each user, compare his/her friends to **all other users** for suggestion. This approach is feasible if applied asynchronously because people adding friends at different time.  However, if there's ton's of users/friends and you wanna to calculate all suggestions at once, a more scalable approach is required.

[MapReduce](https://en.wikipedia.org/wiki/MapReduce) is one of Google's approaches for processing big data, and currently there are many implementations based on the idea, such as [Apache Hadoop](https://en.wikipedia.org/wiki/Apache_Hadoop) or [Spark](https://en.wikipedia.org/wiki/Apache_Spark), etc.  If we can find a way to count common friends, independently, we can split such big job to many workers and make it parallel.

What is the "independently" mean? During the processing of each line in the input, the straightforward approach needs to compare a line to **another** line. That is not independent. There is actually a way to count common friends of one line, **without** reading all other lines. In such way huge inputs can be split to different workers for parallel processing without inter communication in the beginning of the common friend counting. How is it possible? It's based on one fact: A certain user's friends all have one common friend, which is that certain user.

```
A     B, C, D, G
B & C have one common friend (A)
B & D have one common friend (A)
B & G have one common friend (A)
C & D have one common friend (A)
.....so on so forth....
```

Thus when reading line for user A, I first make all permutation pair within that line.  There are two kinds of pair here:

-  [*connected*] Those who are already connected friends, and I assign their 'common friends count' as a very negative number -9999999999 as my personal label for whoever are already friends. Thus I won't suggest friendship request for users who are already friends, since 9999999999 is bigger than the world population.

-  [*commons*] Those who have user A as the common friend, and I assigned these pair's "common friends count" as one, since they have **one** common friend user A. Here is the example:

{% highlight python %}
# input (first line):   A: B, C, D, G
connected = [((A, B), -9999999999), ((A, C), -9999999999), ((A, D), -9999999999), ((A, G), -9999999999)]
commons   = [
              ((B, C), 1), ((C, B), 1), ((B, D), 1), ((D, B), 1), ((B, G), 1), ((G, B), 1),
              ((C, D), 1), ((D, C), 1), ((C, G), 1), ((G, C), 1),
              ((D, G), 1), ((G, D), 1),
            ]
return connected + commons
# repeat for all lines
{% endhighlight %}

You may think, wow, why generate or **map** to so many ((pair), count) from one single line? Is it necessary?  The point is such mapping process is independently line to line, getting their counts (-9999999999 or 1) doesn't depend on reading any other lines. Thus the mapping process of huge input can split into many workers for parallel processing.

The next step is to use the pair as the key, sum up or **reduce** their counts. So after reduce, the sum of these pair from all three lines would like the following: Totally there are two pairs of (A, B), and the sum of the two pairs would be -19999999998. There would be 3 pairs of (A, E), and the sum of the three pairs would be 3. Please note here the summing/reduce process can be executed on different workers, the final reduced result won't be changed.

{% highlight python %}
# Some sums of the pairs (reduced results)
((A, B), -19999999998),  #the pair are generated from user A and user B, the counts are the sum.
((A, C), -19999999998),
((A, D), -19999999998),
((A, G), -19999999998),
((E, B), -19999999998),
((A, E), 3),  #The pair user A and user E have counts 3 from the lines of user B/C/D
((E, A), 3),  #permutation, not combination. so you can also suggest user E to make friend with user A later.
((B, C), 2),  #they have 2 common friends: A, E
((C, B), 2),
((A, K), 1),  #Generated from the line of user G
#....
{% endhighlight %}

Now we see the pattern: The reduced results, which is the counts, are the number of common friends. Don't forget to filter the positive counts and the pair will be the suggestions with the common friend count. We just need to group the same leading users and suggest the most suggested pairs based on their counts.

Based on the same algorithm described above, here are my two implementations, by using two different frameworks. The first one is Python API to the cluster computing framework [Apache Spark](http://spark.apache.org). It comes with many handy functions such as groupByKey() or takeOrdered() for grouping pair-counts or choosing the top pair-counts. You can check [my PySpark code here](https://github.com/xjlin0/cs246/blob/master/w2015/hw1/q1_people_you_may_know_spark.py).  In addition, by using [**Ruby-Spark** gem](http://ondra-m.github.io/ruby-spark/), I also coded a [Ruby-Spark solution](https://github.com/xjlin0/cs246/blob/master/w2015/hw1/q1_people_you_may_know_spark.rb) for you to compare.

My second implementation used [MRJob](https://github.com/Yelp/mrjob), the Yelp's Python API controlling Hadoop for MapReduce tasks. There are not too many built-in functions so I have hard time to implement the groupByKey() or top counts. After struggling here is my working code of groupByKey() in Yelp MrJob:

{% highlight python %}
# Input:  ((pair), [count] )
# Please notify the count are wrapped in the []

def groupByKey(self, key, values):
  return key, reduce(lambda a, b: a + b, values)

# [ ] + [ ] is possible, so ((pair), [a] ) and ((pair), [b] ) will be ((pair), [a, b] )
{% endhighlight %}

I used the [Python's Counter object](https://docs.python.org/2/library/collections.html) to get top counts by its most_common() function. Welcome to check my [MRjob implementation](https://github.com/xjlin0/cs246/blob/master/w2015/hw1/q1_people_you_may_know_mrjob.py). Enjoy!
