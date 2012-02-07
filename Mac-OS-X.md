The installation process on Windows includes the installation of:

1. XCode
2. Python3
3. Python Setuptools (Distribute and PIP)
4. Jasy Installation using PIP

Installing XCode
----------------

Before installing Jasy you must install XCode to have all relevant developer tools like the GNU compilers ready on your system. XCode is available via the Mac App Store.

Installing Python3
------------------

You have different options to install Python 3 on your Mac:

* Homebrew: 
* Official Installer from 

### Installing via Homebrew

Open a terminal and execute the following command:

    $ brew install python3

Scripts like `jasy` are installed to `/usr/local/share/python3`. You need to add it to your `PATH` to make `jasy` easily available. For details have a look at this documentation: https://github.com/mxcl/homebrew/wiki/Homebrew-and-Python

Homebrew is typically installed in a folder which does not require admin rights. All further installations of Jasy could happen without `sudo`.

### Installing via Installer

Download the installer of the latest 3.x release via http://python.org/download/releases/, mount it and use the installer to install Python 3.x on your system. The official installer installs Python into system folders which means that all installations require to use `sudo` for installing any software.


Installing Setuptools
---------------------


Installing Jasy
---------------

