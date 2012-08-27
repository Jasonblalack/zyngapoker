# Build Script

The build script is called `jasyscript.py` and should be placed in the root folder of the project. This file is only needed in projects which define own tasks but is not required in library projects which you are including.

## Showing help

Whenever you call `jasy` without arguments or with an explicit `-h`/`--help` you will be presented the help screen. The help screen will list you all possible options and available tasks with their description and arguments.

## Simple Example

A trivial example of `jasyscript.py` might look like this:

```python
@task
def build():
    """This is the help text for the build task"""

    # Resolving classes
    classes = Resolver().addClassName("notebook.Application").getSortedClasses()

    # Write compressed classes
    storeCompressed(classes, "simple.js")
```

This is plain and simple Python code to detect the dependencies of `notebook.Application` and compress all relevant classes into `build/simple.js`. 

* The task name is prepended automatically to all destination file names. This is the reason `simple.js` is transformed into `build/simple.js` automatically when the task is called `build`.
* The dependency handling starts with the classes added via `addClassName` and processes classes based on unresolved names in each file until no more relevant files are required for inclusion.
* Dependency tracking works by full references to other classes. It does not work when classes are accessed using `[]`, via strings e.g. `my["ui"].Button` or via variables e.g. `var myui = mylib.ui; var btn = myui.Button();` does not figure out that you use `mylib.ui.Button`.
* There is an alternative to `getSortedClasses()` called `getIncludedClasses()` which works without sorting. That's somewhat faster and might be relevant in cases where you require a simple list, but the order is not important.
* The return value of `getSortedClasses()` and `getIncludedClasses()` is a Python `list` with Jasy `Class` objects. These objects allow easy access to things like dependencies, meta data, compressed code, etc.
* The compressor uses a global configuration defined via the `jsFormatting` and `jsOptimization` objects. You can configure these objects anywhere in your code via `enable("feature")` and `disable("feature")`.


## Passing arguments to tasks

You are able to pass (string) arguments from the outside to any jasy task:

```bash
$ jasy build --formatting on
```

To make this possible you have to change your build task a little:

```python
@task
def build(formatting="off"):
    """This is the help text for the build task"""

    # Resolving classes
    classes = Resolver().addClassName("notebook.Application").getSortedClasses()

    # Enable formatting of code when user passes parameter
    if formatting == "on":
        jsFormatting.enable("semicolon")
        jsFormatting.enable("comma")

    # Write compressed classes
    storeCompressed(classes, "simple.js")
```

These parameters should define default values to make it possible to leave them out. Requiring these arguments is typically not a good idea.

When you run the `jasy` command you are able to see that all classes are reprocessed for compression. The Jasy cache is modular and stores very specific entries e.g. the exact set of formatting, optmization, etc. of that exact class. So after calling `jasy` you know have two compressed results of every class in the cache.


## Clearing the cache

Typically it's not needed to clear the cache at all. Jasy is pretty good at invalidating the cache whenever changes occur. And Jasy automatically clears cache files whenever a version change of Jasy is detected. This means other than for cleaning up there is no real requirement to cleanup the caches at all.

```python
@task
def clean():
    """Cleaning the cache files"""

    session.clean()
```


## Working with assets

Jasy can pass data about assets (images, styles, fonts, etc.) to your generated JavaScript files. This happens somewhat automatically based on the needs to the classes. A class which requires specific assets can do this via simple documentation comments:

```javascript
/**
 * #asset(notebook/css/*)
 */
MyClass = function() {
  core.io.Asset.load(["notebook/css/main.css"], function() {
    alert("CSS files was loaded and applied");
  });
};
```

This so-called "asset" hint defines that all assets of the project "notebook" in the folder "css" should be added whenever this class is included. This happens via the already used method `storeCompressed` (and also `storeLoader` and `storeKernel`). 

Including asset means to add information about the existence of these assets to the client side JavaScript. This is especially useful for preloading assets like images, knowing about image sizes before actually loading them, etc. Asset management also simplifies access to assets and unifies their usage inside the application code. You don't work with absolute or relative paths anymore, but just use assets by the project's name they belong to independently of their current location.


### Configuring Profiles

To make loading actually working in your project you have to attach profile(s) to your assets to let them know where to find them. This makes it possible to load assets from different locations, servers, CDNs etc. without changing a single line in your application code. 

