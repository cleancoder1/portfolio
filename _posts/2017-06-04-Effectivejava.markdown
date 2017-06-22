---
layout: post
title:  "Effective Java"
date:   2017-06-04 21:06:23 -0600
categories: Java
---


Effective java is a great book to hone your technical skills and a best investment you can make if you are a Java developer.
  {% raw %}
  <iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="//ws-na.amazon-adsystem.com/widgets/q?ServiceVersion=20070822&OneJS=1&Operation=GetAdHtml&MarketPlace=US&source=ac&ref=qf_sp_asin_til&ad_type=product_link&tracking_id=awesomeyogi-20&marketplace=amazon&region=US&placement=0321356683&asins=0321356683&linkId=4bdd1e1a7678cada93ef0f5b806ad320&show_border=false&link_opens_in_new_window=false&price_color=333333&title_color=0066C0&bg_color=FFFFFF">
    </iframe>
  {% endraw %}  
This is just my notes , will be adding more notes as i go through the book.
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

# Item 8  Obey the general contract when overriding equals


* No need to override the equals method
  * Each instance of the class is inherently unique
  * You don't care whether the class provides a logical equality test ..Example Random class
  * A super class has already overriden the equals method and the super class behavior is appropriate.
  * The class is private or package private and you are certain that its equal method will never be invoked

* The equals method implements an equivalence relation
  * Reflexive
  * Symmetric
  * Transitive
  * Conistent
  * For any non-null reference  x.equals(null)  must return false

  {% highlight java %}
  // Broken - violates symmetry!
  public final class CaseInsensitiveString {
    private final String s;
    public CaseInsensitiveString(String s) {
      if (s == null)
      throw new NullPointerException();
      this.s = s;
    }
  // Broken - violates symmetry!
    @Override public boolean equals(Object o) {
      if (o instanceof CaseInsensitiveString)
      return s.equalsIgnoreCase(
        ((CaseInsensitiveString) o).s);
        if (o instanceof String) // One-way interoperability!
        return s.equalsIgnoreCase((String) o);
        return false;
      }
  }
  {% endhighlight %}

* Once you’ve violated the equals contract, you simply don’t know how other objects will behave when confronted with your object.


# Item 9 Always override hashCode when you override equals

* equal objects needs to have equal hashcode
{% highlight java %}
public final class PhoneNumber {
  private final short areaCode;
  private final short prefix;
  private final short lineNumber;
  public PhoneNumber(int areaCode, int prefix,int lineNumber) {
    this.areaCode = (short) areaCode;
    this.prefix = (short) prefix;
    this.lineNumber = (short) lineNumber;
  }
  @Override public boolean equals(Object o) {
    if (o == this)
      return true;
    if (!(o instanceof PhoneNumber))
      return false;
    PhoneNumber pn = (PhoneNumber)o;
    return pn.lineNumber == lineNumber    && pn.prefix == prefix    && pn.areaCode == areaCode;
  }
  //Broken no hashcode method
}
{% endhighlight %}

{% highlight java %}

Map<PhoneNumber, String> m = new HashMap<PhoneNumber, String>();
m.put(new PhoneNumber(707, 867, 5309), "Jenny");
m.get(new PhoneNumber(707, 867, 5309))  //you expect Jenney but you get null
{% endhighlight %}

 * equal objects should end up in same hash bucket. even if they endup in same hashbucket ,hasmaps are
* If a class is immutable and the cost of computing the hash code is significant,you might consider caching the hash code in the object rather than recalculating it
each time it is requested


# Item 10 Always override toString

* When practical, the toString method should return all of the interesting information contained in the object
* The toString method is automatically invoked when an object is passed to println, printf, string concationation or assert or printed by debugger.
  Programmers will generate diagnostic messages
* Provide programmatic access to all of the information contained in the value returned by toString.
  By failing to provide accessors, you turn the string format into a de facto API, even if you’ve specified that it’s subject to change.

# Item 11 Clone
* Mixin interface
* Objects clone method is protected
* deepCopy


# Item 12 Consider Implementing Comparable

