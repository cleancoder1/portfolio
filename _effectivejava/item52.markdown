---
layout: post
title:  "Item 51: Refer to objects by their interfaces"
date:   2017-08-01 09:06:52 -0600
categories: Java
published: true

---
# Item 52:  Refer to objects by their interfaces

* If appropriate interface types exist, then parameters, return values, variables, and fields should all be declared using interface types
* If you depend on any special properties of an implementation, document these requirements where you declare the variable
* It is entirely appropriate to refer to an object by a class rather than an
  interface if no appropriate interface exists
  
* A second case in which there is no appropriate interface type is that of objects
  belonging to a framework whose fundamental types are classes rather than
  interfaces. If an object belongs to such a class-based framework, it is preferable to
  refer to it by the relevant base class, which is typically abstract, rather than by its
  implementation class. The java.util.TimerTask class falls into this category 