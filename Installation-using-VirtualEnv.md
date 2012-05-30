# Installation using VirtualEnv

Be sure to install Python3 and VirtualEnv for Python3 first. Then create a new environment for your Jasy work:

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

Then continue with the installation of Jasy.

```bash
$ pip install jasy
```

There are some optional dependencies you might want to install now. You can easily do this by using `pip` again:

```bash
$ pip install -r doc/jasy-$INSTALLED_VERSION/requirements.txt
```

Keep in mind that for some of the C-extensions you need to have a C compiler installed.