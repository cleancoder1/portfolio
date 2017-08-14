---
layout: post
title:  "Item 42: Use varargs judiciously"
date:   2017-08-01 09:06:23 -0600
categories: Java
published: true

---

* Use call(int...) when you need zero or more arguments
* Use call(int, int...) when you need one or more arguments
    
* Don’t use varargs for every method that has a final array parameter; use varargs only when a call really operates on a variable-length sequence of values
    Arrays.asList(""one","two","three")
    * Prior to java 1.5 this method takes an final array,now it is retrofit to take varargs. 
* Every invocation of a varargs method causes an array allocation and initialization
* Use overloaded methods with 2, 3, 4 params to cover most use-cases, otherwise use varargs
    * EnumSet uses such technique