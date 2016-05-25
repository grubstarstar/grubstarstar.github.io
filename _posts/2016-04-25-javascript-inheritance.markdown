---
layout: post
title:  "JavaScript object Inheritance"
date:   2016-04-25 12:39:00
categories: rich
---

JavaScript can emulate the classical class hierarchy of more traditional OOP programming languages. The JavaScript object model is quite different from these in terms of it's underlying mechanisms though, so it's important to get a grasp on how it all fits together first of all before we demonstrate how to create "class" hierarchies.

Everything in JavaScript is an object. This includes functions, which can be stored in variables and passed as arguments to other functions, or pretty much anything that you could do with a normal variable. This makes JavaScript great for systems that use callbacks a lot such as node.js.

The traditional way to create JavaScript constructor is just to create a normal function, but use the implicitly declared 'this' reference to refer to the object instance that the the constructor will create. You can then use the 'this' reference to attach properties to the object that it creates. As functions are also objects, you can make them properties of the object which effectively acts like a method would in traditonal OOP languages. You can see this in the 'setTuning()' method in the example below. Notice that in order for the function you created to act as a constructor and instantiate the new guitar object, you need to use the 'new' keyword.

{% highlight javascript %}
function Guitar(args) {
	this.make = args.make;
	this.model = args.model;
	this.tuning = 'E';
	this.setTuning = function(tuning) {
		this.tuning = tuning;
		console.log("Tuning %s %s to %s", this.make, this.model, this.tuning);
	}
}
var lucille = new Guitar({ make: "Gibson", model: "ES-335" });
lucille.setTuning("Eb");
{% endhighlight %}

There are no such things as classes in JavaScript, just objects. We can implement the concept of inheritence through the use prototypes which is the backbone of Javascript's object model.

Every object in JavaScript has a prototype chain, which is how behaviour and properties are inhertited. In Firefox and Chrome's V8 engine this is exposed on every object via the '__proto__' property. As node.js also uses Chrome's V8 engine it is also exposed through the same property there. Whenever you write some JavaScript to access a property on an object, whether it be a variable or a method, athe JavaScript engine will take the following steps to try to resolve it:

* If the object (let's call it o') itself has the property that will be used directly.
* Otherwise, the engine will attempt to find the property on the direct prototype of that object, accessable throuhg 'o.__proto__', and use that if found
* Otherwise, the engine will look at 'o.__proto__.__proto__' and see if that object has the property.

This continues until one of the objects has the property, or it reaches the end of the prototype chain, which case 'undefined' is returned.

Of course the code calling the proprty on object 'o' doesn't see any of this, and if the properrty is found at any point in the chain it looks as though the property was on 'o' itself, which allows us to effectively inherit behviour from ancestors in the prototype hierarchy in a similar way to classic OOP langauges.

We can artificially show this by creating a nested prototype chain as object literals where the last ancestor has the property 'someProperty'. In reality you wouldn't do this because you'd probably want multple objects to be inherting from the parent objects otherwise you'd just have the property in the child itself. But it demonstrates how a call to 'o.somePoperty' will tranverse the hierachy and apear as though the property belongs to the object 'o'.

{% highlight javascript %}
var o = {
		__proto__: {
				__proto__: {
						someProperty: 123
				}
		}
}
// o.someProperty => 123
console.log(o.someProperty);
{% endhighlight %}

Because the traversal of the chain stops once a match is found, we can override properties and methods simply by implementing a property of the same name in a decendent object.

{% highlight javascript %}
var o = {
		__proto__: {
				someProperty: "Something Else",
				__proto__: {
						someProperty: 123
				}
		}
}
// o.someProperty => "Something Else"
console.log(o.someProperty);
{% endhighlight %}

In the above example you can see that the implementation of the middle prototype will override it's parent's implementation. This is commonly used in classic OOP and is useful in JavaScript too.


Every function object that is created will have a property called 'prototype'. This property is used when that function is called as a constructor, and represents a kind of object template for the object that that constructor creates. 

{% highlight javascript %}
lucille.__proto__ === Guitar.prototype;
{% endhighlight %}


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
Parent.prototype.methodA = function() {
	console.log('calling methodA()')
}

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
Child.prototype.methodB = function() {
	console.log('calling methodB()')
}

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
