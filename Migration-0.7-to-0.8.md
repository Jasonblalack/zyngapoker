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

## Session



## Self Initialization

There are only two pre-defined objects inside `jasyscript.py`: `session` and `config`. 

* `session`: Instance of `jasy.core.Session` pre-filled with all projects required by the current project
* `config`: Instance of `jasy.core.Config` pre-loaded with configuration data in `jasyscript.yaml`/`jasyscript.json`



## Custom Initialization

Jasy 0.8 moves a lot of methods into modules which where previously global to the `jasyscript.py` file:

* `AssetManager(session)`: manages assets and asset profiles, exports data to client
* `OutputManager(session, assetManager)`: writes kernel, compressed files, deploys assets
* `FileManager(session)`: offers lower level file handling for writing, copying and deleting files.
* `ApiWriter(session)`: supports generating API data for JavaScript classes/projects


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



