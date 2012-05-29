Jasy is a construction tool for web projects. Think of Jasy as a replacement for the classic [make](http://www.gnu.org/software/make/) utility heavily inspired by modern tools like [SCons](http://www.scons.org/) (Used by V8, MongoDB, ...), [Waf](http://code.google.com/p/waf/) (Used by Samba, NodeJS, ...) and [Maven](http://maven.apache.org/). Jasy is based on the learnings through creating the "generator" for the [qooxdoo](http://qooxdoo.org) JavaScript framework.


# Why not use Make?

[Make](http://www.gnu.org/software/make/) requires the use of its own [domain specific language](http://en.wikipedia.org/wiki/Domain-specific_language) — this is, in general, not a good idea. Makefile are so hard to write and extend that several popular build systems today are essentially Makefile generators e.g. [CMake](http://www.cmake.org/). Also make does not contain any specific tools for dealing with web related files like JavaScript files.


## Why not use a DSL?

Tooling systems which are based on a [DSL](http://en.wikipedia.org/wiki/Domain-specific_language) are often quite limited regarding flexibility. Jasy should be the one and only tooling solution for your JavaScript you need. There should not be anything you can't implement in there. This is one of the reasonings why the build system is not configured using a DSL. Instead Jasy is used through a Python3-API. The project author writes a Python3 script using the Jasy API but is also able to use all standard Python modules to fulfill the tasks. Jasy borrows this idea of using a full programming language instead of pure configuration files from build systems like [SCons](http://www.scons.org/) or [Waf](http://code.google.com/p/waf/).


# What makes Jasy better?

- Jasy was designed from ground up for the requirements of medium to complex web projects. Not these simple jQuery projects but real projects with 50+ classes and hundreds of assets.
- Configuration files are Python scripts -- use the power of a real programming language to solve build problems. Developing for the web is complex and your build tool must offer support for dealing with that.
- Supports complex project structures with dependencies to other libraries and frameworks. No more copying single files from other projects into your projects structure.
- Integrated [GIT](http://git-scm.com/) support for auto-cloning and managing of used libraries and frameworks. Fully supports the model of branches and tags. No more GIT sub repositories to manage by hand.
- Two different project build variants (source and build) offer the right features for developing and deployment phase of an application. The so-named "source" version loads the original class files which is useful during the development phase of an application. The build version puts all compiled into one (or multiple) optimized code bundles to minimize transfer size and runtime performance.
- Highly effective single file binary cache (with integrated memory cache). This solution is very fast on all operating systems (much faster than single file lookups with legacy build tools.
- Integrated permutation supports allow for complex output scenarios (debug=on/off, device=tablet/desktop/mobile, locale=en/fr/de, ...) and access the selected variant using a simple JavaScript API.


# Features

## Language Support

- Deep JavaScript language support through being based on a full-blown parser which itself is based on the mature Spidermonkey parser from Mozilla. Jasy even fully supports upcoming features like generators, array comprehension, etc. of JavaScript 1.8.
- Generates a easy to process [AST](http://en.wikipedia.org/wiki/Abstract_syntax_tree). This `AST` can be used to transform the original code for e.g. API documentation, code formatting, code linting, optimization, etc.
- Handles comments during parse process and attaches the comment informations to the generated `AST` nodes.
- Reliable, automatic dependency analysis for JavaScript is built in. Automatically sorts files by their requirements. Dependency analysis happens with permutations applied (e.g. debug builds might contain classes deployment builds might not contain).


## Build Projects

- Supports permutations to fully remove code blocks, classes, assets, translations which are not required for the current build.
- Removes white space, comments and any formatting from original code.
- Inlines translation into the permutated application code. No more lookup for translation texts once compiled.
- Copies all assets to bundle them in a self contained folder. This simplifies deployment by a major extend.


## Code Optimizers

- Optimizations happens in the same league of [Google Closure Compiler](https://developers.google.com/closure/compiler/) and [Uglify](https://github.com/mishoo/UglifyJS).
- Dead code elimination removes unreachable code in `if`/`else` blocks, conditional statements `?:`, `switch`/`case` and `AND`/`OR` operators.
- Supports comparison of hard-coded or permutation added values for `boolean`, `number` and `string` types.
- Groups single `var` statements into blocks.
- Renames file private variables (starting with double underscore by convention)
- Removes needless blocks (with just one statement)
- Automatically combines strings and numbers (e.g. "Version " + 1.3 => "Version 1.3")
- Optimizes if(-else) statements with expressions as content
  - Translates if-statements without else using `&&` or `||` operators
  - Translates if-statements with else using conditional operator `? :` (especially worth with returns/assignments)
- Removes needless else (if previous if-block ends with a return/throw statement)
- Removes needless parens based on priority analysis on the AST


## Asset Managment

- Integrated path abstraction support for assets (images, CSS files, ...) using project IDs instead of filesystem paths on the client side.
  - Builds a client accessible database of available assets and their meta data. Offers easy to use APIs to access that data.
  - Support for enterprise grade CDN hosting and checksum based URLs to load assets.
- Generating and using image sprites (with easy to use client side API to transparently deal with the original file names).
- Defining and using sprite animations via simple JSON files (for graphically rich animations).


## Localization

- Translate applications with widely support gettext like `po` files.
  - Loads translations from PO-files
  - Create language specific variants
  - Replaces string instances directly inside the original file
  - Removes overhead through translation as no function call is needed anymore
  - Optimizes template replacement e.g. via %1 into a string "plus" operation
- Industry standard CLDR data for localization support (date formats, calenders, etc.)
  - Rebuilds XML files to nicely useable ultra-modular JSON data classes
  - Integrates with dependency system
  - Fast access to data without additional API possible
  - Supports project fallback chain


## Documentation

- Generating API data from JavaScripts projects with deep-analysis of dependencies etc. The data could be rendered by the [API Browser](https://github.com/zynga/apibrowser) or any other custom application.
- Support for package documentation to allow projects to define introduction documentation for every package/node.
- Generates API data from different JavaScript declarations (core library, native prototype, ...)
  - Supports classes, mixins, interfaces, events and properties
  - Supports links to native types like `String`, `Array`, etc.
  - Merges polyfills to target classes e.g. `String.prototype` even if file is named differently.
- Translates [Markdown](http://daringfireball.net/projects/markdown/) content into HTML and applies syntax highlighting to code sections. Uses GitHubs extended high performance [Sundown](https://github.com/tanoku/sundown) Markdown parser and the widely used [Pygments](http://pygments.org/) for syntax highlighting.
- Support tags for easily marking classes, modules, methods etc.
