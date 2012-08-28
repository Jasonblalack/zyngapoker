Creating new Jasy-enabled projects is easy. In involves executing a single line command (using the built-in `create` command` in the Console:

```bash
$ jasy create --name APPNAME --origin ORIGIN --skeleton SKELETON
```

In this line replace all upper case words with the actual values you would like to use:

```bash
$ jasy create --name myapp --origin git://github.com/zynga/core.git --skeleton application
```

All parameters to `jasy create` are optional:

- `name` defines the name of the project. It must consists of lowercase characters only. When `name` is missing Jasy automatically uses `myproject`
- `origin` defines the source project which offers skeletons. This could be a local folder or a Git URL. If it is missing Jasy checks whether the current working directory is actually a Jasy project it can use.
- `skeleton` defines the name of the skeleton being used. It defaults to the first sub folder inside `skeleton` of the `origin` if it is not defined by the user.