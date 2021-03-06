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


{% highlight java %}
// Inappropriate use of string concatenation - Performs horribly!
public String statement() {
    String result = "";
    for (int i = 0; i < numItems(); i++)
        result += lineForItem(i); // String concatenation
    return result;
}
{% endhighlight %}


{% highlight java %}
public String statement() {
    StringBuilder b = new StringBuilder(numItems() * LINE_WIDTH);
    for (int i = 0; i < numItems(); i++)
        b.append(lineForItem(i));
    return b.toString();
}
{% endhighlight %}

* The difference in performance is dramatic. If numItems returns 100 and
lineForItem returns a constant 80-character string, the second method is eightyfive
times faster than the first on my machine. Because the first method is
quadratic in the number of items and the second is linear, the performance
difference is even more dramatic for larger numbers of items. Note that the second
method preallocates a StringBuilder large enough to hold the result. Even if it is
detuned to use a default-sized StringBuilder, it is still fifty times faster.