* Just like equals contract equivalence relationship
* if compareTo returns zero ,better be the objects equals if not specify with a comment
* new BigDecimal("0.0") ,new BigDecimal("0.00") added to HashSet will maintain two objets as it invokes equals contract vs treeSet which maintains one as it invokes compareTo
* Doesn't tell magnitude -1 or -2 its upto programmer. The contract tells only negative and postive or zero


## Classes and interfaces

# Item 13 Minimize the accessibility of classes and members

* A well desinged module hides all of its implentation details, cleanly seperating its API from its implementation.
* Rule of thumb is simple : make each class or member as inaccessible as possible.
* For top-level (non-nested) classes and interfaces there are only two possible access levels : package-private and public
  * If a package-private top-level class is used by only one class,consider making the top-level class a private nested class of the sole class that uses it.
* For memebers (fields,methods,nested classes and nested interfaces) there are four possible assess levels private, package-private, protected( subclass + package)    ,public

* private and package-private members are part of class's implementation and do not notmally impact in exported API. These fields however can leak into the exported API if the class implements serializable
* Instance fields should never be public, if an instance field is non final or final reference to a mutable object, then by making the filed public, you give up the ability to limit the values that can be stored in the field. This means you give up the ability to enforce invariants involving this field.ALso you give up the ability to take action when the filed is modified, so classes with public mutable fields are non thread safe.
* A final field containing a reference to a mutable object has all the disadvantages of a nonfinal field. While the reference cannot be
modified, the referenced object can be modified—with disastrous results.
* Note that a nonzero-length array is always mutable,so it is wrong for a class to have a public static final array field, or an accessor that returns such a
field
{% highlight java %}
// Potential security hole!
public static final Thing[] VALUES = { ... };
{% endhighlight  %}

Two ways to fix
{% highlight java %}
private static final Thing[] PRIVATE_VALUES = { ... };
public static final List<Thing> VALUES = Collections.unmodifiableList(Arrays.asList(PRIVATE_VALUES));
{% endhighlight %}

Alternative
{% highlight java %}
private static final Thing[] PRIVATE_VALUES = { ... };
public static final Thing[] values() {
  return PRIVATE_VALUES.clone();
}
{% endhighlight %}

# Item 14 In public classes , use accessor methods,not public fields

* If a class is package-private or is a private nested class, there is nothing inherently wrong with exposing its data fields.
* Point and Dimension of java api classes are exceptions where there expose public fields and it was a bad idea.
* While its never a good idea for a public class to expose field directly, it is less harmful if the fields are immutable. You can't change the repersentation of such
  class without changing its API , and you can't take auxilary actions when a field is read , but you can enforce invariants.
  {% highlight java %}

  // Public class with exposed immutable fields - questionable
public final class Time {
  private static final int HOURS_PER_DAY = 24;
  private static final int MINUTES_PER_HOUR = 60;
  public final int hour;
  public final int minute;
  public Time(int hour, int minute) {
    if (hour < 0 || hour >= HOURS_PER_DAY)
      throw new IllegalArgumentException("Hour: " + hour);
    if (minute < 0 || minute >= MINUTES_PER_HOUR)
      throw new IllegalArgumentException("Min: " + minute);
    this.hour = hour;
    this.minute = minute;
  }
}
{% endhighlight %}


# Item 15 Minimize immutability
* An immputable class is simply a class whose instance cannot be modified. All of the information contained in each instance is provided when it is created an is fixed
for the lifetime of the object. 
* Recipe for immutability
  * Don't provide any mehtods that modify the Object's state ( known as mutators)
  * Ensure that class can't be extended  : Subclasses can compromise the immutable behavior of the class by behaving as if the objects state has changed. //TODO need to research more on how
  * Make all fields final
  * Make all fields private, technically possible to have public final fields containing primitive value or immutable classes but a bad idea
  * Ensure exclusive access to any mutable components , if your class has any fields that refer to mutable objects ensure that clients of the class cannot obtain references to these objects.  Never initialize such a field to a client-provided object reference or return the object refrenece from the accessors. Make defensive
  copies in constructors, accessors and readObject methods.
