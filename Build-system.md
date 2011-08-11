### Build System

Tooling for JavaScript is not yet captured by a good set of tools yet. Jasy is one approach to fix this situation. Without a good and generic build system it's pretty hard to deal with even semi-complex project structures. In basically every other programming environment (.NET, Java, Objective-C or even Flash) it's typically to use a build system. Still it seems that in JavaSCript people think to be able to life without such a system.

Tooling systems which are based on a [DSL](http://en.wikipedia.org/wiki/Domain-specific_language) are often quite limited regarding flexibility. Jasy should be the one and only tooling solution for your JavaScript you need. There should not be anything you can't implement in there. This is one of the reasonings why the build system is not configured using a DSL. Instead Jasy is used through a Python3-API. The project author writes a Python3-script using the Jasy-API but is also able to use all standard Python modules to fulfill the tasks. 

Jasy is to some point comparable to the ideas also implemented in build systems like [SCons](http://www.scons.org/).

## Requirements

Jasy requires a [Python 3](http://www.python.org/) installation. It does not need any external modules and any other installations. Python 2 comes installed on a lot of machines by default at the moment. Even though Python 3 is out for a few years already (released early 2009) it is still not installed by default on a lot of machines. Luckily installation is pretty straightforward though. 

[Python 3.2](http://www.python.org/download/releases/) is the current version. It is not suggested to install any older 3.x version with Jasy. There are installers available for Windows and Mac. Both should work fine. For Linux users it is suggested to userse their included software manager to look out for Python 3.

After installing type `python3 -V` on the command line to verify that it is working. It should output something like `Python 3.2.1`.

## Key Concepts

Before beginning writing your first build script it might makes sense to learn about a few key concepts of Jasy. You don't need to understand everything in detail yet.

* **Tasks**: A task is a method which is executable from the command line using e.g. `generate.py taskname`.
* **Session**: Every time you are dealing with processing code from projects you create a session instance.
* **Projects**: A project is any folder with JavaScript files, assets and/or translations. The main project is the one which contains the currently executed build script. Each project needs to have a `manifest.json` file to define its public name and configuration.
* **Fields**: A build-time configurable setting which is used by the code of a project which defines it. The build script of the main project is able to enable permutations for all known fields and can override any defaults from the defining project.
* **Permutations**: A permutation is a combination of values for different fields e.g. debug=off and engine=webkit is one possible permutation. Every new field which is enabled as a permutation multiplies the number of created permutations.
* **Locales**: Jasy includes full-blown support for locales as defined by unicode.org and the CLDR archive. It contains informations about number and date formats, country names, etc.
* **Translations**: This is based on gettext-files. The main differentiator from a lot of other implementations is that Jasy can inline-replace translations requests during building a specific permutation.

## Basic Manifest

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

## Project Kinds

Jasy supports different project kinds. In most cases the type is detected automatically based on the folder structure of the project. You can override the kind of project using the `kind` keyword in the manifest. Supported values are: `full`(code in folder `source/class`), `basic` (code in folder `class`), `classic` (code in folder `src`) and `flat` (code in top-level folder).

## Package Handling

Typically each project should define exactly one top-level namespace. The top level namespace is automatically set to the name of the project.

## Build Script







