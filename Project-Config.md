# Project Configurations

## Basic Configuration

Each project must have a configuration. Typically this is stored in the `jasyproject.json`. This file defines the name of the project and optionally a few other things like other required projects, supported fields (to pass data from build to the client), etc.

A simple `jasyproject.json` might look like:

```json
{
  "name" : "projectname"
}
```

Pretty simple. The `projectname` needs to be unique in all the projects you are using (otherwise the previous project with that name is overridden). The name automatically defaults to the project's folder name. If that okay it's even possible to just leave this information out:

```json
{}
```


## Supported Keys

There is a number of top-level keys which are supported:

* **name**: The unique name of the project (defaults to the basename of the project folder)
* **package**: By default the package is identical to the name of the project. See also: [[Naming]]
* **requires**: List of folders or repository URLs which are required. 
* **fields**: Client side configuration values / build permutations. See also: [[Fields]]
* **content**: Manual definition of files with custom naming (if Jasy conventions could not be applied)


## Name vs. Package

The `name` is the unique project identifier while the `package` is the top level package prepended to all names (classes, assets) indexed during project set up. 

There is a default value for `name` which is identical to the local folder name (e.g. name of cloned repository folder) if not configured otherwise. The `package` defaults to `name` if not configured otherwise.


## Configure Requirements

Requirements can work with simple folder structures inside or outside your project's root folder. For accessing the content in the so-called source version it is typically easier to place dependencies inside the project's folder (or at least in a common web accessible folder).

Requirements can be either defined as folder references or as repository references (currently limited to Git). These types can be mixed in any order:

```json
{
  ...
  "requires" : 
  {
    "external/core",
    "https://github.com/documentcloud/underscore",
    "git://github.com/dperini/nwevents.git"
  }
  ...
}
```


### Checking out a specific version

For repository it's also possible to define the version one wants to checkout:


```json
{
  ...
  "requires" : 
  {
    "external/core",
    {
      "source" : "https://github.com/documentcloud/underscore",
      "version" : "1.3.1"
    },
    "git://github.com/dperini/nwevents.git"
  }
  ...
}
```

For supporting the version info we introduce another inner object to configure `source` and `version` separately.


### External configuration for 3rd code

It's possible to inline the configuration of 3rd party code into the main project's configuration. This way it's possible to use non-Jasy ready 3rd party code without hassle. Once configured all Jasy benefits like automatic dependency tracking works fine. To add manual configuration for underscore the code needs to be changed like this:

```json
{
  ...
  "requires" : 
  {
    "external/core",
    {
      "source" : "https://github.com/documentcloud/underscore",
      "version" : "1.3.1",
      "config" :
      {
        "content" :
        {
          "_" : [
            "underscore.js"
          ]
        }
      }
    },
    "git://github.com/dperini/nwevents.git"
  }
}
```

Using this additional configuration we can define that there is a class `_` in the Underscore library which is defined by the file `underscore.js` in the top-level.


### Shallow Clones

One word of warning. Jasy is cloning repositories using so-called shallow clones. This is the best way to deal with external repositories without adding too much bloat to the disk and reduce download size. However this has two side effects one should be aware of:

A shallow repository has a number of limitations. You cannot clone or fetch from it, nor push from nor into it, but is adequate if you are only interested in the recent history or a specific version of a large project with a long history, and would want to send in fixes as patches.

*Warning*: Every execution of `jasy` resets these clones to their origin state. If you make changes inside be sure to copy them over to protect them. Otherwise they will be lost after executing `jasy` the next time.


### Requirements vs. Git Submodules

You can use Git Submodules and requirements (using folder directives) together but typically it's easier to omit submodules all together and just use requirements. You have the benefit of easy recursive dependencies. You have always up-to-date builds using the `jasy` command without the hassle of keeping submodules (recursively) up-to-date manually. Updating these requirements will also automatically update them for all other developers on the next time they execute `jasy`. No need to inform each other to call `git submodule update --recursively` after changing requirements.


### Recursive Dependencies

Jasy supports recursive dependencies. If you have a project `A` which requires a project `B` and project `B` which requires project `C` than during the initialization of project `A`, project `C` will be added/cloned as well. 


### Overriding Projects

Projects can be overridden by their unique identifier. Let's say you are using a company wide JavaScript library which has a requirement to a 3rd party library called `awesome`. Under some condition it might be required to use a more up-to-date version of that exact library. By adding it to the local `requires` section one can override the requirement of the company wide library and make use of that version instead. Just keep in mind that this works with minor changes only. This version of `awesome` still needs to be compatible to the version required by your company's shared library.


## Setup Fields

Have a look at the separate documentation: [[Fields]].


## Manual Layout

