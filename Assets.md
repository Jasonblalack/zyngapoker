Jasy has built-in support for asset managment. Assets is the name we use for all files of these groups:

* Images and Animations
* Sounds and Music
* Fonts
* Style Sheets
* Templates
* Data Structures
* Configurations

## Dependencies

In Jasy we think of assets as objects being required by JavaScript classes. There is no asset depedency handling included (yet), but the approach is more regarding making JavaScript classes depend on specific assets. This makes a lot of sense in e.g. these cases:

* UI components with requirements to style sheets and images
* Themes with requirements to font files
* Main controller with requirements to configuration files
* View instances with requirements to markup templates files

This has another major benefit: Jasy only includes assets for meta data and during deployment which are actually used by a JavaScript class in the dependency tree. Including a new class also adds its assets to the list, while removing it, would also remove the assets uniquely used by that exact class. This keeps deployment and meta data inside each app minimal.

## Defining Requirements

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

## Organization

Generally you can use pretty wide-including wildcards in some classes vs. exactly matching asset tags in separate files. For libraries it typically makes sense to be pretty explicit on which assets a single feature requires. Applications are typically easier and want to include all assets of that application. 

### Modularization

Keep in mind though that if you like to modulize your application later on e.g. post-pone loading of specific features it makes sense to include the assets required only by specific features in the JS files generated for these. To make this possible you have to use pretty specific asset tags in most files. At least there have to be one definition for each "entry point" e.g. a specific dialog inside your application, a level of your game, etc. An example for this approach:

* `mygame.level.Level1` => `#asset(mygame/level/level1)`
* `mygame.level.Level2` => `#asset(mygame/level/level2)`
* ...

or

* `mygame.dialog.Preferences` => `#asset(mygame/dialog/preferences)`
* `mygame.dialog.Accounts` => `#asset(mygame/dialog/accounts)`
* ...

