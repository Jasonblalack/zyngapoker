## jasyscript.py

The documentation for tasks has been moved from the decorator call to the method's doc string. Change:

```python
@task("Doc")
def mytask():
  ...
```

to

```python
@task
def mytask():
  """Doc"""
  ...
```
