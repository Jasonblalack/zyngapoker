Jasy has built-in support for asset managment. Assets is the name we use for all files of these groups:

* Images and Animations
* Sounds and Music
* Fonts
* Style Sheets
* Templates
* Data Structures
* Configurations

## Benefits

* The main advantage of the asset system is to bring ID based asset loading to the application code. So instead of referring to exact URLs all over your code or using a custom management/database you just use globally unique asset IDs and pass them to the methods offered by the *Core* library.
* The asset system supports passing specific meta data e.g. image dimensions, play duration, file sizes, etc. for assets to the client.
* Dependencies based on what the actually included JavaScript files require. Features which are excluded from the build (deployed version) also do not deliver the assets with them which are uniquely used by these features.


## Dependencies

In Jasy we think of assets as objects being required by JavaScript classes. There is no asset depedency handling included (yet), but the approach is more regarding making JavaScript classes depend on specific assets. This makes a lot of sense in e.g. these cases:

* UI components with requirements to style sheets and images
* Themes with requirements to font files
* Main controller with requirements to configuration files
* View instances with requirements to markup templates files

This has another major benefit: Jasy only includes assets for meta data and during deployment which are actually used by a JavaScript class in the dependency tree. Including a new class also adds its assets to the list, while removing it, would also remove the assets uniquely used by that exact class. This keeps deployment and meta data inside each app minimal.

### Defining Requirements

Assets have to be required by JavaScript classes to be included. As Jasy does not have any possibility to savely figure out which assets are used, the developer has to add hints to the JavaScript classes. This happens inside documentation comments using tags:

```javascript
/**
 * This is my class.
 *
 * #asset(my/css/main.css)
 */
core.Class("my.Class", {

});
```

The class `my.Class` uses an asset `my/main.css` (an asset from the same project called `css/main.css`). The `asset()` tag also support file system type wildcards e.g. `#asset(my/css/*.css)` is perfectly valid as well. These are the pattern placeholders supported:

- `*`: matches everything
- `?`: matches any single character
- `[seq]`: matches any character in seq
- `[!seq]`: matches any character not in seq

**Note:** Asset tags make use of unix style file system paths but work cross platform e.g. on Windows using backslash instead of slash.

These asset tags work as a list which is used to build a single regular expression against what all asset IDs are matched. You can easily combine multiple asset tags in one or multiple lines or even in different comments. This makes it possible to place the asset comment exactly at the position where the asset is needed/included/loaded. This is better for keeping code easily maintainable.

```javascript
/**
 * This is my class.
 */
core.Class("my.Class", {

  construct: function() {

    /** #asset(my/css/main.css) #asset(my/css/normalize.css) */
    core.io.Asset.load(["my/css/main.css", "my/css/normalize.css"]);

  }

});
```

### Organization

Generally you can use pretty wide-including wildcards in some classes vs. exactly matching asset tags in separate files. For libraries it typically makes sense to be pretty explicit on which assets a single feature requires. Applications are typically easier and want to include all assets of that application. 

#### Modularization

Keep in mind though that if you like to modulize your application later on e.g. post-pone loading of specific features it makes sense to include the assets required only by specific features in the JS files generated for these. To make this possible you have to use pretty specific asset tags in most files. At least there have to be one definition for each "entry point" e.g. a specific dialog inside your application, a level of your game, etc. An example for this approach:

* `mygame.level.Level1` => `#asset(mygame/level/level1/*)`
* `mygame.level.Level2` => `#asset(mygame/level/level2/*)`
* ...

or

* `myapp.dialog.Preferences` => `#asset(myapp/dialog/preferences/*)`
* `myapp.dialog.Accounts` => `#asset(myapp/dialog/accounts/*)`
* ...


## Meta Data

Jasy automatically adds meta data to all assets it finds and manages. This makes some things on the client side easier like rendering or preloading. Each meta data entry contains the following information:

* File Type + File Type Specific Data
* URL Profile + URL Profile Specific Data

Jasy groups files into different file type categories. This means that the type information is no exact type (mime type), but more like a categorization. Currently these types are supported:

* Image e.g. `.png`
* Audio e.g. `.mp3`
* Video e.g. `.mp4`
* Font e.g. `.ttf`
* Text e.g. `.html`
* Binary e.g. `.swf`
* Other e.g. `.gtd`

All file types might add specific data to the meta data which is then exported to the client as well. At the moment this only is being used for images for the `width` and `height` or each image.



## Profiles

Profiles are used to define groups of assets and their origin related to the application. There are two built-in profiles called `source` and `build` which are actually typically used inside the same named tasks in Jasy projects.

* `source`: Load assets from there original location without copying them around
* `build`: Load assets from merged self-contained asset folder (assets are copied over) in build directory

