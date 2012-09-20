# Project Structure

## Projects

Jasy supports different kind of projects. A project is something like your very own application folder, but also any kind of library you make use of e.g. jQuery. In most cases the kind of project is detected automatically based on its folder structure. A project author is able to define a custom structure using the "content" section inside the configuration files.


## File Layout

JavaScript source code must have the extension `js` and export a single class per file (Single exported symbol per file - Don't put multiple declarations into one file). Assets of arbitrary types are supported (Image size handling supported for `png`, `gif` and `jpeg` only). Translations must be written in Gettext `po` format. One language per file e.g. `fr.po`.


## Configuration Files

### *Project Configuration* - Information about the project
  
* File: `jasyproject.json`/`jasyproject.yaml`
* Occurrences: one
* Location: Project root folder
* Docs: [[Project Config]]

### *Task Configuration* - Configure the build targets
  
* File: `jasyscript.json`/`jasyscript.yaml`
* Occurrences: one
* Location: Project root folder
* Docs: [[Build Script]]

### *Sprites* - Setup image sprites: 

* File: `jasysprite.json`/`jasysprite.yaml` + `jasysprite_xxx.png`
* Occurrences: few
* Location: Inside asset folders
* Docs: [[Image Sprites]]

### *Animations* - Setup frame based image animations 

* File: `jasyanimation.json`/`jasyanimation.yaml`
* Occurrences: few
* Location: Inside asset folders
* Docs: [[Image Animations]]


## Scripting Files

### *Tasks* - Define build targets

* File: `jasyscript.py`
* Occurrences: one
* Location: Project root folder
* Docs: [[Build Script]]

### *Library* - Offer shared methods for other projects

* File: `jasylibrary.py`
* Occurrences: one
* Location: Project root folder
* Docs: [[Sharing Methods]]



## Internal Files

### *Cache* - Cache results

* File: `./jasy/cache*`
* Occurrences: one
* Location: Root folder

### *Web Server Lock* - Local web server

* File: `.jasy/server-xxx` (xxx = Port being used)
* Occurrences: few
* Location: Root folder
* Docs: [[Web Server]]

### *Web Mirror Cache* - Included mirroring functionality of web server

* File: `.jasy/mirror-xxx` (xxx = Route being mirrored)
* Occurrences: few
* Location: Root folder
* Docs: [[Web Server]]