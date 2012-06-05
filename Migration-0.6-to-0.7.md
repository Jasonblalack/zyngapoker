# Migration 0.6 to 0.7

## Asset Handling

The asset handling was revamped by major extend to make it more flexible, modular and efficient:

- **flexible**: multiple roots are supported through the usage of profiles
- **modular**: assets are not registered by the kernel anymore, but by any collection of compiled/source classes individually
- **efficient**: internal storage format has got major optimizations

In the jasyscript.py you must change a few lines for being compatible against the new API:

```python
storeKernel("script/kernel.js", assets=myassets)
```

to:

```python
storeKernel("script/kernel.js")
```

The asset manager is working globally on the per-session basis. It is automatically used by each `storeCompressed` or `storeLoader` at adds assets for the included classes. This new modularity allows you also to easily split your build further while keeping asset information locally to the code requiring it.

You have to configure the assets before building any of packages using `storeKernel`, `storeCompressed` or `storeLoader`. This means that `addSourceProfile`, `addBuildProfile` or `addProfile` should be called early in most tasks.

There are pre-configured methods for dealing with typical source/build scenarios to ease usage:

* `assetManager.addSourceProfile(urlPrefix="", override=False)`: Configure all known assets to being loaded from the source folders
* `assetManager.addBuildProfile(urlPrefix="asset", override=False)`: Configure all known assets to being loaded from the local e.g. asset folder.

For more complex scenarios e.g. remote asset management you can also use `assetManager.addProfile()` for full control over the internal asset data. For more information on the new feature consult the official documentation.

For deploying assets during the e.g. `build` task you now have to call `deploy` on your own:

```python
# Copy over assets
assetManager.deploy(Resolver().addClassName("my.main.Class").getIncludedClasses(), assetFolder="asset")
```

There you are able to define a custom asset folder. The name of the folder is `asset` by default.


## Store Loader/Compressed

The signature of `storeCompressed` and `storeLoader` has been modified a bit. The list of classes has been moved to the front:

```python
storeCompressed("script/my.js", sortedClasses, bootCode)
storeLoader("script/my.js", sortedClasses, bootCode)
```

you now use:

```python
storeCompressed(sortedClasses, "script/my.js", bootCode)
storeCompressed(sortedClasses, "script/my.js", bootCode)
```

## Formatting/Optimization

Rename global `formatting`/`optimization` objects with `js` prefix:

* `formatting` => `jsFormatting`
* `optimization` => `jsOptimization`

Otherwise these are the same objects as before. Rename happened for preparation of supporting new language/derivates (e.g. coffeescript, less, sass, etc.)

## Sorting classes

The `Sorter` class is not available to the jasyscript.py environment anymore. Just call `getSortedClasses` on the `Resolver` instance instead.

```python
resolver = Resolver().addClassName("foo.Bar")
classes = Sorter(resolver).getSortedClasses()
```

to

```python
classes = Resolver().addClassName("foo.Bar").getSortedClasses()
```

## Cleanups

* Removed typically unused `storeCombined()` method.