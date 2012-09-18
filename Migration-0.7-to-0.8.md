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


## File Handling

File handling does not prepends the task-name based prefix anymore. It uses a new pattern replacement inside filenames to unify replacing values inside files/paths instead.

Instead of writing to e.g. `index.html` you now write to `$prefix/index.html` which is much cleaner in calls like this one:

```python
storeKernel("script/kernel.js") => storeKernel("$prefix/script/kernel.js")
updateFile("source/index.html", "index.html") => updateFile("source/index.html", "$prefix/index.html")

```

