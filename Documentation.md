# Documentation

## Projects

Jasy supports different kind of projects. A project is something like your very own application folder, but also any kind of library you make use of e.g. jQuery. In most cases the kind of project is detected automatically based on its folder structure. A project author is able to define a custom structure using the "content" section inside the configuration files.


## Files

JavaScript source code must have the extension `js` and export a single classes. Don't put multiple class declarations into one file. Assets of arbitrary types are supported (Image size handling supported for `png`, `gif` and `jpeg` only). Translations must be written in Gettext `po` format. One language per file e.g. `fr.po`.


## Configuration

* *Config* - Configure the project (`jasyproject.json`): [[Project Config]]
* *Tasks* - Define build targets: (`jasyscript.py`): [[Build Script]]
* *Sprites* - Setup image sprites: (`jasysprite.json`): [[Image Sprites]]
* *Animations* - Setup frame based image animations: (`jasyanimation.json`): [[Image Animations]]

