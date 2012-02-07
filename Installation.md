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

    wget -qO- http://python-distribute.org/distribute_setup.py | python3
    wget -qO- https://raw.github.com/pypa/pip/master/contrib/get-pip.py | python3

Python 3 is installed in parallel to Python2 and is typically made available on the command line as `python3`.

### Windows

Download the following scripts using your browser:

* [Distribute](http://python-distribute.org/distribute_setup.py)
* [PIP](https://raw.github.com/pypa/pip/master/contrib/get-pip.py)

Open the "Admin Console", change the working directory to the download location and execute the following two commands:

    python distribute_setup.py
    python get-pip.py

Python on Windows is installed in "C:\PythonXX" where XX stands for the major and minor version. The executables are named  without any version information so just using `python` on the command line should work fine.

### Testing the Installation

There will be a lot of messages printed out during installation. As long as there is no real error message all should went fine and you should have a `pip` command on the command line available now. You can try it out just executing it without any arguments:

    pip-3.2

You should see some usage hints like "Usage: pip COMMAND [OPTIONS]".

## 3. Installing Jasy

You can install the current stable version of Jasy using _pip_ . PIP is preferred over alternatives like `easy_install` as it supports uninstalls and upgrades etc. 

    pip-3.2 install jasy

### Binary Packages on Windows

It this fails for you, you might be missing a GNU C compiler to compile the C libraries of [misaka](http://pypi.python.org/pypi/misaka/) and [msgpack-python](http://msgpack.org/). On Windows one have to install [MinGW](http://www.mingw.org/) first allow compilation of the files. 

The way easier alternative is to install Misaka and Msgpack using binary installers first:

* Install Misaka using [binary installer](http://pypi.python.org/packages/3.2/m/misaka/misaka-0.4.1.win32-py3.2.msi#md5=2c99bf3926a1c768a66d5b52084923ba)
* Install Msgpack using [binary installer](http://www.lfd.uci.edu/~gohlke/pythonlibs/fj2ir7sn/msgpack-python-0.1.12.win32-py3.2.exe)
* Afterwards continue with the installation of Jasy using PIP: `pip-3.2 install jasy`

### Testing the Installation

Try the following command on your console/terminal:

    jasy

There should be an error message `Cannot find any Jasy script with task definitions (jasyscript.py)!` which is in fact a success message showing you that Jasy is working. No start building your `jasyscript.py` inside your application.

