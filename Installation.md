# Installation

## 1. Installing Python3

You just need a Python 3 installation. Typical Unix based systems like Mac OS X and Ubuntu Linux are typically delivered with a Python 2.x version only at the moment. 

### Mac OS X

We recommend [Homebrew](http://mxcl.github.com/homebrew/). It uses the pre-installed Unix tools and just builds the immediate requirements for the apps you like to install. After you have installed the `brew` command just do a `brew install python3`. Scripts are installed to `/usr/local/share/python3`. You need to add it to your `PATH` to make `jasy` easily available. See also: https://github.com/mxcl/homebrew/wiki/Homebrew-and-Python

### Linux

To install Python 3 use your package manager on that system. On Debian based systems like Ubuntu you typically use `apt-get` for the installation. Normally there is also a package "Python3 Setup Tools". Currently this does not seem to work because of a transition of `setuptools` to Python3. A working alternative is the method using `distribute` as explained below.

### Windows

Install Python3 using the installer offered at the [Python homepage](http://www.python.org/getit/releases/). Please choose the latest 3.x release version.

## 2. Installing PIP

If `pip` for Python 3 is not installed, you can easily do this following the easy steps:

    $ curl http://python-distribute.org/distribute_setup.py | python3
    $ curl https://raw.github.com/pypa/pip/master/contrib/get-pip.py | python3

See also: http://www.pip-installer.org/en/latest/installing.html

This should work on all systems. On Windows you must be sure to execute it in the "Admin Console". On Linux it might require a "sudo". After this `pip` should be installed on your system. Sometimes you need to add the scripts folder to your PATH though. 

## 3. Installing Jasy

You can install the current stable version of Jasy using _pip_ or _easy_install_. PIP is preferred as it supports uninstalls etc. Be sure to use the Python3 version of them. Sometimes these scripts are named differently when installing in parallel to Python-2.x: `pip3`, `pip-3` or `pip-3.2`. Best is to just use tab completion.

    $ pip-3.2 install jasy

You should definitely prefer `pip` over the older `easy_install` because it has features like updates and uninstall. 

### Windows Troubleshooting

Jasy installs a few dependencies like [polib](http://pypi.python.org/pypi/polib), [misaka](http://pypi.python.org/pypi/misaka/), [pygments](http://pygments.org/) and [msgpack-python](http://msgpack.org/). Most of them are pure Python packages and a pretty easy to install. Misaka is a little bit special as it requires a GNU C compiler being installed to compile C source files. This is not a major issue under Mac OS X (with XCode installed) or on any typical Linux system. On Windows one have to install [MinGW](http://www.mingw.org/) first to compile the C files. A way easier alternative is to install Misaka using the downloadable binary installer. This is the suggested installation order for Windows users:

* Python 3.x (using official installer): http://python.org/download/releases/
* Downloading Distribute: http://python-distribute.org/distribute_setup.py
* Downloading PIP: https://raw.github.com/pypa/pip/master/contrib/get-pip.py
* Install Distribute: `$ python3 distribute_setup.py`
* Install PIP: `$ python3 get-pip.py`
* Install Misaka using [binary installer](http://pypi.python.org/packages/3.2/m/misaka/misaka-0.4.1.win32-py3.2.msi#md5=2c99bf3926a1c768a66d5b52084923ba)
* Install Jasy using PIP: `pip install jasy`

## 4. Testing the Installation

Try the following on your console/terminal (Windows users need to execute `jasy.bat` instead):

    $ jasy
    !!! Did not found 'jasyscript.py'!

The error message is in fact a success message showing you that Jasy is working. No start building your `jasyscript.py` inside your application.

