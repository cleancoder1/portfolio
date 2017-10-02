---
layout: post
title:  "Item 51: Beware the performance of string concatenation"
date:   2017-08-01 09:06:51 -0600
categories: Java
published: true

---
# Item 51:  Beware the performance of string concatenation

* Using String concatenation operator repeatedly to concatenate n strings requires time 
quadratic in n. As Strings are immutable, when two strings are concatenated, the contents of both are copied.



* The doc comment for a method should describe succinctly the contract between the method and its client.the contract should say what the method does
rather than how it does its job. The doc comment should enumerate all of the method’s preconditions, which are the things that have to be true in order for a client
to invoke it, and its postconditions. Finally, documentation comments should describe the thread safety of a class or method.

* Two aspects of a class’s exported API that are often neglected are threadsafety and serializability. Whether or not a class is thread-safe, you should document
its thread-safety level. If a class is serializable, you should document its serialized form

{% highlight java %}
// Inappropriate use of string concatenation - Performs horribly!
public String statement() {
    String result = "";
    for (int i = 0; i < numItems(); i++)
        result += lineForItem(i); // String concatenation
    return result;
}
{% endhighlight %}
