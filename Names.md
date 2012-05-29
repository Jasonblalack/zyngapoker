# Names

Each file in a project needs to have a qualified name. A qualified name of any class or asset is automatically derived from the file name and location inside the project and the project's configuration.


## Classes

For classes the name should reflect the name it defines/exports. Jasy requires that there is exactly one (public) name declaration per file. (This might be feel like a strong limitation first but makes modularity and code typically easier to understand. The main argument before tooling to bundle these different classes into one file was to reduce loading overhead. This is not relevant anymore with Jasy.)

In a project called `notebook` a file placed in `src/view/Preferences.js` will be called `notebook.view.Preferences`. As you can see the `notebook`-part is prepended into the fully qualified name automatically. 

If you want to disable this behavior, you can set up the `package` configuration in the project's configuration to something else. If it is called `noty` instead, the exported class name should be `noty.view.Preferences`. There is no name validation happening right now. Jasy does not verify whether the developer is really exporting that symbol. Mainly that's because it would be pretty complicated to do in such a dynamic language.

Public names exported from JavaScript code have to be unique across all projects. If one completely override a full class under its original name it makes no sense to include it at all, right? The dependency engine in the build system use these public names to analyse dependencies between files e.g. a class `notebook.controller.Main` (stored in `src/controller/Main.js`) might use the preferences dialog from above. The dependency and ordering is automatically detected - even through project borders.


## Assets

Assets will be merged and do not behave like classes. Conflict resolution works in direction of how projects are added to the session (typically via automatic project dependency handling). This conflict handling works on a file by file basis. Projects which are added later win over projects added earlier. This allows the application developer to override assets (or translations) from a library project inside an application. This is fine for using data from external repositories or company standards while still having the possibility to easily override single assets.

Keep in mind that assets are typically "protected" by the auto-namespacing enabled through `package` having being configured to the `name` of the project. In the default configuration these assets will automatically get assigned to the asset ID on the right hand side.

    source/asset/icon/save.png => "notebook/icon/save.png"

To use these assets pass the asset ID through `core.io.Asset.toUri("assetId") => fullUri`. In Jasy powered projects you don't have to deal (and shouldn't deal) with file system locations (or relative paths) anymore.

To override icons from e.g. your companies icon pool, the behavior of this ID assignment changes. This means you have to have a sub folder which represents your application name as well as one representing the name of the other project e.g.:

    source/asset/notebook/icon/save.png => "notebook/icon/save.png"
    source/asset/common/logo.png => "common/logo.png"

The last asset file overrides the asset "common/logo.png" which is also available through a project called `common`. This simply works then registering "common" as a requirement to the "notebook" application.

The default of injecting the project name automatically in the namespaces is a good default for most projects. If you have more complex scenarios where you have to override single assets you can still achieve them by resetting `package` in your `jasyproject.json` to an empty string.


## Translations

Translations behave similar to assets, but there is no namespacing at all. All IDs used with the translation engine are regarded as top-level. This is because one normally prefer English sentences like `Save to...` in code instead of logical IDs for translation files. It makes not really a lot of sense to namespace plain sentences. 

You can still achieve something like this – if you really want to – using IDs instead of sentences and building them with namespaces in mind like `notebook.preferences.OptionTitle`. Just keep in mind that IDs don't easily allow you to use any placeholders and make it visible for translators where dynamic data is planned to be inserted e.g. `Copying file %1 of %2...` is easily translated to e.g. German `Kopiere Datei %1 von %2...`. Not so easy with a translation ID like `notebook.CopyProgress`.

Translation files are merged per language e.g. all `de.po` files are merged into one. As the translation system support language variants as well, you might also have a e.g. `de_AT.po` file. If your application figures out that the user comes from Austria you would select the `de_AT` locale. The merge happens on translation level first. On a second round the resolution `variant (de_AT) => language (de) => default (en) => code (en)` happens and leads to a final lookup table for the given language. This behavior is basically identical to every other [gettext](http://www.gnu.org/s/gettext/) implementation. Please note that this means that text from a library project which is placed in a file `de_DE.po` is of higher priority than the same text in your location project's `de.po` file.