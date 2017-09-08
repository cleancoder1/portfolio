---
layout: post
title:  "Item 43: Use varargs judiciously"
date:   2017-08-01 09:06:43 -0600
categories: Java
published: true

---

* Do not return nulls!
* Return empty collection (Collections.emptyList()), or zero-length array (new int[0]) instead of nulls
* Zero-length arrays and empty collections are not performance problems, because they are immutable and only one instance could be used