Generating API docs in Jasy is a cooperation of three projects:

* Jasy: Parsing and processing JavaScript code + exporting data
* Core: Basic JavaScript library used in conjunction with Jasy projects
* API Browser: Web application for navigating and rendering the Jasy generated data

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

## Documenting

...