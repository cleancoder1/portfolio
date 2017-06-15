---
layout: post
title:  "Effective Java"
date:   2017-06-04 21:06:23 -0600
categories: Java
---

## Creating and Destroying Objects

# Item 1: Consider static factories instead of constructors

 * More meaningful name than constructor  BigInteger.probablePrime
 * Compact API like Collections , 32 classes
 * Unlike constructor can return a subType.
  EnumSet example mutliple implementations and client need not know which is returned.
 * The returned object might not even be there when writing code, such loose coupling is seen serve provider API's
 service provider api , the object returned may not be even there

 Service API ,Provider API  ,Provider registration api ,Service Access API
  Analogy to jdbc Connection => Service api         DriverManager.register  DriverManger.getConnection
{% highlight java %}
// Service provider framework sketch
// Service interface
public interface Service {
... // Service-specific methods go here
}
// Service provider interface
public interface Provider {
  Service newService();
}
// Noninstantiable class for service registration and access
public class Services {
  private Services() { } // Prevents instantiation (Item 4)
  // Maps service names to services
  private static final Map<String, Provider> providers =  new ConcurrentHashMap<String, Provider>();
  public static final String DEFAULT_PROVIDER_NAME = "<def>";

  // Provider registration API
  public static void registerDefaultProvider(Provider p) {
    registerProvider(DEFAULT_PROVIDER_NAME, p);
  }
  public static void registerProvider(String name, Provider p){
    providers.put(name, p);
  }
  // Service access API
  public static Service newInstance() {
    return newInstance(DEFAULT_PROVIDER_NAME);
  }
  public static Service newInstance(String name) {
    Provider p = providers.get(name);
    if (p == null)
    throw new IllegalArgumentException(
      "No provider registered with name: " + name);
      return p.newService();
    }
}

{% endhighlight %}

* An instance-controlled class is one that uses static factories to strictly control what instances exist at any time.

# Item 2: Consider a builder when faced with many constructor parameters

* Avoids telescopic conttructor pattern



# Item 3:	Enforce Singleton pattern with a private constructor or an enum type

*	private constructor and static factory method to create a Singleton
* Adding implements serializable to singleton is not enough, you must declare all fields transient and provide a readResolve method.
* A single-element enum type provides serialization for free


# Item 4: Enforce Noninstantiability with a private constructor

* No reason to have utility classes instantiable

{% highlight java %}
public class UtilityClass {
  // suppress default constructor for non instantiability
	private UtilityClass(){
      throw new AssertionError();  //prevents someone in class accidentally initializing the constructor
  }
}

{% endhighlight %}



# Item 5: Avoid creating unnecessary Objects

* you can often avoid creating unnecessary objects by using static factory methods in preference to constructor on immutable classes that provide both.
* "hello" is better than new String("hello")
* Boolean.valueOf("true") is better than new Boolean("true")
* Immutable objects could be reused for free
* Mutable objects could be reused if you really sure, they won’t be modified
* KeySet method of the Map interface returns a Set view of the Map Object, consisting of all the keys in the map. Every call to keyset on a given map returns same Set instance. Although retuned

* prefer primitivates to boxed primitives and watch out for unintenstional autoboxing.



# Item  6: Eliminate obsolete object references

* Nullify obsolete references
{% highlight java %}
public class Stack {
  private Object[] elements;
  private int size = 0;
  private static final int DEFAULT_INITIAL_CAPACITY = 16;
  public Stack() {
    elements = new Object[DEFAULT_INITIAL_CAPACITY];
  }
  public void push(Object e) {
    ensureCapacity();
    elements[size++] = e;
  }
  public Object pop() {
    if (size == 0)
    throw new EmptyStackException();
    return elements[--size];
  }
  // Ensure space for at least one more element, roughly doubling the capacity each time the array needs to grow
  private void ensureCapacity() {
    if (elements.length == size)
    elements = Arrays.copyOf(elements, 2 * size + 1);
  }
}
{% endhighlight %}

* If a stack grows and then shrinks, the objects that were popped off the stack will not be garbage collected, even if the program using the stack has no more references to them. This is because the stack maintains obsolete references to these objects.

* Whenever a class manages its own memory, the programmer should be alert for memory leaks.

* Deregister outdated listeners and callbacks



# Item 7  Avoid Finalizers

* Finalizers are unpredictable, often dangerous and generally unnecessary.
* It can take arbitararily long between the time than an object becomes unreachable and the time that its finalizer is executed.So never do time-criticial things
  ex :- depend on finalizer to close file.
* Use terminate methods like close on streams instead of finalizers
* Might work on your cpu java implementations but can behave differently in different jdk or OS
* chaining with super class is not default behavior ,you need to call super class finalizer method explicitly if a sub class overrides the class.
* Better to write an anonoymous inner class with finalizer  Finalizer guard class
{% highlight java %}
// Finalizer Guardian idiom
public class Foo {
  // Sole purpose of this object is to finalize outer Foo object
  private final Object finalizerGuardian = new Object() {
    @Override protected void finalize() throws Throwable {
      // Finalize outer Foo object
    }
  };
 // Remainder omitted
}
{% endhighlight %}

* Finalizers act as safety net in cas the owner of an object forgets to call its explicit termination method.
* A second legitimate use of finalizers concerns objects with native peers. A native peer is a native object to which a normal object delegates via native methods.
Because a native peer is not a normal object, the garbage collector doesn’t know about it and can’t reclaim it when its Java peer is reclaimed.

# Methods Common to All objects

# Item 8  
