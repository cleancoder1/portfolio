---
layout: post
title:  "Item 39: Make defensive copies when needed
date:   2017-08-01 09:06:39 -0600
categories: Java
published: true

---

* You must program defensively, with the assumption that clients of your class will do their best to destroy its invariants
* If you return mutable reference from class, then class is also mutable
* To make immutable class, make a defensive copy of each mutable parameter
* Defensive copies are made before checking the validity of the parameters and the validity check is performed on the copies rather than on the originals (protect against changes from another thread, TOCTOU time-of-check/time-of-use attack)
* Do not use the clone method to make a defensive copy of a parameter whose type is subclassable by untrusted parties
* Defensive copying of parameters is not just for immutable classes, think twice before returning a reference
* Arrays are always mutable
* Defensive copying can have a performance penalty associated with it and isn’t always justified
* If the cost of the defensive copy would be prohibitive and the class trusts its clients not to modify the components inappropriately, then the defensive copy may be replaced by documentation outlining the client’s responsibility not to modify the affected components