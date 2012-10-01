Jasy allows you to integrate external Python code into your Jasy project, which can be called from your `jasyscript.py` environment. Simply create a `jasylibrary.py` file inside your projects root folder and fill it with Python code. Jasy will automaticly load the content from `jasylibrary.py` into your project.


## Call methods from external libraries

To allow methods to be callable inside your jasy project, you have to place the `@share` flag above them in your librarie code:

```python
@share
def sayHello():
    print("Hello World!")
```

Call this method inside your jasy project with `_yourProjectName_.sayHello()`.


## Manual library import

Outside of using the `jasylibrary.py` file, you have the possibility to manually load python scripts into your project. Use `session.loadLibrary(objectName, fileName)` to load every external python Script. `fileName` will be the path to your script and `objectName` will define a python object from which the script's methods are callable.