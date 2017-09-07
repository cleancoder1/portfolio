---
layout: post
title:  "Item 40: Design method signatures carefully"
date:   2017-08-01 09:06:40 -0600
categories: Java
published: true

---

* Choose method names carefully
* Follow the code conventions
* Too many methods make a class difficult to learn, use, document, test, and maintain
* Avoid long parameter lists (four parameters or fewer)
    * Use builder pattern, helper classes or helper methods to avoid long parameter lists
    * A technique for shortering long parameter lists is to create helper classes to hold groups of parameters
    For example, suppose you are writing a class representing a card game, and you find yourself
    constantly passing a sequence of two parameters representing a card’s rank and its
    suit. Your API, as well as the internals of your class, would probably benefit if you
    added a helper class to represent a card and replaced every occurrence of the
    parameter sequence with a single parameter of the helper class
* For parameter types, favor interfaces over classes
* Prefer two-element enum types to boolean parameters