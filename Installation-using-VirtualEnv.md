Installation using VirtualEnv
-----------------------------

Be sure to install Python 3 and VirtualEnv for Python 3 first. On Mac OS and Linux this typically happens via PIP. On Windows there is also a [binary package of VirtualEnv](http://www.lfd.uci.edu/~gohlke/pythonlibs/) available.

Then create a new environment for your Jasy work:

```bash
$ virtualenv jasy
New python executable in jasy/bin/python3
Also creating executable in jasy/bin/python
Installing distribute............................................................................................................................................................................................................................................................................................................................................................done.
Installing pip...............done.
```

When that's done you can include the `activate` script to activate the Jasy environment:

```bash
$ cd jasy
$ source bin/activate
```

Your prompt should now have been changed with a prefix called "(jasy)".

Then continue with the [normal installation of Jasy](Installation) and optional modules like Misaka and PIL as described on the main installation pages.

```bash
$ pip install jasy
```
