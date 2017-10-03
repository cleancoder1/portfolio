---
layout: post
title:  "Item 54: Use native methods judiciously"
date:   2017-08-01 09:06:54 -0600
categories: Java
published: true

---
# Item 54:  Use native methods judiciously

*The Java Native Interface (JNI) allows Java applications to call native methods, which are special methods written in native programming languages such as C or C++
JNI has three main uses
    * Access to platform-specific facilities such as registries and file locks
    * Access to libraries of legacy code, which could in turn provide access to legacy data
    * Used to write performance-critical parts of applications
* Do not use native methods for improved performance
* Applications using native methods have disadvantages
     *   programs are not immune to memory corruption errors
     * less portable
     * difficult to debug