# Build Script

The most trivial example of a build script (`jasyscript.py`)

```python
@task("Help Text")
def simple():
    # Resolving classes
    resolver = Resolver().addClassName("notebook.Application")
    classes = Sorter(resolver).getSortedClasses()

    # Write compressed classes
    storeCompressed("build/simple.js", classes)
```