---
layout: post
title:  "JavaScript object Inheritence? Don't mind if I do"
date:   2015-11-28 10:39:00
categories: rich
---
This is a test

{% highlight perl %}
my @a = (1,2,3);
for(@a) {
	print $_."\n";
}
#=> prints the array
{% endhighlight %}

Is this parent constructor run before the child's?
{% highlight javascript %}
/* define the parent 'class' */
var Parent = function(args) {
	console.log('Parent() constructor called')
	this.a = args.a
}
Parent.prototype.methodA = function() { console.log('calling methodA()')}

/* define the child 'class' */
var Child = function(args) {
	// call the Parent constructor
	this.__proto__.constructor(args)
	console.log('Child() constructor called')
	this.b = args.b
}
// inherit from the Parent prototype
Child.prototype = Object.create(Parent.prototype);
// create method on Child prototype
Child.prototype.methodB = function() { console.log('calling methodB()')}

/* create child and check */
var child = new Child({ a: 1, b: 2 });
console.log("child.a", child.a)
console.log("child.b", child.b)
child.methodA()
child.methodB()
{% endhighlight %}

Check out the [My blog][http://blog.richgarner.net] for stuff. 

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
