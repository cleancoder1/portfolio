---
layout: post
title:  "Item 44: Write doc comments for all exposed API elements"
date:   2017-08-01 09:06:43 -0600
categories: Java
published: true

---
# Item 44:  Write doc comments for all exposed API elements

* To document your API properly, you must precede every exported class,interface, constructor, method, and field declaration with a doc comment.
* If a class is serializable, you should also document its serialized form


* The doc comment for a method should describe succinctly the contract between the method and its client.the contract should say what the method does
rather than how it does its job. The doc comment should enumerate all of the method’s preconditions, which are the things that have to be true in order for a client
to invoke it, and its postconditions. Finally, documentation comments should describe the thread safety of a class or method.

* Two aspects of a class’s exported API that are often neglected are threadsafety and serializability. Whether or not a class is thread-safe, you should document
its thread-safety level. If a class is serializable, you should document its serialized form

{% highlight java %}
/**
* Returns the element at the specified position in this list.
*
* <p>This method is <i>not</i> guaranteed to run in constant
* time. In some implementations it may run in time proportional
* to the element position.
*
* @param index index of element to return; must be
* non-negative and less than the size of this list
* @return the element at the specified position in this list
* @throws IndexOutOfBoundsException if the index is out of range
* ({@code index < 0 || index >= this.size()})
*/
E get(int index);

{% endhighlight %}
