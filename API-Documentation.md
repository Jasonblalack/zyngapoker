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


## Philosophy

### Don't repeat yourself

At developing the API data generator in Jasy we did not like the approach done by other tools to add tons of comments to the code just to fulfill the needs of the documentation system (e.g. telling a method that it's a member of a class). We preferred adding intrinsic support for specific class and module declarations instead and make supporting them automatically our priority.

### Less but better comments

We felt that JSDoc repeat a lot of text. This gets especially annoying with short methods and trivial function signatures and documentation tasks. One has to typically write a lot of stuff here as well, just to correctly document things.



## Documenting

### Supported Constructs

* Core Library: `core.Module`, `core.Class`, `core.Interface` (also: `core.Main.declareNamespace`, `core.Main.addStatics`, `core.Main.addMembers`)
* Standard JavaScript Prototype: `some.name.Space = {}` or `some.name.Space = function() {}` (incl. prototype)

Jasy currently support a se