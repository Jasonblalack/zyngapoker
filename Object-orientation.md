Object Orientation
==================

The core JavaScript library of Jasy supports the following object orientation patterns:

* Modules
* Classes
* Interfaces

Jasy will support generating API documentation in the future. These concepts will then be used for documentation proposes, too.

Modules
-------

A Module is basically a static container for methods and data. A Module is defined but never initiated. In languages like Java something like this would be called static class (a class which only contains static methods). Modules are perfectly usable for storing generic utility methods which are not related to a specific object instance. Modules should not store any runtime data (caching might be an exception though).

Modules are no replacement for singletons. They must not store any state. [Singletons are not a good match when you need modularity and testability](http://blogs.msdn.com/b/scottdensmore/archive/2004/05/25/140827.aspx).


Classes
-------

Classes are a central concept in most object-oriented languages, and as a programmer you are certainly familiar with it. Jasy supports a "closed form" of class declaration, i.e. the entire declaration is provided within a single statement. A class consists of a constructor and member methods/fields. Jasy also includes support for advanced topics like properties (automatic setter/getter) and events (just for verification proposes).

Classes in Jasy do not support single inheritance. We believe in new ideas like compositing functionality. Using interfaces instead of deep inheritance is the better way to deal with complex requirements. Even the old Java guys would [prefer a Java without a "extend" keyword nowadays](http://www.javaworld.com/cgi-bin/mailto/x_java.cgi?pagetosend=/export/home/httpd/javaworld/javaworld/jw-08-2003/jw-0801-toolbox.html&pagename=/javaworld/jw-08-2003/jw-0801-toolbox.html&pageurl=http://www.javaworld.com/javaworld/jw-08-2003/jw-0801-toolbox.html&site=jw_core). 



Interfaces
----------

