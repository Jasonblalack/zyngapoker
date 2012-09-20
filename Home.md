Jasy - Web Tooling Framework
============================

Jasy is a construction tool for web projects. Think of Jasy as a replacement for the classic [make](http://www.gnu.org/software/make/) utility heavily inspired by modern tools like [SCons](http://www.scons.org/) (Used by V8, MongoDB, ...), [Waf](http://code.google.com/p/waf/) (Used by Samba, NodeJS, ...) and [Maven](http://maven.apache.org/). Jasy is based on the learnings through creating the "generator" for the [qooxdoo](http://qooxdoo.org) JavaScript framework.

* [[FAQ]]
* [[Features]]

## Installation

We have different tutorials for installing Jasy specific to the operating system you are using:

* [[Windows]]
* [[Mac OS X]]
* [[Linux]]

*Doctor*: There is a built-in task called "doctor" which can be used to easily verify an environment. Just call `jasy doctor` when installed or `bin/jasy doctor` in your repository clone.

*Python Experts*: If you already have a Python3 installation and want to install Jasy in parallel you could follow our [[Installation using VirtualEnv]] guide.

## Documentation

### Basics

* [[New Application]]: How to use skeletons for creating new applications
* [[Project Structure]]: Files and folders in your project
* [[Project Config]]: Configure your project settings
* [[Names]]: How files are named on file system and inside the application
* [[Build Script]]: Configure tasks for your application
* [[Web Server]]: Usage of the integrated web server

### Advanced

* [[Fields]]: Pass data from the build script to the application and permutate code for different output scenarios
* [[Image Sprites]]: Setup image sprites for your application
* [[Image Animations]]: Tooling for image based animations
* [[Develop Skeletons]]: How to create new skeletons
* [[Sharing Methods]]: How to share Python features with other projects
* [[Locales]]: How to use locale support in your projects
* [[Translations]]: How to translate your projects
* [[API Docs]]: How to document your code

## Migration

* [[Migration 0.5 to 0.6]]
* [[Migration 0.6 to 0.7]]
* [[Migration 0.7 to 0.8]]