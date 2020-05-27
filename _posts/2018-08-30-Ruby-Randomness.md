---
layout: post
title: Ruby Randomness
---
“We know that there is no such thing as a pure random draw, for the outcome of the draw depends on the quality of the equipment.” — Nassim Taleb
{: .message-gray }

Ever since reading Nassim Taleb’s *Fooled By Randomness*, I have tried to be wary of my day-to-day dealings with probability. Over the past few weeks, I have come across Ruby’s rand method several times and thought it would be useful to explore it below.

## Ruby’s Rand
The Ruby rand documentation is as follows:


>rand(max=0) → number
>
>If called without an argument, or if max.to_i.abs == 0, rand returns a pseudo-random floating point number between 0.0 and 1.0, including 0.0 and excluding 1.0.

As stated above, rand can be called without an argument:

{% highlight ruby %}
:001 > rand
 => 0.41025659315746954
{% endhighlight %}

Next, if rand is called with an integer argument, it will return an integer greater than or equal to 0 but less than the argument:

{% highlight ruby %}
:001 > rand(5)
 => 1
{% endhighlight %}

If rand is called with a positive floating point number argument, rand will return an integer greater than or equal to 0, but less than the closest integer that is less than or equal to the argument (Ruby forces this on the argument using .to_i):

{% highlight ruby %}
:001 > rand(4.5)
 => 1
{% endhighlight %}

Users can also use rand on ranges of numbers. For example, rand(x..y) will return an integer between x and y (including both x and y). If y needs to be excluded, rand(x…y) can be utilized instead.

{% highlight ruby %}
:001 > rand(1..10)
 => 7
:002 > rand(1...10)
 => 6
{% endhighlight %}

These applications of rand could be useful when the user needs to quickly simulate games of chance (think dice rolls) or perhaps generate a random quotation from a curated list.

Ruby also provides users with the access to a Random class, and you can use this to handle floating point numbers.

{% highlight ruby %}
:001 > prng1 = Random.new
 => #<Random:0x007fbd1e9d1830>
{% endhighlight %}

Here, you can call prng.rand(x) on a floating point number to return a floating point number that is between 0.0 and x (excluding x).

{% highlight ruby %}
:002 > prng1.rand(7.7)
 => 5.620702054027026
{% endhighlight %}

The rand method continues to work on ranges here and can return floating point numbers.

{% highlight ruby %}
:003 > prng1.rand(1.2..5.6)
 => 5.296660528148246
{% endhighlight %}

---
“Unless you have confidence in the ruler’s reliability, if you use a ruler to measure a table you may also be using the table to measure the ruler.” — Nassim Taleb discussing Wittgenstein
{: .message-gray }

## Why Ruby’s Built-in Rand Can Be Problematic

Since rand allows users to access Ruby’s pseudo-random number generator, the method has some subtleties that the user should keep in mind. In general, pseudo-random number generators use algorithms to produce sequences of numbers that appear random. The sequences, however, are actually determined by an initial parameter termed a key (or seed). Additionally, given a large amount of time, a particular sequence will eventually repeat itself. This determinism and periodicity do not make these generators ideal in situations that necessitate strict cryptographic security.

## An Alternative Method to Generating Random Numbers in Ruby

Instead of monitoring the radioactive decay of some element within the controlled lab in your bedroom, you can use the RealRand gem. This gem allows ruby users to request random numbers from several public sites that purport to host true random number generators. The sources for these generators mostly include natural phenomenon such as atmospheric noise and radioactive decay.

## Sources
Taleb, Nassim Nicholas. FOOLED BY RANDOMNESS. RANDOM HOUSE, INC, 2008.

https://ruby-doc.org/core-2.5.1/Kernel.html#method-i-rand

https://ruby-doc.org/core-2.2.0/Random.html
