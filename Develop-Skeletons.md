Creating skeletons is pretty easy. Basically it's just a project where some names are replaced with variables and two optional files in it. A skeleton is copied 1:1 to the destination folder, then patches are being applied and a custom script is being executed.

## Placeholders

Every skeleton can use placeholders to making it possible to replace sections in files with new dynamic content which is defined by the application being created. Placeholders should be formatted as `$${name}`. This somewhat complex placeholder is used to prevent conflicts with existing template formats so that the skeleton support also works across files like Smarty or Handlebars templates.

Jasy automatically figures out the files which it is able to patch. It intelligently omits binary files as fast as it is possible. It prefers UTF-8 encoding but should also work with other encodings as long as the actual placeholder name is ASCII or UTF-8 compatible.

## Automatic values

There are a few pre-defined fields available for your skeleton:

* `name`: Name of the project
* `jasy.version`: Jasy version being used to create the skeleton
* `origin.name`: Name of the project which offered the skeleton
* `origin.version`: Version of the project offered the skeleton
* `origin.skeleton`: Name of the skeleton which was used

## Skeleton Specific Files

* **Questions**: `jasycreate.yaml/json`: Questions to ask the user. Support pre-filling fields via command line as well.
* **Scripting**: `jasycreate.py`: Custom scripting executed after files have been copied and questions from YAML/JSON files have been answered.

### Questions

The simplest way to add more fields to be stored in the skeleton configuration or be replaced in your skeletons is to add them to a configuration file called either `jasycreate.yaml` or `jasycreate.json`. A typical file might look like:

```yaml
- name: user.name
  question: What's your name
  accept: String
- name: user.age
  question: How old are you
  accept: Integer
- name: pi
  question: What's PI
  default: 3.14
  required: false
  accept: Float
- name: incr
  question: Increment from 1..3
  accept: List
```

### Supported types

* `primitive`: either strings, booleans or numbers
* `integer`/`int`: non floating point numbers
* `float`: requires floating point numbers
* `number`/`num`: any number
* `string`/`str`: any string
* `boolean`/bool`: needs to be Python-ish `True` or `False`
* `dict`/`map`: key/value data structure
* `array`/`list`: can be defined as tuples as well aka 0,2 without braces

### Scripting

A typical `jasycreate.py` might look like that:

```python
# Custom questions
config.set("custom", [1,2,3])
config.ask("What's your name", "user.name", "String")
config.ask("How old are you", "user.age", "Integer")
config.ask("What's PI", "pi", "Float", default=3.14, required=False)
config.ask("Increment from 1..3", "incr", "List")

# Moving file
file.mv("placeholder.xcode", config.get("name") + ".xcode")
```

config.ask() is pretty much identical to what you can achieve via the question files. Other than that you are able to hardly set values to the configuration and interact with it. So you could even ask different questions based on previous input etc.

#### Configuration API

* `config.set(name, value, accept?, parse?)`: Sets the given field to the given value. Supports optional type checking and value parsing.
* `config.has(name)`: Whether the given field is set
* `config.get(name, default?)`: Returns the value of the given field or `default` when it is not set
* `config.ask(question, name, accept?, required?, default?, force?, parse?)`: Asks the user the given question if not value was set yet (overwrite via `force=True`)
* `config.export()`: Returns a flat representation of all data
* `config.write(fileName, indent?, encoding?)`: Writes the data to a config file. Supports JSON and YAML. File format is auto chosen by the file extension.
* `config.readQuestions(fileName)`: Manually read question file 
* `config.executeScript(fileName)`: Execute another script like the current one

#### File API

* `file.cp(src, dst)`: Copies the given file
* `file.cpdir(src, dst)`: Copies the given directory
* `file.exists(name)`: Returns whether the given file or folder exists
* `file.mkdir(name)`: Recursively creates the given directory
* `file.mv(src, dst)`: Renames/moves files or directories
* `file.rm(name)`: Deletes the given file
* `file.rmdir(name)`: Deletes the given directory

## Command Line Arguments

The user is able to pre-fill values asked for in the question files or from scripting via the command line just by adding `--name value` to the command line of `jasy create`. This is useful for testing skeletons automatically e.g. via Travis.ci.

* *Note*: Command line values are not checked by the types defined in question files or scripts right now as they are stored before this data is being made available.
* *Note*: Every additional command line argument is added to the configuration and therefore also patched into files when the fields are being used anywhere. This means that typos on the command line are also not detected right now.