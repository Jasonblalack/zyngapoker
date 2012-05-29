# Fields

Fields can be used to pass data from the build process to the application. You can even permutate code and assets to match specific values during build time and add JavaScript code to the application to figure out which one to use.


## Basic Usage

Fields are shared by all projects e.g. an application using a library project also inherits all fields defined by this library. Conflicts result in an exception during session initialization. Be careful on how you name your fields. It is not uncommon to prefix library specific fields with the project name e.g. `richtext.table` instead of just `table`. There is nothing enforced here, though, as there might be also common useful names like `debug`.

Fields can be configured using these keys:

* `detect`: Defines a class name to query for the value. The class defined here is automatically injected into the build.
* `check`: The check to apply to the detected value for validation. Value could be either one of `"Boolean"`, `"String"`, `"Number"` or a list of possible values e.g. `["small", "medium", "large"]`
* `default`: Defines a default value. Used whenever no other value is available.

You can freeze a fields value in your build script using:

```python
session.setField("name", value)
```

Each field may automatically tested via the `detect` class defined in the project's configuration and filled with a value. These detection classes could have a static field `VALUE` or a getter `get("field")` which should return the value of the given field. Using getter methods one can share a detection class for multiple fields (e.g. to return server data, parse the query string, etc.)

Example:

```json
{
  ...
  "fields": 
  {
    "debug" : 
    {
      "check" : "Boolean",
      "default" : false,
      "detect" : "core.detect.Param"
    },
    
    "engine" : 
    {
      "check" : ["webkit", "gecko", "trident", "presto"],
      "detect" : "core.detect.Engine"
    }
  }
}
```

Two fields are defined. First one is boolean and called `debug`. It figures out it's value using the class `core.detect.Param`. This class implements a method `get` which is called with the name of the parameter to figure out:

```
core.Module("core.detect.Param",
{
  get : function(name) 
  {
    ...
    return value;
  }
});
```

In this case no detection happens. This is good to inject values from the outside e.g. the version number of your application. In any case this field have to be defined by the project (e.g. your application) first.



## Access via JavaScript

Inside your JavaScript you have access to the fields configured in your fields using the `core.Env` class (inside the core project).

```js
// read value for local variable
var appTitle = core.Env.getValue("app-title");

// auto queries for a true value
if (core.Env.isSet("debug")) { 
  // debug code
}

// easy string/number compare
if (core.Env.isSet("customer", "premium")) {
  // premium customer
} else if (core.Env.isSet("customer", "plus")) {
  // plus customer
} else {
  // free customer
}
```


## Enable Permutations

Permutations build upon the field configuration. The idea is to combine separate values of different fields into a combination - a so called permutation. This permutation can be used to build a specific optimized JavaScript file (and assets). Each new field which should be permutated multiplies the number of files created e.g.:

* `debug` = `true`/`false`
* `engine` = `gecko`/`webkit`/`trident`/`presto`
* `locale` = `en`, `de`, `fr`

Results in 2*4*3 => 24 files.

Permutations are pretty fast to generate as Jasy keeps track of the fields (using `core.Env`) queried for in each file and caches it accordingly. So generating 40 or 200 makes not really so much difference. Best is to try it out yourself on a real application. Still, if you want to limit the number of files created keep in mind, to only permutate often used or fields with a huge impact to the file size (e.g. free vs. premium user).

These permutated fields might be loaded server-side e.g. when placing the name or the fields into the file. In the loop, where you are creating the permutations, you can ask each permutation object for specific field values e.g. the value of `debug` and inject this into the file name (`permutation.get("debug")`). 

The other, more scalable alternative, which also has better possibilities to figure out in which environment you are executing the application, is to use a kernel script. This kernel script uses the configured tests from the field configuration to automatically determine the value at runtime. With these values and the information about which fields are used for the permutation it is able to know which permutated files to load. This works based on a [SHA1](http://en.wikipedia.org/wiki/SHA1) checksum. 

```js
// Change this line
core.io.Script.load(["myapp.js", ...]);

// to this to include the computed checksum:
var checksum = core.Env.CHECKSUM;
core.io.Script.load(["myapp-" + checksum + ".js", ...]);
```

On the Python side you can inject this checksum as well using `permutation.getChecksum()`. You can construct your filename based on this checksum during your build loop e.g. `"myapp-" + permutation.getChecksum() + ".js"`. Jasy has a built-in feature to generate a kernel script based on the permutated fields in the session. This kernel can be written to a file using the `storeKernel("kernel.js", assets)` command. In your HTML file you integrate the kernel script into the `head` element and load the file using the `Permutation.loadScripts()` command.

Keep in mind that you can use fields without permutation as well e.g. for a version number etc. Fields need not to be permutated for being usable on the application side via `core.Env`.
