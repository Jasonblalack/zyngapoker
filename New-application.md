Creating new Jasy-enabled projects is easy. In involves executing a single line command (using the built-in `create` command` in the Console:

```bash
$ jasy create --name APPNAME --origin ORIGIN --skeleton SKELETON
```

In this line replace all upper case words with the actual values you would like to use:

```bash
$ jasy create --name myapp --origin git://github.com/zynga/core.git --skeleton application
```

All parameters to `jasy create` are optional:

- **Name**: `name` defines the name of the project. It must consists of lowercase characters only. When `name` is missing Jasy automatically uses `myproject`
- **Origin**: `origin` defines the source project which offers skeletons. This could be a local folder or a Git URL. If it is missing Jasy checks whether the current working directory is actually a Jasy project it can use.
- **Skeleton**: `skeleton` defines the name of the skeleton being used. It defaults to the first sub folder inside `skeleton` of the `origin` if it is not defined by the user.
- **Version**: `version` defines a version of the origin project to clone. This is only relevant when auto-cloning the origin from Git. This could be a tag name or branch name like "0.8" for a specific release or "refactorui" for a branch name. It automatically figures out whether the given value is a version in most cases and falls back to a branch name is other scenarios. If auto handling fails just use the absolute Git identifier like "refs/heads/refactorui" or "refs/tags/0.8".
- **Destination**: `destination` defines the output directory. Normally the output directory is just a folder in the current working directory named like `name`. This allows overriding this mechanics.

The scaffolding offers three major features during creating new application from skeletons:

* **Patching**: Replaces `$${varname}` placeholders with the actual values inside all files
* **Configuration**: Stores a configuration `jasyscript.yaml` based on questions/flags defined in the skeleton (`jasycreate.yaml/json`). 
* **Scripting**: Acustom `jasycreate.py` script to further ask questions of modify the created application further (e.g. renaming folders, loading remote data, etc.)

The process during application creation follows this order:

1. Finding skeleton in origin (eventually after automatic cloning)
2. Copying a files from skeleton
3. Patches all files with available data
4. Ask questions defined by `jasycreate.yaml/json`
5. Ask questions and do scripting implemented in `jasycreate.py`

*Note*: The `jasycreate.*` files are removed from the final application during creating the application. These are not relevant for the execution of the application nor any `jasy` commands.