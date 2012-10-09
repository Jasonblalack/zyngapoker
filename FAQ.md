## General

### Why is Jasy based on Python3?

Python is great for tools. It's used at Google for a lot of administration tasks. It has tons of built-in modules and feature. Python offers nice OO capabilities as well as functional programming paradigms. Python 3 is basically a cleanup version of the previous Python 2.x iterations. While not being used that widely yet, more and more modules are able being used in Python 3. Python 3 has got a lot of cleanups and was streamlined a lot regarding dealing with strings, unicode and binary data and makes it far easier to use for string heavy tooling aspects. Python 3 is a great choice for tooling and scripting. It's easy to learn, easy to extend and is able to run cross platform easily.

### Why not use Make?

[Make](http://www.gnu.org/software/make/) requires the use of its own [domain specific language](http://en.wikipedia.org/wiki/Domain-specific_language) — this is, in general, not a good idea. Makefile are so hard to write and extend that several popular build systems today are essentially Makefile generators e.g. [CMake](http://www.cmake.org/). Also make does not contain any specific tools for dealing with web related files like JavaScript files.


### What makes Jasy better?

- Jasy was designed from ground up for the requirements of medium to complex web projects. Not these simple jQuery projects but real projects with 50+ classes and hundreds of assets.
- Configuration files are Python scripts -- use the power of a real programming language to solve build problems. Developing for the web is complex and your build tool must offer support for dealing with that.
- Supports complex project structures with dependencies to other libraries and frameworks. No more copying single files from other projects into your projects structure.
- Integrated [GIT](http://git-scm.com/) support for auto-cloning and managing of used libraries and frameworks. Fully supports the model of branches and tags. No more GIT sub repositories to manage by hand.
- Two different project build variants (source and build) offer the right features for developing and deployment phase of an application. The so-named "source" version loads the original class files which is useful during the development phase of an application. The build version puts all compiled into one (or multiple) optimized code bundles to minimize transfer size and runtime performance.
- Highly effective single file binary cache (with integrated memory cache). This solution is very fast on all operating systems (much faster than single file lookups with legacy build tools.
- Integrated permutation supports allow for complex output scenarios (debug=on/off, device=tablet/desktop/mobile, locale=en/fr/de, ...) and access the selected variant using a simple JavaScript API.

## Start development

### Do i have to install Jasy?

If you don't want to install Jasy like described in the [wiki](https://github.com/zynga/jasy/wiki), simply clone the repository and call Jasy via `$ bin/jasy` from the directories root.  

### Is there a smart sample project to start coding from?

Of course! Check out our [Jasy-Boilerplate project](https://github.com/zynga/jasy-html5-boilerplate) and its [QuickStart guide](https://github.com/zynga/jasy-html5-boilerplate/wiki/QuickStart) for beginners.

### Are there any useful commands for Jasy beginners?

Yes. Use `$ jasy doctor` to check if all Jasy requirements are installed and up to date and, if not, to get install instructions.

Look up the scripting functionality of Jasy with `$ jasy showapi`.


