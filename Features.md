# Concepts

Jasy is a construction tool for web projects. Think of Jasy as a replacement for the classic make utility heavily inspired by modern tools like [SCons](http://www.scons.org/) and [Waf]().

# What makes Jasy better?

- [Make](http://www.gnu.org/software/make/) requires the use of its own [domain specific language](http://en.wikipedia.org/wiki/Domain-specific_language) — this is, in general, not a good idea. Configuration files in Jasy are Python scripts -- use the power of a real programming language to solve build problems. 
- Full support for complex projects with dependencies to libraries and frameworks.
- Deep language support through being based on a full-blown JavaScript parser which itself is based on the mature Spidermonkey parser from Mozilla.
- Reliable, automatic dependency analysis built-in for JavaScript files.
- Integrated path abstraction support to access assets (images, CSS files, ...) using project IDs instead of folder on the client side.


# Features

- Integrated GIT support supports auto-cloning and managing of used libraries and frameworks.
- Support for internationalization and localization (gettext/CLDR based)
- Support for generating and using image sprites.
- Support for defining and using sprite animations.







## General

- Build scripts are plain Python and can do everything you want to do. No limitations.
- Import Jasy and define any number of custom tasks
- Project support bundle every project into it's own config and omits copying files around manually
- Permutation Support (building different results from one code base). Might be used to remove debug blocks or alternative code
- Supports a so-named "source" version which loads the original class files which is useful during the development phase of an application.


## JavaScript

### Parser

- Full JavaScript 1.8 parser based on Narcissus (which itself is based on Spidermonkey)
- Reworked parser for better child handling (easier to traverse tree compared to original)
- Full support for JavaScript 1.8 (Generators, Block Scope, Function Expressions, Array Comprehensions, ...)
- Comment processing (Comments are attached to nodes and are part of the AST)

### Dependency Analysis

- Dependency Analysis (with support for permutations). 
- All dependencies are regarded as load-time specific.
- Automatic and fine-tunable breaking of circular dependencies.
- Detects all objects which are not declared in the file itself e.g. window, document, my.namespaced.Class, etc.

### Dead Code Removal

- Resolves conditions and removes blocks which could not be reached
- Function part of the permutation support
- Supports switch statements
- Supports conditional statements (?:)
- Resolves boolean, number and string compares
- Supports AND and OR operators

### Compression

- Comment Removal
- White Space Removal (even in some quirky places e.g. keywords before strings, etc.)

### Optimizer

- Renames local variables/functions/exceptions (based on their usage number)
- Renames file private variables (starting with double underscore by convention)
- Combines multi var statements into one per function (really all of them)
- Removes needless blocks (with just one statement)
- Automatically combines strings and numbers (e.g. "Version " + 1.3 => "Version 1.3")
- Optimizes if(-else) statements with expressions as content
  - Translates if-statements without else using `&&` or `||` operators
  - Translates if-statements with else using conditional operator `? :` (especially worth with returns/assignments)
- Removes needless else (if previous if-block ends with a return/throw statement)
- Removes needless parens based on priority analysis on the AST

### API Data

- Generates API data from different JavaScript declarations (core library, native prototype, ...)
- Supports classes, mixins, interfaces, events and properties
- Supports links to native types like String, Array
- Merges polyfills to target classes
- Translates markdown content into HTML and applies syntax highlighting to code sections
- Support tags on any statics, members, etc.


## Localization

- Unicode CLDR Data Transformation
  - Rebuilds XML files to nicely useable ultra-modular JSON data classes
  - Integrates with dependency system
  - Fast access to data without additional API possible
  - Supports project fallback chain
- PO-File translations
  - Loads translations from PO-files
  - Create language specific variants
  - Replaces string instances directly inside the original file
  - Removes overhead through translation as no function call is needed anymore
  - Optimizes template replacement e.g. via %1 into a string "plus" operation