Build System
============

Tooling for JavaScript is not yet captured by a good set of tools yet. Jasy is one approach to fix this situation. Without a good and generic build system it's pretty hard to deal with even semi-complex project structures. In basically every other programming environment (.NET, Java, Objective-C or even Flash) it's typically to use a build system. Still it seems that in JavaSCript people think to be able to life without such a system.

Tooling systems which are based on a [DSL](http://en.wikipedia.org/wiki/Domain-specific_language) are often quite limited regarding flexibility. Jasy should be the one and only tooling solution for your JavaScript you need. There should not be anything you can't implement in there. This is one of the reasonings why the build system is not configured using a DSL. Instead Jasy is used through a Python3-API. The project author writes a Python3-script using the Jasy-API but is also able to use all standard Python modules to fulfill the tasks. 

Jasy is to some point comparable to the ideas also implemented in build systems like [SCons](http://www.scons.org/).

Concepts
--------

* Session: Every time you are dealing with processing code from projects you create a session instance.
* Projects: A project is any folder with JavaScript files, assets and/or translations. The main project is the one which contains the currently executed build script. Each project needs to have a `manifest.json` file to define its public name and configuration.
* Fields: A build-time configurable setting which is used by the code of a project which defines it. The build script of the main project is able to enable permutations for all known fields and can override any defaults from the defining project.
* Permutations: A permutation is a combination of values for different fields e.g. debug=off and engine=webkit is one possible permutation. Every new field which is enabled as a permutation multiplies the number of created permutations.
* Locales: Jasy includes full-blown support for locales as defined by unicode.org and the CLDR archive. It contains informations about number and date formats, country names, etc.
* Translations: This is based on gettext-files. The main differentiator from a lot of other implementations is that Jasy can inline-replace translations requests during building a specific permutation.


