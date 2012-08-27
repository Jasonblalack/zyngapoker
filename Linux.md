The installation process on Linux includes the installation of:

1. Python3
2. PIP
3. Jasy


Installing Python3
------------------

To install Python 3 use your package manager on that system. On Debian based systems like Ubuntu you typically use `apt-get` for the installation. 

Python3 : 

    $ apt-get install python3


Installing PIP
--------------

Python3 PIP :

    $ apt-get install python3-pip

_NOTE_ : Ubuntu <= 12.04 doesn't have a python3-pip package in its `universe` repos. One can be found [here](http://ubuntu.mirror.cambrium.nl/ubuntu//pool/universe/p/python-pip/python3-pip_1.1-3_all.deb)

### Alternative method for other distributions : Installing Setuptools

Execute the following commands to install Distribute and PIP.

    $ wget -qO- http://python-distribute.org/distribute_setup.py | sudo python3
    $ wget -qO- https://raw.github.com/pypa/pip/master/contrib/get-pip.py | sudo python3

Python 3 is installed in parallel to Python2 and is typically made available on the command line as `python3`.


Installing Jasy
---------------

You can install the current stable version of Jasy using _pip_ . PIP is preferred over alternatives like `easy_install` as it supports uninstalls and upgrades etc. 

    $ pip-3.2 install jasy

Try the following command on your console/terminal after installation is complete:

    $ jasy

This should print out the help screen.


Optional Packages
-----------------

## Misaka for generating JavaScript API docs

Use pip to install Misaka: 

    $ pip-3.2 install misaka

## Installing PIL for sprite sheet generation

Use pip to install PIL from Git: 

    $ pip-3.2 install -e git+git://github.com/sloonz/pil-py3k.git#egg=pil
