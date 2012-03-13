The installation process on Linux includes the installation of:

1. Python3
2. Python Setuptools (Distribute and PIP)
3. Jasy Installation using PIP

Installing Python3
------------------

To install Python 3 use your package manager on that system. On Debian based systems like Ubuntu you typically use `apt-get` for the installation. 

Normally there is also a package "Python3 Setup Tools". Currently this does not seem to work because of a transition of `setuptools` to Python3. A working alternative is the method using `distribute` and `pip` as explained below.

Installing Setuptools
---------------------

Execute the following commands to install D^istribute and PIP.

    $ wget -qO- http://python-distribute.org/distribute_setup.py | sudo python3
    $ wget -qO- https://raw.github.com/pypa/pip/master/contrib/get-pip.py | sudo python3

Python 3 is installed in parallel to Python2 and is typically made available on the command line as `python3`.

Installing Cython
---------------
{code}
$ pip-3.2 install cython
{code}

Installing Jasy
---------------

You can install the current stable version of Jasy using _pip_ . PIP is preferred over alternatives like `easy_install` as it supports uninstalls and upgrades etc. 

    $ pip-3.2 install jasy

Try the following command on your console/terminal after installation is complete:

    $ jasy

There should be an error message `Cannot find any Jasy script with task definitions (jasyscript.py)!` which is in fact a success message showing you that Jasy is working.