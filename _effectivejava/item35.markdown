---
layout: post
title:  "Item 35: Prefer annotations to naming patterns"
date:   2017-08-01 09:06:23 -0600
categories: Java
published: true

---

# Item 35  Prefer annotations to naming patterns

* Prior to release 1.5, it was common to use naming patterns to indicate that some program elements demanded special treatment by a tool or framework.
For example,the JUnit testing framework originally required its users to designate test methods by beginning their names with the characters test

 * First, typographical errors may result in silent failures  Ex tsetSafetyOverride
 * A second disadvantage of naming patterns is that there is no way to ensure that they are used only on appropriate program elements Ex call a class testSafetyMechanisms and hoping all methods will be tests
 * A third disadvantage of naming patterns is that they provide no good way to associate parameter values with program elements
  Ex You want to support a test that succeeds only if it throws a particular exception.
