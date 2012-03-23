Migration 0.5 to 0.6
====================

- Session, Formatting and Optimization objects are globally initialized
- Projects are automatically scanned for. No need to add projects to the session manually. Configure project dependencies using new "require" section in jasyscript.py. The `addProject()` method is still available but there should not be a need to call it manually.
- Permutations are still configured on the `session` object via `setField` or `permutateField`. Just use the pre-existing object instead of your own instance.
- Permutation instance is not passed around anymore. It's a globally available variable as soon as one call `for xyz in session.permutate(): ...`. The variable of the loop is not important for the system to work. Update your calls to `Resolver`, `Sorter` to not contain a permutation variable anymore.
- Support was added for manual project structures like typical 3rd party JavaScript. It should be now possible to deal with all kinds of other JavaScript libraries. For an idea how to use that feature have a look at the [Jasy Compat](https://github.com/zynga/jasy-compat) project.
- Automatic destination folder (aka `prefix`) handling was added. Typically the destination folder is identical to the name of the task e.g. the results of the task "api" land into the "api" folder (when using the included file APIs, etc.). You can override the default by adding a named parameter `prefix` to your call of the `task` decorator.
- Exception to the previous rule are tasks which contain the word "clean". These are working from the root directory of each project e.g. `removeDir("build")` deletes the build folder in the project's root.
- Projects could now be called from other locations e.g. `jasy -f ~/Workspace/myJasy/Project/jasyscript.py build`.
- Tasks support parameters via the command line. The signature of jasy is now like `jasy --general-options task1 --option1=foo --option2=bar task2 --option1=xyz`. These parameters are passed to the defined methods as named parameters e.g. in `task1` needs to be two named parameters `option1` and `option2`. Parameters of tasks must have a value. Flags are not supported.
- It's possible to call remote tasks while keeping and sharing the prefix with the running task e.g. building another project into the local folder. This can be achieved using the `runTask("projectName", "taskName")`.
- `Asset` was renamed to `AssetManager` to differentiate with the `Asset` class being used by projects right now.
- `Resolver` does not has any parameters anymore. `addClassName` and `excludeClasses` return the `Resolver` instance for making calls chainable.
- `Sorter` has only one parameter now (`resolver`)
- `storeKernel()` does not support the `formatting` and `session` parameters anymore.
- `storeCompressed()` does not support the parameters `formatting`, `optimization` and `permutation` anymore.
- `storeSourceLoader()` was renamed to `storeLoader()` and does not support the parameter `session` anymore.
- `session.clearCache()` was renamed to `session.clean()`
