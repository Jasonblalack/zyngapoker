The installation process on Linux includes the installation of:

1. Python3
2. Python Setuptools (Distribute and PIP)
3. Jasy Installation using PIP

Installing Python3 && PIP
-------------------------

To install Python 3 use your package manager on that system. On Debian based systems like Ubuntu you typically use `apt-get` for the installation. 

Python3 : 

    $ apt-get install python3

Python3 PIP : 

    $ apt-get install python3-pip

Note : Ubuntu <= 12.04 doesn't have a python3-pip package in its `universe` repos. One can be found [here](http://ubuntu.mirror.cambrium.nl/ubuntu//pool/universe/p/python-pip/python3-pip_1.1-3_all.deb)

Installing Setuptools (On non-Ubuntu systems)
---------------------------------------------

Execute the following commands to install D^istribute and PIP.

    $ wget -qO- http://python-distribute.org/distribute_setup.py | sudo python3
    $ wget -qO- https://raw.github.com/pypa/pip/master/contrib/get-pip.py | sudo python3

Python 3 is installed in parallel to Python2 and is typically made available on the command line as `python3`.

Installing Jasy
---------------

You can install the current stable version of Jasy using _pip_ . PIP is preferred over alternatives like `easy_install` as it supports uninstalls and upgrades etc. 

    $ pip-3.2 install jasy

Try the following command on your console/terminal after installation is complete:

    $ jasy

There should be an error message `Cannot find any Jasy script with task definitions (jasyscript.py)!` which is in fact a success message showing you that Jasy is working.


_NOTE:_ You may get some encoding errors with polib during jasy installation. If that happens, you need to find the directory containing the files with the encoding errors, back them up, and replace them with empty files of the same name. Then retry the jasy install and it should work.