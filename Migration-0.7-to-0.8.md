Jasy 0.8 is a major rework of a lot of internal APIs in Jasy. While keeping most approaches the usage of the actual APIs changed by some extend. The goals were:

* Less pre defined objects in `jasyscript.py`
* Fully sandboxed environment in `jasyscript.py`
* Possibility to run multiple tasks in sequence without unwanted side effects
* Cleanup of prefix handling to use less magic

## Task documentation

The documentation for tasks has been moved from the decorator call to the method's doc string. Change:

```python
@task("Doc")
def mytask():
  ...
```

to

```python
@task
def mytask():
  """Doc"""
  ...
```


## Self Initialization

There are only two pre-defined objects inside `jasyscript.py`: `session` and `config`. 

* `session`: Instance of `jasy.core.Session` pre-filled with all projects required by the current project
* `config`: Instance of `jasy.core.Config` pre-loaded with configuration data in `jasyscript.yaml`/`jasyscript.json`



## Custom Initialization

Jasy 0.8 moves a lot of methods into modules which where previously global to the `jasyscript.py` file:

* `AssetManager(session)`: manages assets and asset profiles, exports data to client
* `OutputManager(session, assetManager)`: writes kernel, compressed files, deploys assets
* `FileManager(session)`: offers lower level file handling for writing, copying and deleting files.

This means that these objects are typically all unique for every task and could redo there work when executing multiple tasks in a row without influencing each other. This is much better than previously.


## Lazy Scanning

Projects are now scanned lazily. This basically means that instead of indexing every project in the session on boot for every task it is now post-poned to the moment when data is required the first time. This allows you e.g. to execute other generators for projects before actually letting the scanning etc. fill up caches. This is useful for a full range of requirements e.g. executing `grunt` in `jQuery` before actually including it, pre-compile `.sass` files into `.css` before deploying them via the `AssetManager`, loading/syncing configuration or assets from a server and putting them in the assets folder, etc.



## Project Library

Each project can now offer Python methods to other projects including it. This is implemented adding a `jasylibrary.py` to the project and adding a `@share` decorator (like `@task` in `jasyscript.py`) to all methods which should be exported. The methods are being made available using an object instance named as the project is named e.g. `mylibrary` exports all methods under `mylibrary.mymethod()` etc.

This new approach can be seen as the successor of the `Task.runTask(project, taskname)` command.


## Built-in Tasks

New in Jasy 0.8 is a feature called built-in tasks. These are tasks which are pre-defined in Jasy and do not require and project and specific project structure/setup.

* `about`: Shows about screen
* `create`: Scaffolds a new project from local or remotely hosted skeletons
* `doctor`: Troubleshooting for the environment which also offers help for installing required Python modules etc.
* `help`: Shows help screen with global options and available task list
* `showapi`: Prints out a detailed help screen about the API made available in `jasyscript.py`


## Logging

A new module `Console` with methods like `info()`, `debug()`, `warn()`, etc. offers Firebug like printing in your custom tasks or `jasylibrary.py` files.



## Compression and Formatting

Jasy 0.8 changes the approach of formatting and compression. Instead of a lot of custom options it basically simplifies usage to simple numeric values. These can be configured centrally for a build using constructor parameters on the `OutputManager`:

* `compressionLevel`:
  * 0: Disabled specific compression features
  * 1: Simple optimization features only
  * 2: More aggressive optimization
* `formattingLevel`: 
  * 0: No new lines, all content written to a single line
  * 1: New lines after comma and semicolons



## File Handling

File handling does not prepends the task-name based prefix anymore. It uses a new pattern replacement inside filenames to unify replacing values inside files/paths instead.

Instead of writing to e.g. `index.html` you now write to `$prefix/index.html` which is much cleaner in calls like this one:

```python
storeKernel("script/kernel.js") => storeKernel("$prefix/script/kernel.js")
updateFile("source/index.html", "index.html") => updateFile("source/index.html", "$prefix/index.html")

```

Generic file handling with support for this prefix handling is now moved into a class. These methods were moved:

* `removeFile`
* `updateFile`
* `writeFile`
* `makeDir`
* `removeDir`
* `copyDir`

So instead of calling e.g. `removeFile()` you now should use `fileManager.removeFile()`



## Filename Placeholders:

There are some new built-in placeholders in filenames:

* `$prefix`: is replaced with the current prefix, typically the name of the task
* `$permutation`: current SHA1 checksum of permutation. Calls `permutation.getChecksum()` internally.
* `$locale`: current locale in permutation loop e.g. `de_DE`


## Resolving Dependencies

The `Resolver` needs to be initialized with the session object:


```python
Resolver() => Resolver(session)

```


## Filtering Kernel Classes

The `OutputManager` automatically filters kernel classes from all further builds. This means that the code inside `jasyscript.py` has got a little simpler around this:

```python
includedByKernel = storeKernel("script/kernel.js")
resolver = Resolver().addClassName("myproject.Main").excludeClasses(includedByKernel)
outputManager.storeCompressed(resolver.getSortedClasses(), "script/myproject.js", "new myproject.Main;")
```

The new way looks a little cleaner:

```python
outputManager.storeKernel("script/kernel.js")
resolver = Resolver().addClassName("myproject.Main").excludeClasses(includedByKernel)
outputManager.storeCompressed(resolver.getSortedClasses(), "$prefix/script/myproject.js", "new myproject.Main;")
```


## Configuration Object

Instead of placing your configuration inside your `jasyscript.py` you are now able to have a `jasyscript.json`/`jasyscript.yaml` inside the same folder to pass a so-called configuration object to the `jasyscript.py`.



## Generating API Docs

The `ApiWriter` needs to be initialized with the session object:


```python
ApiWriter().write("data") => ApiWriter(session).write("$prefix/data")

```



## Deploying Assets

Assets are now deployed by the `OutputManager` and not anymore using the `AssetManager`. Instead of delivering a full set of classes to the output manager you know only have to call it with the main class you want to include (and its assets).

```python
assetManager.deploy(sortedClassList) => outputManager.deployAssets(["main.Class"])

```

This also offers to define a list of classes to deploy assets for, but typically all you need is your main application class.



## Generating API Docs

The usage for generating API docs has been slightly modified:

```python
ApiWriter().write("data") => ApiWriter(session).write("$prefix/data")
```


## Cleaning up

Instead of manual calls to `removeFile()` and `removeDir()` you are now able to call `Repository.clean()` or `Repository.distclean()`. Be careful with these methods though as they might delete uncommited files from your folders, too e.g. the source folder.



