# Build Script

The build script is called `jasyscript.py` and should be placed in the root folder of the project. This file is only needed in projects which should define own tasks and is not required in library projects which you are including.

The most trivial example of `jasyscript.py` might look like this:

```python
@task("Help Text")
def build():
    # Resolving classes
    resolver = Resolver().addClassName("notebook.Application")
    classes = Sorter(resolver).getSortedClasses()

    # Write compressed classes
    storeCompressed("simple.js", classes)
```

This is plain and simple Python code to detect the dependencies of `notebook.Application` and compress all relevant code into `build/simple.js`.