# Build Script

The build script is called `jasyscript.py` and should be placed in the root folder of the project. This file is only needed in projects which define own tasks but is not required in library projects which you are including.

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

* The task name is prepended automatically to all destination file names. This is the reason `"simple.js"` is transformed into `"build/simple.js"` automatically when the task is called `build`

