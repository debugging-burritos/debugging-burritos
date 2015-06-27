---
layout: article
title: "Using Google Chrome's Copy to Clipboard Feature"
author: jessica
categories: articles

modified: 2015-06-19
tags: [javascript, chrome, tips]

excerpt: I came across a helpful Google Chrome developer tool the other day that helped me debug a complex issue. 
comments: true
ads: false
image:
  teaser: articles/2015-06/copy_small.jpg
  feature: articles/2015-06/copy_large.jpg
---

I came across a helpful Google Chrome developer tool the other day that helped me debug a complex issue. 

I was working with a large data set and wanted to work with the contents of my array in a sandbox. While `console.log` allows me to easily look at the contents inside the console, I wanted to copy the contents of my data set to my clipboard so I could work with it elsewhere. In this case, I wanted to paste it into a jsFiddle in order to work out the issue in isolation.

After a quick search, I discovered the [Chrome command line API](#api-reference) provides a tool that does exactly this!

By using the [`copy`](https://developer.chrome.com/devtools/docs/commandline-api#copyobject) command, I was able to copy the contents of my array at a given state to my clipboard.

All I had to do was type this into the Chrome developer console at my breakpoint:
{% highlight console %}
copy(variableToCopy);
{% endhighlight %}

If you’re working with an object, you might want to serialize the variable into JSON like this: 
{% highlight console %}
copy(JSON.stringify(objectToCopy));
{% endhighlight %}

I recommending checking out the full list of Chrome’s command line tools here:

<a id="api-reference"></a>
<a target="_blank" href="https://developer.chrome.com/devtools/docs/commandline-api">Chrome Command Line API Reference</a>

Many of the functions could come in handy next time you’re trying to debug an issue!

[Image credits](https://www.flickr.com/photos/maurizio_mwg/2754327402/)


