The installation process on Windows includes the installation of:

1. Python 3
2. PIP
3. Jasy

Installing Python3
------------------

Install Python3 using the installer offered at the [Python homepage](http://www.python.org/getit/releases/). Please choose the latest 3.x release version as 32 bit. Even if your Windows is 64 bit it is important to install the 32 bit version.

Python on Windows is installed in "C:\PythonXX" where XX stands for the major and minor version. For Python 3.2.x this folder is named "Python32". The executables are named without any version information so just using `python` on the command line should work fine. Be sure to add `C:\Python32;C:\Python32\Scripts` to your `PATH` (using the system preferences dialog).


Installing PIP
--------------

Download the following scripts using your browser:

* [Distribute](http://python-distribute.org/distribute_setup.py)
* [PIP](https://raw.github.com/pypa/pip/master/contrib/get-pip.py)

Open the "Admin Console", change the working directory to the download location and execute the following two commands:

    python distribute_setup.py
    python get-pip.py

_Alternative_: Using binary installers for Distribute and PIP from this page: http://www.lfd.uci.edu/~gohlke/pythonlibs/


Installing Jasy
---------------

Jasy should finally be installed using the Python package manager "PIP" which has fine support for advanced features like upgrades and uninstallation of packages, too. Open the "Admin Console" and execute the following command:

    pip-3.2 install jasy

Try the following command on your "Console" after installation is complete:

    jasy

This should print out the help screen.


Optional Packages
-----------------

### Misaka for generating JavaScript API docs

Unfortunately not supported at the moment. 

There is an old [Misaka 0.4.1](http://pypi.python.org/pypi/misaka/0.4.1) build available on PyPI - but that's not officially supported and might have bugs. Featurewise it's pretty much identical to the 1.0.x releases.

### PIL for sprite sheet generation

1. Go to this [Page](http://www.lfd.uci.edu/~gohlke/pythonlibs/) and look for "PIL"
2. Download and execute the installer corresponding to your platform (32 vs 64 bits) and Python runtime (2.7 vs 3.2)

### C-based YAML implementation

There is a pre-built PyYaml implementation ready for download. This one is linked against the C implementation of Yaml and should be somewhat faster. It can be found [here](http://www.lfd.uci.edu/~gohlke/pythonlibs/).