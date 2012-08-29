Creating skeletons is pretty easy. Basically it's just a project where some names are replaced with variables and two optional files in it. A skeleton is copied 1:1 to the destination folder, then patches are being applied and a custom script is being executed.

## Placeholders

Every project can use place

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



### Scripting

A typical `jasyscript.py` might look like that:

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

* `config.export()`:  
Returns a flat representation of all data
* `config.readQuestions(fileName)`:  
Manually read question file 
* `config.executeScript(fileName)`:  
Execute another script like the current one
* `config.has(name)`:  
Whether the given field is set
* `config.get(name, default?)`:  
Returns the value of the given field or `default` when it is not set
* `config.ask(question, name, accept?, required?, default?, force?, parse?)`:  
Asks the user the given question if not value was set yet (overwrite via `force=True`)
* `config.set(name, value, accept?, parse?)`:  
Sets the given field to the given value. Supports optional type checking and value parsing.

* file.mv 


## Command Line Arguments

Every additional command line argument is automatically added to the configuration. This is useful for testing skeletons automatically e.g. via Travis.ci.