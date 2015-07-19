---
layout: article
title: "Working With Protractor Elements Interactively"
author: jessica
categories: articles

modified: 2015-06-30
tags: [javascript, angular, protractor, tips]

excerpt: Running protractor with the element explorer option instead of running the entire test suite.       
comments: true
ads: false
image:
  teaser: articles/2015-07/protractor_small.jpg
  feature: articles/2015-07/protractor_large.jpg
---


When I was learning protractor, one of the most painful parts was figuring out the correct selector to use and how to interact with an element on the page. It usually involved writing a small bit of code, and then repeatedly running the tests to catch and fix errors.  End to end tests are slow so relying on them to catch errors means your development process also becomes slow.

Luckily, new versions of protractor support the `elementExplorer` option, which loads up the URL on WebDriver and puts the terminal into a REPL loop.  A REPL loop, or read-eval-print loop, is an interactive environment that takes your input, evaluates it, and returns the result back to you.  According to the angular documentation, you can simply run protractor as usual and pass in the `--elementExplorer` flag:

{% highlight console %}
protractor --elementExplorer
{% endhighlight %}

For some reason I was not able to get it working that way, but I was able to get it working by reinstalling protractor and running these commands:

{% highlight console %}
npm install protractor
./node_modules/protractor/bin/webdriver-manager update \ --chrome
./node_modules/protractor/bin/webdriver-manager start
./node_modules/protractor/bin/protractor --elementExplorer
{% endhighlight %}

Once Protractor starts up, you should see a `>` prompt.  The browser, element function, and protractor variables should all be available. With the available browser instance, you can manually navigate to the page you want to work with or request it from the terminal.

{% highlight console %}
> browser.get('http://www.angularjs.org')
{% endhighlight %}

Once you are on the page you want to test, you can try out commands you would use in your tests.

{% highlight console %}
> element(by.id('foobar')).getText()
{% endhighlight %}

Overall, protractor’s element explorer is a great way to speed up development time when writing tests.  It is a convenient way to try out commands without having to rerun the entire test suite.

[Image credits](https://www.flickr.com/photos/faceme/16890516270/)