# Fields

Fields allow for configuration flags in projects. They are shared by all projects e.g. an application using a library project also inherits all fields defined by this library. Conflicts result in an exception during session initialization time. Be careful on how you name your fields. It is not uncommon to prefix library specific fields with the project name e.g. `richtext.table` instead of just `table`. There is nothing enforced here, though, as there might be also common useful names like `debug`. Please note that you can override default values or test classes in your build script.

Fields can be configured using these keys:

* `detect`: Defines a class name to query for the value. The class defined here is automatically injected into the build.
* `check`: The check to apply to the detected value for validation. Value could be either one of `"Boolean"`, `"String"`, `"Number"` or a list of possible values e.g. `["small", "medium", "large"]`
* `default`: Defines a default value. Used when validation fails or no value was detected.

Each field is automatically tested via the `detect` class defined in the project's configuration and filled with a value. These detection classes could have a static field `VALUE` or a getter `get("field")` which should return the value of the given field. Using getter methods one can share a detection class for multiple fields (e.g. to return server data, parse the query string, etc.)

You can hard-configure a field in your build script using as well:

```python
session.setField("name", value)
```

In this case no detection happens. This is good to inject values from the outside e.g. the version number of your application. In any case this field have to be defined by the project (e.g. your application) first.



## Environment

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


## Permutations

Permutations build upon the field configuration. The idea is to basically combine separate values of different fields into a combination - a so called permutation. This permutation can be used to build a specific optimized JavaScript file. Each new field which should be permutated multiplies the number of files created e.g.:

* `debug` = `true`/`false`
* `engine` = `gecko`/`webkit`/`trident`/`presto`
* `locale` = `en`, `de`, `fr`

Results in 2*4*3 => 24 files.

If you want to limit the number of files created keep in mind, to only permutate often used or otherwise important fields (e.g. free vs. premium user). Permutations are pretty fast to generate as Jasy keeps track of the fields (using `jasy.Env`) queried for in each file and caches it accordingly. So generating 40 or 200 makes not really so much difference. Best is to try it out yourself on a real application.

These permutated fields might be loaded server-side e.g. when placing the name or the fields into the file. In the loop, where you are creating the permutations, you can ask each permutation object for specific field values e.g. the value of `debug` and inject this into the file name (`permutation.get("debug")`). 


## Kernel Script

The other more scalable alternative, which also has better possibilities to figure out which client you are using, is to use a kernel script. This kernel script uses the configured tests from the field configuration to automatically determine the value at runtime. With these values and the information about which fields are used for the permutation it is able to know which permutated files to load. This works based on a [SHA1](http://en.wikipedia.org/wiki/SHA1) checksum. 

```js
// Instead of loading your scripts using:
core.io.Script.load(["myapp.js", ...]);

// you are using:
core.Env.loadScripts(["myapp.js", ...]);

// the files loaded are automatically patched to inject the SHA1 checksum:
"myapp.js" => "myapp-8a401c44a4c74321062b8378176be3f5636ff3ed.js"
```

On the Python side you can inject this checksum as well using `permutation.getChecksum()`. You can construct your filename based on this checksum during your build loop e.g. `"myapp-" + permutation.getChecksum() + ".js"`. Jasy has a built-in feature to generate a kernel script based on the permutated fields in the session. This kernel can be written to a file using the `storeKernel("kernel.js", assets)` command. In your HTML file you integrate the kernel script into the `head` element and load the file using the `Permutation.loadScripts()` command.
