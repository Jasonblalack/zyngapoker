Build System
============

Tooling for JavaScript is not yet captured by a good set of tools yet. Jasy is one approach to fix this situation. Without a good and generic build system it's pretty hard to deal with even semi-complex project structures. In basically every other programming environment (.NET, Java, Objective-C or even Flash) it's typically to use a build system. Still it seems that in JavaSCript people think to be able to life without such a system.

Tooling systems which are based on a [DSL](http://en.wikipedia.org/wiki/Domain-specific_language) are often quite limited regarding flexibility. Jasy should be the one and only tooling solution for your JavaScript you need. There should not be anything you can't implement in there. This is one of the reasonings why the build system is not configured using a DSL. Instead Jasy is used through a Python3-API. The project author writes a Python3-script using the Jasy-API but is also able to use all standard Python modules to fulfill the tasks. 

Jasy is to some point comparable to the ideas also implemented in build systems like [SCons](http://www.scons.org/).

FAQ
---

### Do I need to know how to program in Python to use SCons?

No, you can use Jasy very successfully even if you don't know how to program in Python.

With Jasy, you use Python functions to tell a engine about your projects and output wishes. You can look at these simply as different commands that you use to specify what software you want built. Jasy takes care of the rest, including figuring out dependencies etc.

Of course, if you do know Python, you can use its scripting capabilities to do more sophisticated things in your build: construct lists of files, manipulate file names dynamically, handle flow control (loops and conditionals) in your build process, etc.

