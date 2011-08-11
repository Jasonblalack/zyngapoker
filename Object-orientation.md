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

Modules are no replacement for singletons. They must not store any state. Read more here on why singletons are bad: http://blogs.msdn.com/b/scottdensmore/archive/2004/05/25/140827.aspx


Classes
-------

Classes are a central concept in most object-oriented languages, and as a programmer you are certainly familiar with it. Jasy supports a "closed form" of class declaration, i.e. the entire declaration is provided within a single statement.

Classes in Jasy do not support single inheritance. We believe in new ideas like mashing functionality together and think that the concept of interfaces and mixins is more important than simple inheritance.



Interfaces
----------

