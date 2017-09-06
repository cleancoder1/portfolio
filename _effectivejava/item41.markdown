---
layout: post
title:  "Item 41: Use Overloading judiciously"
date:   2017-08-01 09:06:41 -0600
categories: Java
published: true

---

* The choice of which overloaded method to invoke is made at compile time
* Selection among overloaded methods is static, while selection among overridden methods is dynamic
* A safe, conservative policy is never to export two overloadings with the same number of parameters
* The rules that determine which overloading is selected are extremely complex. 
They take up thirty-three pages in the language specification [JLS, 15.12.1-3]