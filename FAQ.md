## Why is Jasy based on Python3?

Python is great for tools. It's used at Google for a lot of administration tasks. It has tons of built-in modules and feature. Python offers nice OO capabilities as well as functional programming paradigms. Python 3 is basically a cleanup version of the previous Python 2.x iterations. While not being used that widely yet, more and more modules are able being used in Python 3. Python 3 has got a lot of cleanups and was streamlined a lot regarding dealing with strings, unicode and binary data and makes it far easier to use for string heavy tooling aspects. Python 3 is a great choice for tooling and scripting. It's easy to learn, easy to extend and is able to run cross platform easily.

## Why not use Make?

[Make](http://www.gnu.org/software/make/) requires the use of its own [domain specific language](http://en.wikipedia.org/wiki/Domain-specific_language) — this is, in general, not a good idea. Makefile are so hard to write and extend that several popular build systems today are essentially Makefile generators e.g. [CMake](http://www.cmake.org/). Also make does not contain any specific tools for dealing with web related files like JavaScript files.

## Why not use a DSL?

Tooling systems which are based on a [DSL](http://en.wikipedia.org/wiki/Domain-specific_language) are often quite limited regarding flexibility. Jasy should be the one and only tooling solution for your JavaScript you need. There should not be anything you can't implement in there. This is one of the reasonings why the build system is not configured using a DSL. Instead Jasy is used through a Python3-API. The project author writes a Python3 script using the Jasy API but is also able to use all standard Python modules to fulfill the tasks. Jasy borrows this idea of using a full programming language instead of pure configuration files from build systems like [SCons](http://www.scons.org/) or [Waf](http://code.google.com/p/waf/).

## What makes Jasy better?

- Jasy was designed from ground up for the requirements of medium to complex web projects. Not these simple jQuery projects but real projects with 50+ classes and hundreds of assets.
- Configuration files are Python scripts -- use the power of a real programming language to solve build problems. Developing for the web is complex and your build tool must offer support for dealing with that.
- Supports complex project structures with dependencies to other libraries and frameworks. No more copying single files from other projects into your projects structure.
- Integrated [GIT](http://git-scm.com/) support for auto-cloning and managing of used libraries and frameworks. Fully supports the model of branches and tags. No more GIT sub repositories to manage by hand.
- Two different project build variants (source and build) offer the right features for developing and deployment phase of an application. The so-named "source" version loads the original class files which is useful during the development phase of an application. The build version puts all compiled into one (or multiple) optimized code bundles to minimize transfer size and runtime performance.
- Highly effective single file binary cache (with integrated memory cache). This solution is very fast on all operating systems (much faster than single file lookups with legacy build tools.
- Integrated permutation supports allow for complex output scenarios (debug=on/off, device=tablet/desktop/mobile, locale=en/fr/de, ...) and access the selected variant using a simple JavaScript API.



