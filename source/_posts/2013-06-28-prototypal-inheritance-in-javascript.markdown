---
author: sunilkumarn
comments: true
date: 2013-06-28 09:06:11+00:00
layout: post
slug: prototypal-inheritance-in-javascript
title: Prototypal Inheritance in Javascript
wordpress_id: 1184
categories:
- Javascript
---

When I decided to get my hands on Javascript, the biggest road block for me was to understand and follow the prototypal inheritance model in Javascript. Coming from a Ruby background, the inheritance model I learned and understood was the classical one. The prototypal model had fundamental differences and did run me nuts at times. Here I try to make some note of what its all about going along a few snippets.

**What exactly is a prototype?**
As explained in 'Javascript: The Definitive Guide' - An object’s prototype is a reference to another object from which properties are inherited. Every JavaScript object has a second JavaScript object (or null, but this is rare) associated with it. This second object is known as a prototype, and the first object inherits properties from the prototype.

In Javascript, an object can be created with object literals, with the new keyword, and (in ECMAScript 5) with the Object.create() function



	
  1. obj = { }                                                    //object literals, objects created will have Object.prototype as their prototype

	
  2. obj = Object.create(args, [args2])        //Object.create(), Objects created like this will have the first argument to the create method as their prototype

	
  3. obj = new Constructor invocation     //new Keyword, objects created this way will have the value of the prototype property of the constructor function as their prototype


**Object.prototype**: Its one of those rare objects which do not have a prototype, and hence is not associated with any other object.

Lets try and study the prototype of objects created via each of these 3 ways.
1) **object literals**, **{ }**

{% highlight javascript %}
obj = { }
{% endhighlight %}

obj will have always Object.prototype as its prototype. Inspecting obj in your console, and you will find this.

