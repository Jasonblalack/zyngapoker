The installation process on Windows includes the installation of:

1. Python3
2. Python Setuptools (Distribute and PIP)
3. Binary Packages for Modules (Misaka and Msgpack)
4. Jasy Installation using PIP

Installing Python3
------------------

Install Python3 using the installer offered at the [Python homepage](http://www.python.org/getit/releases/). Please choose the latest 3.x release version as 32 bit. Even if your Windows is 64 bit it is important to install the 32 bit version.

Python on Windows is installed in "C:\PythonXX" where XX stands for the major and minor version. For Python 3.2.x this folder is named "Python32". The executables are named without any version information so just using `python` on the command line should work fine. Be sure to add `C:\Python32;C:\Python32\Scripts` to your `PATH` (using the system preferences dialog).


Installing Setuptools
---------------------

Download the following scripts using your browser:

* [Distribute](http://python-distribute.org/distribute_setup.py)
* [PIP](https://raw.github.com/pypa/pip/master/contrib/get-pip.py)

Open the "Admin Console", change the working directory to the download location and execute the following two commands:

    python distribute_setup.py
    python get-pip.py




Installing Binary Packages
--------------------------

On Windows you typically don't have any C compiler installed. To ease the installation process please download the following binary installers and execute them:

* [Misaka Binary Installer](http://pypi.python.org/packages/3.2/m/misaka/misaka-0.4.1.win32-py3.2.msi#md5=2c99bf3926a1c768a66d5b52084923ba)
* [Msgpack Binary Installer](http://www.lfd.uci.edu/~gohlke/pythonlibs/fj2ir7sn/msgpack-python-0.1.12.win32-py3.2.exe)


Installing Jasy
---------------

Jasy should finally be installed using the Python package manager "PIP" which has fine support for advanced features like upgrades and uninstallation of packages, too. Open the "Admin Console" and execute the following command:

    pip-3.2 install jasy

This should print out some messages about dependencies which are being installed like "polib" and "pygments". After the command is executed you should be able to execute "jasy":

    jasy

There should be an error message `Cannot find any Jasy script with task definitions (jasyscript.py)!` which is in fact a success message showing you that Jasy is working. 


Optional: Installing PIL for sprite sheet generation
----------------------------------------------------

1. Go to this [Page](http://www.lfd.uci.edu/~gohlke/pythonlibs/)
2. Look for PIL
3. Download the `.exe` corresponding to your platform (32 vs 64 bits) and Python runtime (3.2 vs 3.3)
4. Install it!