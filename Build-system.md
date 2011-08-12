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

## Names

Each file in a project needs to have a qualified name. For classes or modules this is typically identical to the name of the public JavaScript object/function it defines/exports. A qualified name of any class or asset is automatically derived from the file name and location inside the project. Jasy currently only supports one name declaration per file. A class per file is required to make the dependency engine works well.

Let's start with a short example: In a project (kind: classic) called `notebook` a file placed in `src/view/Preferences.js` will be called `notebook.view.Preferences`. As you can see the `notebook`-part is injected into the fully qualified name automatically. If you want to disable this behavior, you can set up the `package` configuration in the project's manifest to something else. If it is called `noty` instead, the exported class name should be `noty.view.Preferences`. Jasy does not very whether you are exporting this global symbol: There is no name validation happening at the moment.

Public names exported from JavaScript code have to be unique across all projects. If you completely override a full class under its original name it makes no sense to include it at all, right?

### Assets

Something different happens for assets. These will be merged. Conflict resolution works in direction of adding projects to the session in your build script. Projects which are registered later win over projects registered earlier. This allows to override assets or translations from a library project you use in your application project. This is fine for using data from external repositories or company standards and still having the possibility to easily override single assets or translations.

Keep in mind that assets are typically "protected" by the auto-namespacing enabled through `package` having being configured to the `name` of the project. To override assets from another project you have to configure `package` in your `manifest.json` to an empty string.

In the default configuration these assets will automatically get assigned to the asset ID on the right hand side.

    source/asset/icon/save.png => "notebook/icon/save.png"
    source/asset/icon/open.png => "notebook/icon/open.png"

You use these assets by passing this asset ID through `jasy.io.Asset.toUri(assetId) => fullUrl`. In Jasy powered projects you don't have to deal with file system locations anymore when using `jasy.io.Asset` together with asset indexing. 

To override icons from e.g. your companies icon pool, the behavior of this ID assignment changes. This means you have to have a sub folder which represents your application name as well as one representing the name of the other project e.g.:

    source/asset/notebook/icon/save.png => "notebook/icon/save.png"
    source/asset/notebook/icon/open.png => "notebook/icon/open.png"
    source/asset/common/logo.png => "common/logo.png"

The last asset file overrides the asset "logo.png" which is also available through a project called `common`. To make this work the application project have to be registered after the `common` project in your build script.

The default of injecting the project name automatically in the namespaces is a good default for most projects. If you have more complex scenarios where you have to override single assets from you can still achieve them by resetting `package` in your project's manifest to an empty string.

### Translations

Translations behave similar to assets. But there is no namespacing at all. All IDs used with the translation engine are regarded as top-level. This is because one normally prefer English sentences like `Save to...` in code instead of logical IDs for translation files. It makes not really a lot of sense to namespace sentences. You can still achieve something like this – if you really want to – using IDs instead of sentences and building them with namespaces in mind like `notebook.preferences.OptionTitle`. Just keep in mind that IDs don't easily allow you to place any placeholders and make it visible for translators where dynamic data is inserted e.g. `Copying file %1 of %2...` is easily translated to e.g. German `Kopiere Datei %1 von %2...`. Not so easy with a translation ID like `notebook.CopyProgress`.

## Manifest

Each project must have a `manifest.json`. This file defines the name of the project and optionally a few other things like supported fields and their configuration.

A simple manifest file looks like this:

    {
      name : "projectname"
    }

The `projectname` needs to be unique in all the projects you are using. 

These are all the top-level keys which are supported:

* **name**: The unique name of the project (defaults to the basename of the project folder)
* **kind**: This defines the structure inside the project. For alot of typical structures this just works without any configuration. For more information take a look at the section "Project Kinds".
* **package**: By default the package is identical to the name of the project. This means that if your project is called `my`, Jasy thinks that every class or asset in this project should be identified via the `my.`-string in front of it e.g. `my.ui.Button`, `my.io.Ajax`, etc. If the project name differs from the package you use, or all files are regarded as being non-sandboxed inside a package, you are able to override the package via this configuration flag.
* **fuzzy**: Whether the naming of clases is strictly bound the filenames or not. Defaults to `false`. If configured with `true` every class in the project needs to have a documentation comment with the JavaDoc-like `@name {package.ClassName}`. Such a documentation comment looks like `/** @name {my.ui.Button} */`. As soon as fuzzy mode is enabled classes without such a comment are regarded as errors.
* **fields**: Configures the fields which are defined and used by the project. Fields are automatically made available to every project using the project defining them. In the build script one can also activate fields to being permutated into specicialized output files/folders. 

## Build Script





