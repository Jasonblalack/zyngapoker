# Features

## 1. Projects

### Project Dependencies

Each Jasy project is able to define requirements to other projects. These requirements are solved recursively and duplicates (projects with the same name) are filtered out. Projects can also be overridden by projects being higher in the dependency chain.

### Custom Project Layout

Jasy supports a set of layouts inside projects. For adding support for the various 3rd party libraries and frameworks Jasy is able to use a custom set of files and is able to files to internal file IDs (basically the exported name of a file e.g. jQuery exports `$`).

### Project Setup Routines

Some projects are using custom build routines which applies some kind of magic to the code before concatting single files (aka adding intro/outro content, patching files with version numbers, etc.). For these cases Jasy is able to define setup routines which should be executed whenever a project is initially used or updated.

### Remote Projects





## 2. JavaScript 

### Parser

- Deep JavaScript language support through being based on a full-blown parser which itself is based on the mature Spidermonkey parser from Mozilla. Jasy even fully supports upcoming features like generators, array comprehension, etc. of JavaScript 1.8.
- Generates a easy to process [AST](http://en.wikipedia.org/wiki/Abstract_syntax_tree). This `AST` can be used to transform the original code for e.g. API documentation, code formatting, code linting, optimization, etc.
- Handles comments during parse process and attaches the comment informations to the generated `AST` nodes.

### Compressor

### Optimizer

- Removes white space, comments and any formatting from original code.
- Inlines translation into the permutated application code. No more lookup for translation texts once compiled.
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

### Dependencies

- Reliable, automatic dependency analysis for JavaScript is built in. Automatically sorts files by their requirements. Dependency analysis happens with permutations applied (e.g. debug builds might contain classes deployment builds might not contain).





## 3. Assets

- Integrated path abstraction support for assets (images, CSS files, ...) using project IDs instead of filesystem paths on the client side.
  - Builds a client accessible database of available assets and their meta data. Offers easy to use APIs to access that data.
  - Support for enterprise grade CDN hosting and checksum based URLs to load assets.
- Generating and using image sprites (with easy to use client side API to transparently deal with the original file names).
- Defining and using sprite animations via simple JSON files (for graphically rich animations).
- Copies all assets to bundle them in a self contained folder. This simplifies deployment by a major extend.





## 4. Permutations

Supports permutations to fully remove code blocks, classes, assets, translations which are not required for the current build.



## 5. Localization

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


## 6. Documentation

- Generating API data from JavaScripts projects with deep-analysis of dependencies etc. The data could be rendered by the [API Browser](https://github.com/zynga/apibrowser) or any other custom application.
- Support for package documentation to allow projects to define introduction documentation for every package/node.
- Generates API data from different JavaScript declarations (core library, native prototype, ...)
  - Supports classes, mixins, interfaces, events and properties
  - Supports links to native types like `String`, `Array`, etc.
  - Merges polyfills to target classes e.g. `String.prototype` even if file is named differently.
- Translates [Markdown](http://daringfireball.net/projects/markdown/) content into HTML and applies syntax highlighting to code sections. Uses GitHubs extended high performance [Sundown](https://github.com/tanoku/sundown) Markdown parser and the widely used [Pygments](http://pygments.org/) for syntax highlighting.
- Support tags for easily marking classes, modules, methods etc.