---
layout: post
title:  "Item 38: Check parameters for validity"
date:   2017-08-01 09:06:38 -0600
categories: Java
published: true

---

* Detect errors and wrong values as soon as possible
* For public methods, use the Javadoc @throws tag to document the exception that will be thrown if a restriction on parameter values is violated
* nonpublic methods should generally check their parameters using assertions
* Unlike normal validity checks, they have no effect and essentially no cost unless you enable them, which you do by passing the -ea (or -enableassertions) flag to the java interpreter
* Do not use validity check if it is impractical or performed implicitly in the process of doing the computation