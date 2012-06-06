# Build Script

The build script is called `jasyscript.py` and should be placed in the root folder of the project. This file is only needed in projects which define own tasks but is not required in library projects which you are including.

A trivial example of `jasyscript.py` might look like this:

```python
@task("This is the help text for the build task")
def build():
    # Resolving classes
    resolver = Resolver().addClassName("notebook.Application").getSortedClasses()

    # Write compressed classes
    storeCompressed(classes, "simple.js")
```

This is plain and simple Python code to detect the dependencies of `notebook.Application` and compress all relevant code into `build/simple.js`.