# Build Script

The build script is called `jasyscript.py` and should be placed in the root folder of the project. This file is only needed in projects which define own tasks but is not required in library projects which you are including.

## Showing help

Whenever you call `jasy` without arguments or with an explicit `-h`/`--help` you will be presented the help screen. The help screen will list you all possible options and available tasks with their description.

## Simple Example

A trivial example of `jasyscript.py` might look like this:

```python
@task("This is the help text for the build task")
def build():
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
@task("This is the help text for the build task")
def build(formatting="off"):
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
@task("Cleaning the cache files")
def clean():
    session.clean()
```