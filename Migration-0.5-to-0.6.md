Migration 0.5 to 0.6
====================

- Session, Formatting and Optimization objects are globally initialized
- Projects are automatically scanned for. No need to add projects to the session manually. Configure project dependencies using new "require" section in jasyscript.py. The `addProject()` method is still available but there should not be a need to call it manually.
- Permutations are still configured on the `session` object via `setField` or `permutateField`. Just use the pre-existing object instead of your own instance.
- Permutation instance is not passed around anymore. It's a globally available variable as soon as one call `for xyz in session.permutate(): ...`. The variable of the loop is not important for the system to work. Update your calls to `Resolver`, `Sorter` to not contain a permutation variable anymore.
- Support was added for manual project structures like typical 3rd party JavaScript. It should be now possible to deal with all kinds of other JavaScript libraries. For an idea how to use that feature have a look at the [Jasy Compat](https://github.com/zynga/jasy-compat) project.

- `Asset` was renamed to `AssetManager` to differentiate with the `Asset` class being used by projects right now.
- `storeCompressed` does not support the parameters `formatting`, `optimization` and `permutation` anymore.
- `storeKernel` does not support the `formatting` parameter anymore.
- `session.clearCache()` was renamed to `session.clean()`
- `Resolver` does not has any parameters anymore.
- `Sorter` has only one parameter now (`resolver`)
