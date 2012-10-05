Generating API docs in Jasy is a cooperation of three projects:

* _Jasy_: Parsing and processing JavaScript code + exporting data
* _Core_: Basic JavaScript library used in conjunction with Jasy projects
* _API Browser_: Web application for navigating and rendering the Jasy generated data

## Task

Generating the API data and the API browser is pretty easy by simply adding a new task to your `jasyscript.py` and fulfilling the requirements of the projects required.

Add this task to your `jasyscript.py`:

```python
@task
def api():
    """Build API viewer"""

    Task.runTask("apibrowser", "build")
    ApiWriter(session).write("$prefix/data")
```

Be sure to have *Core* and *API Browser* in your project requirements (jasyproject.yaml/json) e.g.:

```
requires:
- source: https://github.com/zynga/core.git
  version: 0.8
- source: https://github.com/zynga/apibrowser.git
  version: 0.6
```

Then generating the API Browser for your project (and all required projects included) is as simple as:

```bash
$ jasy api
```

This generates a new top level folder called "api" with:

* The compiled API Browser
* Jasy generated API data for your projects
* Highlighted JavaScript files

The folder is self-contained an can be moved/copied anywhere easily.


## Features

* Syntax Types
  * All *Core* features including multiple inheritance, mixins, interfaces, events and properties
    * Copying documentation from interfaces
    * Inlining members, events, properties from mixins while highlighting their origin
    * Mark overridden items
  * Plain JavaScript object assignment 
  * Plain JavaScript prototype based classes
* Linking
  * Generating API data across all included projects
  * Cross project inheritance/include/override
  * Linking to methods, properties, events
* Tagging
  * Marking all documented items via tags for simple categorization features e.g. `#deprecated`
  * Parametrized tags e.g. `#since(1.0)`.
* Highlighting:
  * Full JavaScript source code
  * Code section in documentation comments
* Package Docs: 
  * Package level documentation (placing an `index.md` or `readme.md` inside any class folder).
  * Auto listing and summarizing classes of a package in package overview page.
* Markdown
  * Convert all documentation comments via Markdown for easy and writable and human readable documentation comments.
  * Convert package docs using Markdown as well.
  * Generate plain summary texts for all comments based on the first sentence to show in compact/list views.
* Parameters
  * Full range of parameter handling with support for marking as optional, with default values
  * Support for variable arguments lists e.g. `function sum(varargs) { return ... }`
* Exporting
  * Generated separate files for dynamic loading of class data
  * Meta data index for building a tree/list of all classes
* Search: Generating word index for client side search logic
* Error Handling
  * Detect and collect documentation errors during generating API data
  * Verify links between classes or to members/events/properties
* Basic variable resolving logic when using assignments in class declaration
* Private/Internal: Mark private/internal methods and hide them by default
* Caching: Cache API data for all unmodified JavaScript files
* Size Analysis: Show statistics on optimized / compressed size of each class.
* Dependency Analysis: Show which dependencies a class has to better understand the effects of including it.

## Philosophy

### Don't repeat yourself

At developing the API data generator in Jasy we did not like the approach done by other tools to add tons of comments to the code just to fulfill the needs of the documentation system (e.g. telling a method that it's a member of a class). We preferred adding intrinsic support for specific class and module declarations instead and make supporting them automatically our priority.

### Less but better comments

We felt that JSDoc repeat a lot of text. This gets especially annoying with short methods and trivial function signatures and documentation tasks. One has to typically write a lot of stuff here as well, just to correctly document things.



## Documenting

### Supported Constructs

Currently Jasy handles code written using the *Core* library or plain simple JavaScript constructs (with no additional magic applied):

* Core Library: `core.Module`, `core.Class`, `core.Interface` (also: `core.Main.declareNamespace`, `core.Main.addStatics`, `core.Main.addMembers`)
* Standard JavaScript Prototype: `some.name.Space = {}` or `some.name.Space = function() {}` (incl. prototype)

### Future Extendable

The approach in general is designed to be extendable so more styles might be supported in future versions. Jasy generally supports these lists for each class/file:

* Module
  * Statics
* Class
  * Constructor
  * Members
  * Properties
  * Events
  * Includes

New styles which should be added must also conform to these two groups for making further analysis possible.

