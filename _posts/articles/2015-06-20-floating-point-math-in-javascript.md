---
layout: article
title: "Floating Point Math in JavaScript"
author: jessica
categories: articles

modified: 2015-06-20
tags: [javascript]

excerpt: Do you know how to correctly add together decimals values in JavaScript? 
comments: true
ads: false
image:
  teaser: articles/2015-06/math_small.jpg
  feature: articles/2015-06/math_large.jpg
---

How do you correctly add two decimals values in Javascript so that `.01 + .02 = .03` instead of `.01 + .02 = 0.30000000000000004`? Before I skip to the answer, let's take a look at what's happening.

##Why does this happen?
Javascript uses a binary floating-point representation of numbers which cannot exactly represent a number like `.01`. As a result, it approximates many decimals fractions which result in rounding errors.

Think about an amount like 1/3 - There's not an exact decimal equivalent so we use `.33`, but `.33 + .33 + .33` do not add up to 1. Likewise, binary numbers cannot accurately represent all decimals numbers. Computers use binary numbers because they are faster and in most cases, a rounding error in the 17th decimal place doesn't matter. You can read more about floating point arithmetic [here](http://floating-point-gui.de/basic/).

##Solution 1: Using a multiplier
If you’re working with money, one solution is to multiply your dollar values by 100 and work with the values in cents. For example, `$1.01 + $1.02` would become `101 cents + 102 cents = 203 cents`. Then we can divide the sum by 100 in order to see the dollar amount `203 cents / 100 = $2.03`.  This works because integer values are exact in floating point math. Using whole numbers will not result in any rounding errors. In your code, this option might look something like this:

{% highlight javascript %}
((1.01*100) + (1.02*100))/100
{% endhighlight %}

However, this method can become difficult to use if you are working with decimals that have a variable number of digits after the decimal point. For example, if we were to add `1.02 + 2.032 + 1.000000004`, we would have to make sure we multiply each number by `100000000` to convert all the numbers into integers.

##Solution 2: Use toFixed()
Another option is to use `Number.toFixed` which will round the result to specified number of decimal places. Applying this solution to the previous example, we could do something like this:

{% highlight javascript %}
Number((1.02 + 2.032 + 1.000000004).toFixed(2));
{% endhighlight %}

Here, we are taking the sum of the numbers and then calling `toFixed` which allows us to round to the sum to certain number of decimal places. Since `toFixed` returns a string, we need to convert the string back into a number.

The downside to this approach is that `toFixed` can sometimes round a decimal value of `5` down instead of up. For example:

{% highlight javascript %}
Number((1.005).toFixed(2)); // 1 instead of 1.01
{% endhighlight %}


##Solution 3: Using exponential notation
After reading about the `toFixed` rounding error, I came across this great [post by Jack Moore](http://www.jacklmoore.com/notes/rounding-in-javascript/). His post provides a solution which involves using numbers represented in exponential notation.  In this example, we want the sum to be rounded 2 decimal places.

{% highlight javascript %}
Number(Math.round(1.02 + 2.032 + 1.000000004 +'e2')+'e-2');
{% endhighlight %}

Jack also shows how to abstract the function into a reusable function:

{% highlight javascript %}
function round(value, decimals) {
    return Number(Math.round(value+'e'+decimals)+'e-'+decimals);
}
{% endhighlight %}

More on decimal rounding can be found on the [MDN documentation page](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/round).

##Summary
All three solutions have pros and cons:
<br>Solution #1 is precise but sometimes requires additional work to figure out the right multiplier.
<br>Solution #2 is easy to work with, but does have a small rounding error.
<br>Solution #3 is precise and easy to work with if you abstract it into a resuable function, but a little more difficult to understand.

I encourage you to try out all three and see which one works best for your use case.

[Image credits](https://www.flickr.com/photos/infomatique/6053082373)

