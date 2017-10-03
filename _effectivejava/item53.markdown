---
layout: post
title:  "Item 53: Prefer interfaces to Reflection
date:   2017-08-01 09:06:53 -0600
categories: Java
published: true

---
# Item 53:  Prefer interfaces to Reflection

* Reflection allows one class to use another, even if the latter class did
  not exist when the former was compiled. This power, however, comes at a price
  
    * You lose all the benefits of compile-time type checking, including exception
    checking. If a program attempts to invoke a nonexistent or inaccessible method
    reflectively, it will fail at runtime unless you’ve taken special precautions.
    * The code required to perform reflective access is clumsy and verbose. It is
    tedious to write and difficult to read.
    * Performance suffers. Reflective method invocation is much slower than
    normal method invocation
    
 * Reflection is used only at design time. As a rule, objects should not be accessed reflectively
   in normal applications at runtime.
   
 * You can obtain many of the benefits of reflection while incurring few of
   its costs by using it only in a very limited form. For many programs that must
   use a class that is unavailable at compile time, there exists at compile time an
   appropriate interface or superclass by which to refer to the class (Item 52). If this
   is the case, you can create instances reflectively and access them normally via
   their interface or superclass. If the appropriate constructor has no parameters,
   then you don’t even need to use java.lang.reflect; the Class.newInstance
   method provides the required functionality.
   
   
   {% highlight java %}
   public class MakeSet {
   	public static void main(String[] args) {
   		// Translate the class name into a Class object
   		Class<?> cl = null;
   		try {
   			cl = Class.forName("java.util.HashSet");
   		} catch (ClassNotFoundException e) {
   			System.err.println("Class not found.");
   			System.exit(1);
   		}
   
   		// Instantiate the class
   		Set<String> s = null;
   		try {
   			s = (Set<String>) cl.newInstance();
   		} catch (IllegalAccessException e) {
   			System.err.println("Class not accessible.");
   			System.exit(1);
   		} catch (InstantiationException e) {
   			System.err.println("Class not instantiable.");
   			System.exit(1);
   		}
   
   		// Exercise the set
   		s.addAll(Arrays.asList(args).subList(1, args.length));
   		System.out.println(s);
   	}
   }
    {% endhighlight %}
