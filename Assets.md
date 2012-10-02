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

This has another major benefit: Jasy only includes assets for meta data and during deployment which are actually used by a JavaScript class in the dependency tree.