To use assets in our current project we add a call to `assetManager.addBuildProfile()` to our `jasyscript.py`:

```python
assetManager.addBuildProfile()
```

This adds data to your assets so that they will be loaded from a local folder (called `asset` by default). This means even assets from other projects will be expected to be loaded from there aka all assets are merged into this single folder. As adding the data is not enough, you also need to copy these assets around. You don't have to do this manually though. 

```python
assetManager.deploy(classList)
```

As you can see `deploy` needs the list of classes for deployment as well. So we can share it. This is how the build script can look like now:

```python
@task
def build(formatting="off"):
    """This is the help text for the build task"""

    # Resolving classes
    classes = Resolver().addClassName("notebook.Application").getSortedClasses()

    # Use assets from local asset folder
    assetManager.addBuildProfile()
    
    # Copy over all used assets to local asset folder
    assetManager.deploy(classes)
  
    # Enable formatting of code when user passes parameter
    if formatting == "on":
        jsFormatting.enable("semicolon")
        jsFormatting.enable("comma")

    # Write compressed classes
    storeCompressed(classes, "simple.js")

@task("Cleaning the cache files")
def clean():
    session.clean()
```

If you run that task you now should have the following structure in your project's `build` folder:

```
- build
  - script
    - kernel.js
  - asset
    - notebook
      - css
        main.css
```

- `kernel.js` contains all classes required by `notebook.Application`
- the `asset` folder contains all assets required to run that application class

This means you have an fully standalone, deploy-ready application inside the `build` folder now. What might be missing is some kind of `HTML` file you are using to open your application from inside the browser. Typically that file is placed in the root folder of your `source` folder. You need to copy over that file to the build folder:

```python
@task
def build(formatting="off"):
    """This is the help text for the build task"""
    
    # Resolving classes
    classes = Resolver().addClassName("notebook.Application").getSortedClasses()

    # Use assets from local asset folder
    assetManager.addBuildProfile()
    
    # Copy over all used assets to local asset folder
    assetManager.deploy(classes)
    
    # Copy over HTML file(s)
    updateFile("source/index.html", "index.html")
  
    # Enable formatting of code when user passes parameter
    if formatting == "on":
        jsFormatting.enable("semicolon")
        jsFormatting.enable("comma")

    # Write compressed classes
    storeCompressed(classes, "simple.js")

@task("Cleaning the cache files")
def clean():
    session.clean()
```

The modification copied over the file `source/index.html` to `build/index.html`. As already mentioned the destination folder is typically auto prepended based on the name of the current task e.g. `build`. 


## Using Fields/Permutations

One of the nice features of Jasy is integrated handling for fields and permutations. This means you can pass arbitrary information from the build script to the client and also build different output scripts or structures for different environments/devices/locales.

### Accessing fields

You can access fields by APIs of the Core Framework:

* `core.Env.getValue("fieldName")` => `var`
* `core.Env.isSet("fieldName", expectedValue)` => `bool`
* `core.Env.isSet("fieldName")` => `bool`
* `core.Env.select("fieldName", {expectedValue1: result1, expectedValue2: result2})` => `var`

The values will be automatically injected by Jasy like that:

```
if (core.Env.isSet("debug", true)) {
  console.log("VAR: ", myvar);
}
```

If you set `debug` to `true` that's the result:

```
if (true) {
  console.log("VAR: ", myvar);
}
```

But that's not all. The dead code optimizer removes all un-reachable code and optimizes all `true` cases automatically as well. So the final result for the compressor looks like:

```
console.log("VAR: ", myvar);
```

### Setting fields

Fields can only be set inside `jasyscript.py` and not on the client side anymore. To configure a field you have two approaches:

* `setField("fieldname", fieldvalue)`
* `permutateField("fieldname")`

The first sets the value of the field to exactly the given value. The second one cycles through all possible values. Possible values could be defined by the `jasyproject.json` which defines the field or can be defined in the `jasyscript.py`:

`permutateField("fieldname", valueList, detectionClass, defaultValue)`

* `valueList`: A list of values to build e.g. `["desktop", "phone", "tablet"]`
* `detectionClass`: Class to detect the value of the given field on the client.
* `defaultValue`: Value to use when `detectionClass` is not configured or fails with detection.



