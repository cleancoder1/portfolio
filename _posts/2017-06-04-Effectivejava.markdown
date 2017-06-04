---
layout: post
title:  "Effective Java"
date:   2017-06-04 21:06:23 -0600
categories: Java
---

# Item 1: Consider static factories instead of constructors

 * More meaningful name than constructor  BigInteger.probablePrime
 * Compact APi like Collections , 32 classes
 * Unlike constructor can return a subType.
  EnumSet example mutliple implementations and client need not know which is returned.
 * The returned object might not even be there when writing code, such loose coupling is seen serve provider api's
 service provider api , the object returned may not be even there

 Service Api ,Provider APi  ,provider registration api ,service access api
  Analogy to jdbc Connection => Service api         DriverManager.register  DriverManger.getConnection



# Item 2 Consider a builder when faced with many constructor parameters

* Avoids telescopic conttructor pattern



# Item 3 	Enforce Singleton pattern with a private constructor or an enum type

*	private constructor and static factory method to create a Singleton
* A single-element enum type provides serialization for free
* Adding implements serializable to singleton is not enough, you must declare all fields transient and provide a readResolve method.


# Item 4 Enforce Noninstantiability with a private constructor

* No reason to have utility classes instantiable

{% highlight java %}
public class UtilityClass {
  // suppress default constructor for non instantiability
	private UtilityClass(){
      throw new AssertionError();  //prevents someone in class accidentally initializing the constructor

  }

}

{% endhighlight %}



# Item 5 Avoid creating unnecessary Objects

* you can often avoid creating unnecessary objects by using static factory methods in preference to constructor on immutable classes that provide both.

* KeySet method of the Map interface returns a Set view of the Map Object, consisting of all the keys in the map. Every call to keyset on a given map returns same Set instance. Although retuned

* prefer primitivates to boxed primitives and watch out for unintenstional autoboxing.


# Item 33
EnumMap


{% highlight java %}
public class Provider {
	public static void main (String[] args){
	}
}

{% endhighlight %}
