# Project Configurations

Each project must have a configuration. Typically this is stored in the `jasyproject.json`. This file defines the name of the project and optionally a few other things like other required projects, supported fields (to pass data from build to the client), etc.

A simple `jasyproject.json` might look like:

```js
{
  "name" : "projectname"
}
```

Pretty simple. The `projectname` needs to be unique in all the projects you are using (otherwise the previous project with that name is overridden). The name automatically defaults to the project's folder name. If that okay it's even possible to just leave this information out:

```js
{}
```

There is a number of top-level keys which are supported:

* **name**: The unique name of the project (defaults to the basename of the project folder)
* **package**: By default the package is identical to the name of the project. See also: [[Naming]]
* **requires**: List of folders or repository URLs which are required. 
* **fields**: Client side configuration values / build permutations. See also: [[Fields]]

A more complex `jasyproject.json` might look like this:

```js
{
  "name" : "notebook",
  "package" : "",
  "requires" : [
    "../core"
  ]
}
```
