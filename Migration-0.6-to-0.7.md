# Migration 0.6 to 0.7

## AssetManager

The asset handling was revamped by major extend to make it more flexible, modular and efficient:

- flexible: multiple roots are supported through the usage of profiles
- modular: assets are not registered by the kernel anymore, but by any collection of compiled/source classes individually
- efficient: internal storage format has got major optimizations

In the jasyscript.py you must change these lines:

```python
storeKernel("script/kernel.js", assets=myassets)
```

to:

```python
storeKernel("script/kernel.js")
```

The asset manager is working globally on the per-session basis. It is automatically used by each `storeCompressed` or `storeLoader`.

You have to configure the assets before building any of packages using `storeKernel`, `storeCompressed` or `storeLoader`.

There are pre-configured methods for dealing with typical source/build scenarios:

* `session.getAssetManager().addSourceProfile(urlPrefix="", override=False)`: Configure all known assets to being loaded from the source folders
* `session.getAssetManager().addBuildProfile(urlPrefix="asset", override=False)`: Configure all known assets to being loaded from the local e.g. asset folder.

For more information on the new feature consult the official documentation.
