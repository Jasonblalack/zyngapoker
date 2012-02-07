# Installation

## 1. Installing Python3

You just need a Python 3 installation. Typical Unix based systems like Mac OS X and Ubuntu Linux are typically delivered with a Python 2.x version only at the moment. 

### Mac OS X

It is suggested to install XCode first, to have the typical compilers and linkers installed on your system.

You have different options to install Python 3 on Windows:

* Homebrew: After you have installed the `brew` command just do a `brew install python3`. Scripts are installed to `/usr/local/share/python3`. You need to add it to your `PATH` to make `jasy` easily available. See also: https://github.com/mxcl/homebrew/wiki/Homebrew-and-Python
* Official Python 3 Installer from http://python.org/download/releases/

### Linux

To install Python 3 use your package manager on that system. On Debian based systems like Ubuntu you typically use `apt-get` for the installation. 

Normally there is also a package "Python3 Setup Tools". Currently this does not seem to work because of a transition of `setuptools` to Python3. A working alternative is the method using `distribute` and `pip` as explained below.

### Windows

Install Python3 using the installer offered at the [Python homepage](http://www.python.org/getit/releases/). Please choose the latest 3.x release version as **32 bit**. Even if your Windows is 64 bit it is important to install the 32 bit version. Be sure to add "C:\Python32;C:\Python32\Scripts" to your PATH (using e.g. the system preferences dialog).

## 2. Installing Distribute and PIP

You need to install the package manager "PIP" to install Python packages. PIP requires Distribute being installed first. (See also: http://www.pip-installer.org/en/latest/installing.html)

### Mac OS

Open a Terminal and execute the following commands:

    curl http://python-distribute.org/distribute_setup.py | python3
    curl https://raw.github.com/pypa/pip/master/contrib/get-pip.py | python3

Python 3 is installed in parallel to Python2 and is typically made available on the command line as `python3`.

### Linux

It might be required to use "sudo" to have installation rights.

    wget http://python-distribute.org/distribute_setup.py | python3
    wget https://raw.github.com/pypa/pip/master/contrib/get-pip.py | python3

Python 3 is installed in parallel to Python2 and is typically made available on the command line as `python3`.

### Windows

Download the following scripts using your browser:

* [Distribute](http://python-distribute.org/distribute_setup.py)
* [PIP](https://raw.github.com/pypa/pip/master/contrib/get-pip.py)

Open the "Admin Console", change the working directory to the download location and execute the following two commands:

    python distribute_setup.py
    python get-pip.py


### All

There will be a lot of messages printed out. As long as there is no real error message all should went fine and you should have a `pip` command on the command line available now. You can try it out just executing it without any arguments:

    pip

You should see some usage hints like "Usage: pip COMMAND [OPTIONS]".

## 3. Installing Jasy

You can install the current stable version of Jasy using _pip_ or _easy_install_. PIP is preferred as it supports uninstalls etc. Be sure to use the Python3 version of them. Sometimes these scripts are named differently when installing in parallel to Python-2.x: `pip3`, `pip-3` or `pip-3.2`. Best is to just use tab completion.

    $ pip-3.2 install jasy

You should definitely prefer `pip` over the older `easy_install` because it has features like updates and uninstall. 

### Windows Hints

Jasy installs a few dependencies like [polib](http://pypi.python.org/pypi/polib), [misaka](http://pypi.python.org/pypi/misaka/), [pygments](http://pygments.org/) and [msgpack-python](http://msgpack.org/). Most of them are pure Python packages and are easily installable. Misaka is a little bit special as it requires a GNU C compiler being installed to compile C source files. This is not a major issue under Mac OS X (with XCode installed) or on any typical Linux system. On Windows one have to install [MinGW](http://www.mingw.org/) first to compile the C files. A way easier alternative is to install Misaka using the downloadable binary installer. This is the suggested installation order for Windows users:

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

