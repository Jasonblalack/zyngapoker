# Documentation

## Projects

Jasy supports different kind of projects. A project is something like your very own application folder, but also any kind of library you make use of e.g. jQuery. In most cases the kind of project is detected automatically based on its folder structure. A project author is able to define a custom structure using the "content" section inside the configuration files.


## Files

JavaScript source code must have the extension `js` and export a single classes. Don't put multiple class declarations into one file. Assets of arbitrary types are supported (Image size handling supported for `png`, `gif` and `jpeg` only). Translations must be written in Gettext `po` format. One language per file e.g. `fr.po`.


## Configuration

### *Cache* - Cache results

* File: `jasycache.xxx`
* Occurrences: one
* Location: Root folder

### *Config* - Configure the project
  
* File: `jasyproject.json`
* Occurrences: one
* Location: Root folder
* Docs: [[Project Config]]

### *Tasks* - Define build targets

* File: `jasyscript.py`
* Occurrences: one
* Location: Root folder
* Docs: [[Build Script]]

### *Sprites* - Setup image sprites: 

* File: `jasysprite.json` + `jasysprite_xxx.png`
* Occurrences: many
* Location: Inside asset folder
* Docs: [[Image Sprites]]

### *Animations* - Setup frame based image animations 

* File: `jasyanimation.json`
* Occurrences: many
* Location: Inside asset folder
* Docs: [[Image Animations]]
