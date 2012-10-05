Generating API docs in Jasy is a cooperation of three projects:

* _Jasy_: Parsing and processing JavaScript code + exporting data
* _Core_: Basic JavaScript library used in conjunction with Jasy projects
* _API Browser_: Web application for navigating and rendering the Jasy generated data

## Philosophy

### Be smart

At developing the API data generator in Jasy we did not like the approach done by other tools to add tons of comments to the code just to fulfill the needs of the documentation system (e.g. telling a method that it's a member of a class). We preferred adding intrinsic support for specific class and module declarations instead and make supporting them automatically our priority.

### Better comments

We felt that JSDoc repeat a lot of text. This gets especially annoying with short methods and trivial function signatures and documentation tasks. One has to typically write a lot of stuff here as well, just to correctly document things.

### Markdown powered

The API system uses Markdown for everything text. Markdown makes writing readable text easy and could be easily transformed into HTML. It supports injecting code blocks via indenting or via so-called fenced blocks.

### Syntax highlighter

All code blocks are automatically highlighted. Language to highlight can be specified in fenced code blocks (e.g. support for PHP, Python, etc. is included). The default highlighting is JavaScript.



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
* Error Handling
  * Detect and collect documentation errors during generating API data
  * Verify links between classes or to members/events/properties
* Search: Generating word index for client side search logic
* Basic variable resolving logic when using assignments in class declaration
* Private/Internal: Mark private/internal methods and hide them by default
* Caching: Cache API data for all unmodified JavaScript files
* Size Analysis: Show statistics on optimized / compressed size of each class.
* Dependency Analysis: Show which dependencies a class has to better understand the effects of including it.


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




## Formats

### Supported Constructs

Currently Jasy handles code written using the *Core* library or plain simple JavaScript constructs (with no additional magic applied):

* Core Library: 
  * `core.Module`
  * `core.Class`
  * `core.Interface` 
  * `core.Main.declareNamespace`
  * `core.Main.addStatics`
  * `core.Main.addMembers`
* Vanilla JavaScript: 
  * `some.name.Space = {}`
  * `some.name.Space = function() {}` (together with `prototype` for members)

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
  * Implements

New styles which should be added must also conform to these two groups for making further analysis possible.

## Documenting

Code comments are formatted using normal JSDoc like comment styles. Both formats, the compact and verbose, produce the same results. Verbose allows for multiline code blocks, lists, headers and other Markdown features.

### Compact Formatting

```js
/** Calculates the result */
function calc() {}

/** {Number} Returns the result */
function calc() { return this.x1 + this.x2; }

/** {Number} Calculates and returns the sum of @x and @y */
function sum(x, y) { return x+y; }
```

### Verbose Formatting

```js
/** 
 * Calculates the result 
 */
function calc() {}

/** 
 * {Number} Returns the result 
 */
function calc() { return this.x1 + this.x2; }

/** 
 * {Number} Calculates and returns the sum of @x and @y 
 */
function sum(x, y) { return x+y; }
```

**Note 1:** Documentation comments should be placed directly on top of the item they should document. Don't use any new lines after the comment. This would break the association between the comment and the code. You can use as many line breaks before the comment - that does not has any side effect.

**Note 2:** Documentation comments must not mix indenting (tabs/spaces) or number of indention. This breaks correctly parsing the comment. This is especially important to pass a correct extracted text to the Markdown parser and supporting indentation dependent features like block quotes or code sections correctly.

### Protected Comments

Protected comments are used to keep information about copyrights etc. These are kept in tact when and are neither processed by Markdown, syntax highlighting or comment parsing. A protected comment is formatted like a documentation comment with the difference that instead of the second star `*` to mark the comment a `!` is used. Example:

```js
/*!
 * jQuery JavaScript Library v@VERSION
 * http://jquery.com/
 *
 * Copyright 2011, John Resig
 * Dual licensed under the MIT or GPL Version 2 licenses.
 * http://jquery.org/license
 */ 
```

**Note:** There is still some basic outdenting and cleanup logic applied so the actual content of the comment is stored and not the leading stars on every line.


### Parameters

Parameters have to use the `@` in front of the name of the parameter. The type of the parameter or its default values only needs to be assigned once to the parameter. These data is defined using curly brackets `{Number}` after the name of the parameter.

```js
/** 
 * {Number} Calculates and returns the sum of @x {Number} and @y {Number}
 */
function sum(x, y) { return x+y; }
```

Defining these hints directly inside the written sentence makes comments a lot more compact and far easier to read. And it also omits duplicating the work of creating and maintaining these information.


#### Multiple References

The same parameters might be used multiple times in the same comment. It's good style to mark every parameter occurrence with the `@` symbol. You have to write the additional data like type and defaults only once, though. 

```js
/**
 * {Boolean} Returns whether @x is bigger than @y.
 *
 * Parameters:
 *
 * - @x {Number}
 * - @y {Number}
 */
```


In the following example both parameters are type `Number` and not marked as optional and not defined with default values (This means that last parameter specification always wins).

```js
/**
 * {Boolean} Returns whether @x {String ? 13} is bigger than @y.
 *
 * Parameters:
 *
 * - @x {Number}
 * - @y {Number}
 */
```


#### Multiple Types

In both, parameters and return types are *OR* connection of different types is supported. This is done using the pipe `|` symbol:

```js
/** 
 * {Number|String} Returns the combination of @x {Number|String} and @y {Number|String}
 */
function sum(x, y) { return x+y; }
```

White spaces are not important. Same result with:

```js
/** 
 * { Number | String } Returns the combination of @x { Number | String } and @y { Number | String }
 */
function sum(x, y) { return x+y; }
```



#### Optional Parameters

This is as easy as putting a `?` after the type definition

```js
/** 
 * {Number} Calculates and returns the sum of @x {Number} and @y {Number?}
 */
function sum(x, y) { return x + (y || 0); }
```

#### Default values

For parameters which are optional you often like to define a default value. The default value itself needs to be written like a typical JS expression (string with quotes, numbers as numbers, etc.):

```js
/** 
 * {Number} Calculates and returns the sum of @x {Number} and @y {Number?0}
 */
function sum(x, y) { return x + (y || 0); }
```

Here again white spaces are not important:

```js
/** 
 * {Number} Calculates and returns the sum of @x { Number } and @y { Number ? 0 }
 */
function sum(x, y) { return x + (y || 0); }
```

And with a string type default value:

```js
/** 
 * {String} Concats and returns @x {String} and @y {String ? ""}.
 */
function concat(x, y) { return x + (y || ""); }
```


#### Dynamic arguments

For params it's also possible to mark them as dynamic length params using an ellipsis symbol `...` (three dots). This sequence needs to be placed at the end of the curly braces section. An example:

```js
/** 
 * {String} Concats and returns all @strings {String...}.
 */
function concat(strings) 
{ 
  result = [];
  for (var i=0, l=arguments.length; i<l; i++) {
    result.push(arguments[i]);
  }
  return result.join("");
}
```

This also works in combination of static arguments:

```js
/** 
 * {String} Concats and returns all @strings {String...}. 
 * Uses @divider {String ? "|"} for separating the incoming strings.
 */
function concat(divider, strings) 
{ 
  result = [];
  for (var i=1, l=arguments.length; i<l; i++) {
    result.push(arguments[i]);
  }

  return result.join(divider || "|");
}
```

**Note:** Best approach is to place these dynamic arguments lists to the end of the functions signature. This might be documented right when done otherwise, but this is not directly good supported in JavaScript. You have to access the value of `divider` not by its variable name then, but by the position in the arguments list.



### Return Types

If you wondered about the leading `{Number}`. That's the return type of the method `sum`. By convention if the first thing in a comment is a block of curly brackets it is parsed as a return value (or static type). These types also can be of any class available in your projects:

```js
/** 
 * {my.Vector} Combines and results both vector @x {my.Vector} and @y {my.Vector}.
 */
function sum(x, y) { return x+y; }
```

There could also be multiple types of return values.

```js
/** 
 * {Number|String} Returns the combination of @x {Number|String} and @y {Number|String}
 */
function sum(x, y) { return x+y; }
```



### Static Types

These are used on non-functions to define the type for static members. Most of these types are figured out automatically. Static type hints are used to fill in the gaps or to correct misunderstandings by the auto detection in Jasy. Static types are defined just like return types, but the first character in the open brackets needs to be a equal sign.

```js
core.Module("my.Module", 
{
  /** {=Date} Start time */
  start : new Date()
});
```

This works with all the project classes as well:

```js
core.Module("my.Module", 
{
  /** {=my.NumberFormat} Number format to use */
  format : new my.NumberFormat("#.##")
});
```

### Links

Linking works via the same kind of curly braces.

#### Modules / Classes / Interfaces

To link to other files just use the unique file ID aka class name in the curly braces.

```js
/** 
 * Main controller of my application. Connects {my.View} with {my.Data}.
 */
core.Module("my.Controller", {
  ...
});
```
#### Statics / Members / Events / Properties

Internal links to other members/properties/events work but just using a hash symbol `#` in front.

```js
core.Module("my.Module", 
{
  /**
   * {=Date} Start date. Also take a look at {#stop}.
   */
  start : new Date(),

  /** 
   * {=Date} Stop date.
   */
  stop : null
});
```

#### Conflicts / Explicit Type Links

Sometimes there might be different items with the same name in a class e.g. a method `update` and an event `update`. To qualify which target the link should follow you can specify the exact type to link to using prefixes:

* `static:` link to statics section
* `member:` link to members section
* `event:` link to events section
* `property:` link to properties section

Example: `{event:#update}` will link to the event `update` even when there is an identically named member.


#### Combinations

Links to members/events/properties works combining all the previous things:

* Link to event `update` of `my.Loader`: `{my.Loader#update}`
* Making type `event` explicit: `{event:my.Loader#update}`


### Tags

Tags are used to mark an item in a special way. In theory tags could be used as some kind of second level categorization. Typical use cases for tags are:

* deprecation of an item e.g. `#deprecated`
* visibility of an item e.g. `#public`, `#private`, ...
* marking since when item is available e.g. `#since(1.0)`

```js
/** 
 * Main controller of my application.
 *
 * #controller #since(1.3)
 */
core.Module("my.Controller", {
  ...
});
```

**Note 1:** The tags are stripped out the text during the process. They are not kept in place like parameter names. This means using tags outside sentences is a good idea - even if this means some duplication of text.

**Note 2:** Tags can be listed in any way. There is no special line breaks etc. required. You can place them in one line, on multiple lines or in totally different places in the comment text.

There are also some Jasy internal tags to control pass meta data to the dependency tracker in *Jasy*:

* Explicit require: `#require(other.Class)`: Marking `other.Class` as explicitely required - even if not used in code.
* Explicit optional: `#optional(other.Class)`: Marking `other.Class` as optional and not required - even if used in code.
* Explicit break: `#break(other.Class)`: Down prioritizing dependency to other class (fixing circular dependencies)
* Explicit load: `#load(other.Class)`: Requiring but down prioritizing dependency to other class (lazy loading)
* Required assets: `#asset(my/css/*)`: Include all assets whose ID is matched by `my/css/*` 

**Note:** It's not a good idea to use these names for other proposes.


### Built-in types

These types can be used for parameter types, return types etc. as they are built into cleanups.

* `Object`
* `String`
* `Number`
* `Boolean`
* `Array`
* `Function`
* `RegExp`
* `Date`

There are also some other built in "pseudo" types available:

* `true`
* `false`
* `var`: any kind of type
* `null`: explicit null. Typically used in conjuction with "OR" cascades like `{String | null}` for return values.
* `undefined`: nothing
* `this`: Typically used for function return types e.g. to document that this method is chainable.
* `arguments`: Expects an arguments object (Array like)



### Deep Objects