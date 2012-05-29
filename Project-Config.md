# Project Configurations

Each project must have a `jasyproject.json`. This file defines the name of the project and optionally a few other things like other required projects, supported fields (pass data from build to the client), etc.

A simple `jasyproject.json` file looks like this:

```js
{
  "name" : "projectname"
}
```

Pretty simple. The `projectname` needs to be unique in all the projects you are using. At name automatically defaults to the project's folder name. If that okay it's even possible to just leave this info out and make the JSON file nearly empty:

```js
{}
```

These are all the top-level keys which are supported:

* **name**: The unique name of the project (defaults to the basename of the project folder)
* **package**: By default the package is identical to the name of the project. See the "Naming" section above for details.
* **requires**: List of folders or repository URLs which are required. 
* **fields**: The fields which are used by the project. See section about fields and permutations for details.

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
