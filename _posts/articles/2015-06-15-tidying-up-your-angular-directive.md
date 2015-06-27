---
layout: article
title: "Tidying up Your Angular Directive"
author: guerric
categories: articles

modified: 2014-06-15
tags: [angular, directives]

excerpt: Directives are the place to bind events directly to DOM elements in your application. But what happens when your directive is destroyed?
comments: true
ads: false
image:
  teaser: articles/2015-06/robot_hand_small.jpg
  feature: articles/2015-06/robot_hand_large.jpg
---

Directives are the place to bind events directly to DOM elements in your application. But what happens when your directive is destroyed? Do those events get cleaned up by AngularJS? Short answer: Nope. Let's look at a sample directive to frame our conversation:

JavaScript:
{% highlight javascript %}
angular.module('blogPost', []);
angular.module('blogPost').directive('withJsEvents', function() {
  return {
    restrict: 'E',
    link: function(scope, element, attrs) {
      element.on('click', onClick);
    }
  };
  
  function onClick() {
    console.log('Clicked our element!');
  }
});
{% endhighlight %}

HTML:
{% highlight html %}
<with-js-events></with-js-events>
{% endhighlight %}

If we run our directive in a simple application, we might not encounter any issues. However, if we end up using the directive many times, we'll start to encounter memory leaks as the directives are created and destroyed. To clean up after ourselves, we'll use a convenience event that AngularJS provides: `$destroy`

{% highlight javascript %}
// Let's just examine the link function for clarity.
function link(scope, element, attrs) {
  element.on('click', onClick);
  scope.$on('$destroy', function() {
  
    // clean up when the directive is destroyed
    element.off('click', onClick);
  });
}
{% endhighlight %}

Now we are removing the directive's DOM events when the directive is removed. We may also want to clean up generated DOM element events  before our directive is removed. To do this, you can listen to the `$destroy` event on the element itself.

{% highlight javascript %}
// Again, let's just examine the link function.
function link(scope, element, attrs) {

  // create a new span element and add it to the directive content
  element.append('<span></span>');
  var span = element.find('span');
  
  // for some reason we want to set some text in the span on click
  var setSpanText = function() {
    span.text('Debugging Burritos @ ' + new Date());
  };
  element.on('click', setSpanText);
  span.on('$destroy', function() {
  
    // clean up when the element is destroyed
    element.off('click', setSpanText);
  });
}
{% endhighlight %}

## Wrapping up
We've seen two ways to clean up events from within our directives. Depending on your use case, you'll want to adopt one or both of these approaches in your project. Now go tidy up your messy directives!


Image credits unknown

