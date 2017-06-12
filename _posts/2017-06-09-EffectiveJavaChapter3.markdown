---
layout: post
title:  "Effective Java Chapter 3"
date:   2017-06-04 21:06:23 -0600
categories: Java
---

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