While it's possible for different assets to use different profiles, each asset can only be assigned to exactly one profile. 

**Note:** Be sure to assign profiles to assets before exporting them into JavaScript data. Otherwise the client has no information how to resolve URLs and to load them.

### Built-in Profiles

* Enable the `source` asset profile using `assetManager.addSourceProfile()`. 
* Enable the `build` asset profile using `assetManager.addBuildProfile()`.

By default this not only adds the profile to the list of profiles, but also marks all assets which are not yet assigned to a profile to use the `source`/`build` profile. 

* To override existing asset profiles add the option `override=True`.
* To prepend a specific URL prefix before all relative URLs add the option `urlPrefix="http://localhost:8081/myapp/"`.

### Custom Profile

As you might have seen the profile system is pretty scalable and also offers support for far more complex hosting situations regarding your assets e.g. having different domains for distributed loading, using checksums for better caching/invalidation, etc. Custom profile make all these things possible, easily.

**Note:** Each profile has a name and a unique ID. This unique ID is just a incremented number and is assigned to asset items later on. This means that the length of the name has no negative impact on the size of the exported meta data.

Adding a new profile called `cdn`:

```python
assetManager.addProfile("cdn", root="http://akamai.mycompany.com/myapp/")
```

Profiles can be added together with a list of assets to update. This is useful for directly updating a specific assets from e.g. data delivered by your company central asset management system:

```python
items = {
  "myapp/main.css" : { "c" : "3043fac03929d030" }
  "myapp/splash.png" : { "c" : "abe4729f9230d0a0" }
}

assetManager.addProfile("cdn", root="http://akamai.mycompany.com/myapp/", items=items)
```

This adds a new profile called "cdn" and assigns that profile to all items given. The `c` field in this case is used for some kind of checksum. 


## Meta Data Structure

Jasy stores meta data in a compact way to reduce the overhead of data which is needed for transportation to the client. Each asset has a hash map of data to be easy and fast to access while still being compact. This is the list of built-in keys used by Jasy internal methods and functionality:

* `t` used to store the asset type.
* `d` used to store asset type related data e.g. image sizes.
* `p` used to assign the profile to assets. 
* `u` used to keep the relative asset URI for the `source` profile. 

Don't use any of these for your custom asset data. For some inspiration here is a list of often used choices for custom asset data and their fields:

* `c` to store the checksum of a file e.g. *SHA1*
* `l` to store the length of a file (e.g. play duration, encoding, ...)
* `m` to store the modification date


## Delegates

Delegates are used to define custom URL resolvers based on additional data added to the profiles. There is some simple URL resolving included in *Core* which basically just prepends the asset ID after the configured profile root. For other use cases one is able to define new delegate functions assigned to a specific asset profile:

```js
jasy.Asset.registerDelegate("cdn", function(profileData, assetId, assetEntry) { return URL; })
```

As you can see this is JavaScript code for the client side. This is great for better flexibility e.g. random CDN hosts, account specific URL prefixes, etc. Each profile can define a separate delegate. If no delegate is used it falls back to the `source`/`build` patterns automatically. 

Notes for the delegate method:

* It has to return a fully qualified URL.
* It also needs to prepend the `profileData.root` when this is needed. This is not done automatically with a custom asset profile delegate.
* It has full access to all data inside the asset entry e.g. a custom defined checksum in `c` etc.



## Deployment

Assets can be easily deployed using either the `AssetManager` or `OutputManager`. The last one is preferred as it automatically resolves classes.

```python
outputManager.deployAssets(["myapp.Main"])
```

Alternative using `AssetManager` directly:

```python
assetManager.deploy(Resolver(session).addClassName("myapp.Main").getIncludedClasses())
```

Both methods have a optional parameter `assetFolder` to define the name of the asset folder (defaults to `"asset"`).

This copies assets from different projects using their asset ID instead of the original file system location. In consequence this means that manually defined assets (using a custom content section) get the name of the ID defined in the configuration instead of the original filesystem path. All asset IDs are generated with the prefix of the "package" of the project they rely to. For most application/libraries this is identical to the "name" of the project. 

* `myapp/source/asset/main.css` => `myapp/build/asset/myapp/main.css`
* `myapp/external/normalize/normalize.css` => `myapp/build/asset/normalize/normalize.css`
* `myapp/external/glyphicons/reload.png` => `myapp/build/asset/glyphicons/reload.png`
* ...

As you can see all assets from the different projects are merged under `myapp/build/asset` in folders identical to project package (which is identical to the project name in most cases).


## Resolving URLs

The


## Loading

### Loader Handling

### Batch Loading

### Section Loading