[![Prototype1](http://sunilkumarn.files.wordpress.com/2013/06/prototype11.png?w=570)](http://sunilkumarn.files.wordpress.com/2013/06/prototype11.png)

**__proto__** is the prototype object ( Object.prototype) from which obj inherits the properties.

You can always use Object.getPrototypeOf to check the prototype of an object. The below screenshot shows the prototype object of obj.

[![prototype2](http://sunilkumarn.files.wordpress.com/2013/06/prototype2.png?w=570)](http://sunilkumarn.files.wordpress.com/2013/06/prototype2.png)

2)** Object.create()**

{% highlight javascript %}
obj = Object.create(Object.prototype)
{% endhighlight %}

Doing this is exactly the same as creating an object with literals( obj = { }). This is because the first argument to the create method forms the prototype of the newly created object, obj. In our statement we have specified Object.prototype as the first argument to be used. Also note that Object.create also takes a second optional argument which defines the properties of the object.

We could specify an object or null as the first argument to the create method. If we pass null as the first argument to create, then the newly created object won't inherit any property.

{% highlight javascript %}
 Mammal = Object.create(null, {knownTime: { value: 'infinity' }} )
{% endhighlight %}

We have an object Mammal with no inherited properties. It has a property of its own, (knownTime with a value set to 'infinity').

{% highlight javascript %}
Man = Object.create(Mammal)
{% endhighlight %}

Object.getPrototypeOf(Man) will return an object with a single property, knownTime. Also,

{% highlight javascript %}
Man.knownTime
   -> 'infinity'
{% endhighlight %}

Now create another object inheriting from 'Man'

{% highlight javascript %}
John = Object.create(Man)
John.knownTime
   -> 'infinity'
 {% endhighlight %}

Now, lets fiddle around these objects.
Try adding new properties to these objects.

Set a max height of Mammals.

{% highlight javascript %}
Mammal.maxHeight = '200'
{% endhighlight %}


{% highlight javascript %}
Man.maxHeight
   -> '200'
John.maxHeight
   -> '200'
{% endhighlight %}

Set a nice rhythmic vocal for mammals.

{% highlight javascript %}
Mammal.sound = function() { return( "Wholaa!" ) }
{% endhighlight %}


{% highlight javascript %}
Mammal.sound()
  -> "Wholaa!"
Man.sound()
  -> "Wholaa!"
John.sound()
  -> "Wholaa!"
{% endhighlight %}

Sure, Man has a different kind of a pitch from the other mammals.

{% highlight javascript %}
Man.sound = function() { return( "Cheerpp!" )}
{% endhighlight %}


{% highlight javascript %}
Man.sound()
   -> "Cheerpp!"
John.sound()
   -> "Cheerpp!"
{% endhighlight %}

Object.getPrototypeOf(John) returns an object(Man - with a property 'sound') which has another object(Mammal - with properties 'knownTime, maxHeight and sound') as its prototype.

[![prototype3](http://sunilkumarn.files.wordpress.com/2013/06/prototype3.png)](http://sunilkumarn.files.wordpress.com/2013/06/prototype3.png)















The prototype chain looks as follows

[![Prototype_Chain4 - New Page](http://sunilkumarn.files.wordpress.com/2013/06/prototype_chain4-new-page.png?w=570)](http://sunilkumarn.files.wordpress.com/2013/06/prototype_chain4-new-page.png)

Object **Mammal** has properties 'knownTime', 'maxHeight' and and the method 'sound'.  **Man** has Mammal as its prototype object and hence inherits these properties from Mammal.  However, it overrides the 'sound' method. **John **has Man as its prototype object and hence inherits properties from Man(which includes Man's own properties and properties inherited from Mammal).

3) **new keyword followed by a Constructor Invocation**

Try this,

{% highlight javascript %}
obj = new Object()
{% endhighlight %}

Here again, this is exactly the same as Object.create(Object.prototype) and {}. The prototype of the object, obj is Object.prototype.

Here, Object is the constructor which when invoked using the new keyword will return a new object which inherits properties from the prototype of the constructor (here, Object.prototype).

**Constructor**(as per the Definitive Guide): A constructor is a function designed for the initialization of newly created objects. Constructors are invoked using the new keyword. Constructor invocations using new automatically create the new object, so the constructor itself only needs to initialize the state of that new object. The critical feature of constructor invocations is that the prototype property of the constructor is used as the prototype of the new object.

Also, note that every JavaScript function (except functions returned by the EC-MAScript 5 Function.bind() method) automatically has a prototype property. Hence, a constructor being merely a function also has the prototype property.
To make this more sense, lets create a constructor and try and figure out what exactly happens.

{% highlight javascript %}
function Startup(config) {
   this.config = config ;
   this.motto = function(verbiage) {
      return("Our motto is" + verbiage)
    }
 }
{% endhighlight %}

Now, create a new instance

{% highlight javascript %}
s1 = new Startup(true)
{% endhighlight %}

s1 is an object with Startup.prototype as its prototype, and hence inherits properties from Startup.prototype

{% highlight javascript %}
s1.config
  -> true
s1.motto("we change")
  -> "Our motto is we change"
{% endhighlight %}

Create another instance,

{% highlight javascript %}
s2 = new Startup(false)
{% endhighlight %}

s2 is an object with Startup.prototype as its prototype, and hence inherits properties from Startup.prototype

{% highlight javascript %}
s2.config
  -> false
s2.motto("we build")
  -> "Our motto is we build!"
{% endhighlight %}

Now, we did like a bit more functionality to be achieved by all objects instantiated from Startup. For that to happen, we need to add more properties to Startup.prototype

{% highlight javascript %}
Startup.prototype.space = function(people) { return(people/ 5) }
Startup.prototype.stream = function() {
   if(this.config == true)
       return("service") ;
   else
       return("business");
 }
{% endhighlight %}

Now, you could use these properties on the instances,

{% highlight javascript %}
s1.space(25)
   -> 5
s1.stream()
   -> "service"

s2.space(40)
   -> 8
s2.stream()
   -> "business"
{% endhighlight %}

[![Prototoype_5 - New Page (1)](http://sunilkumarn.files.wordpress.com/2013/06/prototoype_5-new-page-11.png?w=570)](http://sunilkumarn.files.wordpress.com/2013/06/prototoype_5-new-page-11.png)

As is clear from the image above, we have 4 objects here, the constructor object(Startup) with its property(prototype), the prototype object (Startup.prototype) with all its properties and the instances(s1 & s2).[
](http://sunilkumarn.files.wordpress.com/2013/06/prototoype_5-new-page.png)

Now suppose we alter the prototype property of Startup

{% highlight javascript %}
Startup.prototype = { stream : function() {return("No more a startup") }}
{% endhighlight %}

As is quite evident,

{% highlight javascript %}
s1.stream()
   -> "No more a startup!"
s2.stream()
   -> "No more a startup!"
{% endhighlight %}

On this account, it might be clear on how to add new methods to be used by arrays, dates and other such objects created via a Constructor Invocation. With _Array being the constructor function and Array.prototype being the prototype of all objects instantiated via the Array Constructor invocation_,

A clone on all arrays,

{% highlight javascript %}
Array.prototype.clone = function() {
   return this.concat()
   }

[1,2,3].clone()
   -> [1,2,3]
{% endhighlight %}

Clear off the array,

{% highlight javascript %}
Array.prototype.clear = function() {
    this.length = 0 ;
    return this ;
   }

[1,2,3].clear()
    -> []
{% endhighlight %}
