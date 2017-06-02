---
layout: post
title:  "Effective Java!"
date:   2017-02-17 21:06:23 -0600
categories: Java
---

# Item 1: Consider static factories instead of constructors

 * More meaningful name than constructor  BigInteger.probablePrime
 * Compact APi like Collections , 32 classes
 * EnumSet example mutliple implementations and client need not know which is returned.
 * The returned object might not even be there when writing code, such loose coupling is seen serve provider api's
 service provider api , the object returned may not be even there

 Service Api ,Provider APi  ,provider registration api ,service access api
  Analogy to jdbc Connection => Service api         DriverManager.register  DriverManger.getConnection



# Item 2 Consider a builder when faced with many constructor parameters

* Avoids telescopic conttructor pattern



# Item 3 	Enforce Signleton pattern with a private constructor or an enum type

*	private constructor and static factory method to create a Singelton
* A single-element enum type provides serialization for free
* Adding implements serializable to singleton is not enough, you must declare all fields transient and provide a readResolve method.



# Item 33
EnumMap


{% highlight java %}
public class Provider {
	public static void main (String[] args){
	}
}

{% endhighlight %}
