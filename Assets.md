Jasy has built-in support for asset managment. Assets is the name we use for all files of these groups:

* Images and Animations
* Sounds and Music
* Fonts
* Style Sheets
* Templates
* Data Structures
* Configurations

In Jasy we think of assets as objects being required by JavaScript classes. There is no asset depdency handling included (yet), but the approach is more regarding making JavaScript classes depend on specific assets. This makes a lot of sense in e.g. these cases:

* UI components with requirements to style sheets and images
* Themes with requirements to font files
* Main controller with requirements to configuration files
* View instances with requirements to markup templates files

This has another major benefit: Jasy only includes assets for meta data and during deployment which are actually used by a JavaScript class in the dependency tree. Including a new class also adds its assets to the list, while removing it, would also remove the assets uniquely used by that exact class. This keeps deployment and meta data inside each app minimal.

### Defining Asset Requirements

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