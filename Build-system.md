# Build System

Tooling for JavaScript is not yet captured by a good collection of tools yet. **Jasy** is one approach to fix this situation. Without a good and generic build system it's pretty hard to deal with even semi-complex project structures. In basically every other programming environment (.NET, Java, Objective-C or even Flash) it's typically to use a build system. Still it seems that in JavaSCript people think to be able to life without such a system.

Tooling systems which are based on a [DSL](http://en.wikipedia.org/wiki/Domain-specific_language) are often quite limited regarding flexibility. Jasy should be the one and only tooling solution for your JavaScript you need. There should not be anything you can't implement in there. This is one of the reasonings why the build system is not configured using a DSL. Instead Jasy is used through a Python3-API. The project author writes a Python3-script using the Jasy-API but is also able to use all standard Python modules to fulfill the tasks. 

Jasy borrows the idea of using a full programming language instead of pure configuration files from build systems like [SCons](http://www.scons.org/). Instead of doing configuration via a limited configuration file, you just write code with all the flexibility of a modern scripting language.

## Requirements

Jasy requires a [Python 3](http://www.python.org/) installation. It does not need any external modules. Python 2 comes installed on a lot of machines by default at the moment. Even though Python 3 is out for a few years already (released in early 2009) it is still not installed by default on most machines. Luckily installation is pretty straightforward though. 

[Python 3.2](http://www.python.org/download/releases/) is the current version. It is not suggested to install any older 3.x version with Jasy. There are installers available for Windows and Mac. Both should work fine. For Linux users it is suggested to userse their included software manager to look out for Python 3.

After installing type `python3 -V` on the command line to verify that it is working. It should output something like `Python 3.2.1`.

## Projects

Jasy supports different kind of projects. A project is something like your very own application folder, but also any kind of library you make use of e.g. jQuery. 

In most cases the kind of project is detected automatically based on its folder structure. A project author is able to override the kind of project using the `kind` keyword in the manifest. Supported values are: 

* `full`: code in `source/class`, assets in `source/asset`, translations in `source/translation`.
* `basic`: code in `class`, assets in `asset`, translations in `translation`
* `classic`: code and assets in `src`
* `flat`: code and assets in top-level project folder

For your own application the `full` variant makes the most sense as you typically also generate a folder `build` or `dist` for your optimized and distribution ready application build. Libraries which are itself not a full-blown application are typically perfectly fine with being kind of `basic`, `classic` or `flat`.

A fully custom folder structure is currently not supported. Jasy throws an error when a project structure is not supported. All projects can also only use a subset of the folder structure e.g. just assets (icon sets).

Each project needs to contain `manifest.json` file in its top-level folder. If your project is part of a larger project you might want to place the `manifest.json` file into a sub-folder of your larger project and point to this folder in your build script.

JavaScript source code must have the extension `js` and export a single class. Don't put multiple class declarations into one file. Assets of arbitrary types are supported (Image size handling supported for `png`, `gif` and `jpeg` only). Translations must be in the [gettext](http://www.gnu.org/s/gettext/) `po` format. One language per file e.g. `de.po`.

## Names

Each file in a project needs to have a qualified name. A qualified name of any class or asset is automatically derived from the file name and location inside the project.

### Classes

For classes this should be identical to the name it defines/exports. Jasy requires that there is exactly one name declaration per file. (This might be feel like a strong limitation first but makes modularity and code typically easier to understand. The main argument before tooling to bundle these classes into one file was to reduce loading overhead. This is not relevant anymore with Jasy.)

In a project (kind: classic) called `notebook` a file placed in `src/view/Preferences.js` will be called `notebook.view.Preferences`. As you can see the `notebook`-part is injected into the fully qualified name automatically. If you want to disable this behavior, you can set up the `package` configuration in the project's manifest to something else. If it is called `noty` instead, the exported class name should be `noty.view.Preferences`. Jasy does not verify whether the developer is really are exporting that symbol: There is no name validation happening.

Public names exported from JavaScript code have to be unique across all projects. If one completely override a full class under its original name it makes no sense to include it at all, right? The dependency engine in the build system use these public names to analyse dependencies between files e.g. a class `notebook.controller.Main` (stored in `src/controller/Main.js`) might use the preferences dialog from above. The dependency and ordering is automatically detected - even through project borders.

Fuzzy names allows you to have class names which differ from file names. This is by no means thought as the ideal way to do, but is sometimes helpful when you don't own a specific project or a in the middle of a refactoring. If `fuzzy` is enabled in a project every class in the project needs to have a documentation comment with the JavaDoc-like `@name {package.ClassName}` e.g.

```js
/** 
 * @name {notebook.ui.Button} 
 */
```

As soon as fuzzy mode is enabled classes without such a comment are regarded as errors and prevent the build script from succeeding.

### Assets

Assets will be merged and do not behave like classes. Conflict resolution works in direction of adding projects to the session in your build script. In works on a file-by-file basis. Projects which are registered later win over projects registered earlier. This allows the application developer to override assets or translations from a library project inside an application. This is fine for using data from external repositories or company standards while still having the possibility to easily override single assets or translations.

Keep in mind that assets are typically "protected" by the auto-namespacing enabled through `package` having being configured to the `name` of the project. To override assets from another project you have to configure `package` in your `manifest.json` to an empty string.

In the default configuration these assets will automatically get assigned to the asset ID on the right hand side.

    source/asset/icon/save.png => "notebook/icon/save.png"

To use these assets pass the asset ID through `jasy.io.Asset.toUri(assetId) => fullUri`. In Jasy powered projects you don't have to deal (and shouldn't deal) with file system locations (or relative paths) anymore. Just use `jasy.io.Asset` together with asset indexing via the build tools.

To override icons from e.g. your companies icon pool, the behavior of this ID assignment changes. This means you have to have a sub folder which represents your application name as well as one representing the name of the other project e.g.:

    source/asset/notebook/icon/save.png => "notebook/icon/save.png"
    source/asset/common/logo.png => "common/logo.png"

The last asset file overrides the asset "logo.png" which is also available through a project called `common`. To make this work the application project have to be registered after the `common` project in your build script.

The default of injecting the project name automatically in the namespaces is a good default for most projects. If you have more complex scenarios where you have to override single assets from you can still achieve them by resetting `package` in your project's manifest to an empty string.

### Translations

Translations behave similar to assets, but there is no namespacing at all. All IDs used with the translation engine are regarded as top-level. This is because one normally prefer English sentences like `Save to...` in code instead of logical IDs for translation files. It makes not really a lot of sense to namespace sentences. 

You can still achieve something like this – if you really want to – using IDs instead of sentences and building them with namespaces in mind like `notebook.preferences.OptionTitle`. Just keep in mind that IDs don't easily allow you to use any placeholders and make it visible for translators where dynamic data is planned to be inserted e.g. `Copying file %1 of %2...` is easily translated to e.g. German `Kopiere Datei %1 von %2...`. Not so easy with a translation ID like `notebook.CopyProgress`.

Translation files are merged per language e.g. all `de.po` files are merged into one. As the translation system support language variants as well, you might also have a e.g. `de_AT.po` file. If your application figures out that the user comes from Austria you would select the `de_AT` locale. The merge happens on translation level first. On a second round the resolution `variant (de_AT) => language (de) => default (en)` happens and leads to a final lookup table for the given language. This behavior is basically identical to every other [gettext](http://www.gnu.org/s/gettext/) implementation.

## Manifest

Each project must have a `manifest.json`. This file defines the name of the project and optionally a few other things like supported fields and their configuration.

A simple manifest file looks like this:

```js
{
  "name" : "projectname"
}
```

Pretty simple. The `projectname` needs to be unique in all the projects you are using. At name automatically defaults to the project's folder name. If that okay it's even okay to just leave it nearly empty:

```js
{}
```

These are all the top-level keys which are supported:

* **name**: The unique name of the project (defaults to the basename of the project folder)
* **kind**: This defines the structure inside the project. Should work automatically in most cases. See "Projects" section above for details.
* **package**: By default the package is identical to the name of the project. See the "Naming" section above for details.
* **fuzzy**: Whether the naming of clases is strictly bound the filenames or not. Defaults to `false`. 
* **fields**: The fields which are used by the project. See section about fields and permutations for details.

A more complex `manifest.json` might look like this:

```js
{
  "name" : "projectname",
  "kind" : "full",
  "package" : "",
  "fuzzy" : true,
  "fields" : 
  {
    "debug" : 
    {
      "check" : "Boolean",
      "default" : false,
      "detect" : "jasy.detect.Param"
    }
  }
}
```

## Build Script

The most trivial example of a build scriot

```python
# Extend PYTHONPATH with 'lib'
import sys, os
sys.path.insert(0, "/path/to/jasy")

# Import JavaScript tooling
from jasy import *

@task
def simple():
    # Setup session
    session = Session()
    session.addProject(Project("."))

    # Collecting projects
    resolver = Resolver(session.getProjects())
    resolver.addClassName("notebook.Application")

    # Resolving classes
    classes = Sorter(resolver).getSortedClasses()

    # Compressing classes
    compressedCode = Combiner(classes).getCompressedCode()

    # Writing files
    writefile("build/simple.js", compressedCode)
  
# Execute Jasy
run()
```

## Fields

Fields allow for configuration flags in projects. They are shared by all projects e.g. an application using a library project also inherits all fields defined by this library. Conflicts result in an exception during build time. Be careful on how you name your fields. It is not uncommon to prefix library specific fields with the project name e.g. `richtext.table` instead of just `table`. There is nothing enforced here, though, as there might be also common useful names like `debug`.

Fields can be configured using these keys:

* `detect`: Defines a class name to query for the value. The class defined here is automatically injected into the build.
* `check`: The check to apply to the detected value for validation. Value could be either one of `"Boolean"`, `"String"`, `"Number"` or a list of possible values e.g. `["small", "medium", "large"]`
* `default`: Defines a default value. Used when validation fails or no value was detected.

Each field it is automatically tested via the `detect` class defined in the project's configuration and filled with a value. These detection classes could have a static field `VALUE` or a getter `get(field)` which should return the value of the given field. Using getter methods one can share a detection class for multiple fields (e.g. to return server data, parse the query string, etc.)

You can hard-configure a field in your build script using as well:

```python
session.setField("name", value)
```

In this case no detection happens. This is good to inject values from the outside e.g. the version number of your application.

## Environment

Inside your JavaScript you have access to the fields configured in your fields using the `Permutation` class.

```js
var appTitle = Permutation.getValue("app-title");

if (Permutation.isSet("debug")) {
  // debug code
}

if (Permutation.isSet("customer", "premium")) {
  // premium customer
} else if (Permutation.isSet("customer", "plus")) {
  // plus customer
} else {
  // free customer
}
```

## Permutations

Permutations build upon the field configuration. The idea is basically to combine separate values of different fields into a combination - a so called permutation. This permutation can be used to build a specific JavaScript file 
