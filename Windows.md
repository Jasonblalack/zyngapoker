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


Installing Jasy
---------------

Jasy should finally be installed using the Python package manager "PIP" which has fine support for advanced features like upgrades and uninstallation of packages, too. Open the "Admin Console" and execute the following command:

    pip-3.2 install jasy

This should print out some messages about dependencies which are being installed like "polib" and "pygments". 


Testing Installation
--------------------

Try the following command on your console after installation is complete:

    jasy

This should print out the help screen.


Optional: Installing Misaka for generating JavaScript API docs
--------------------------------------------------------------

TODO


Optional: Installing PIL for sprite sheet generation
----------------------------------------------------

1. Go to this [Page](http://www.lfd.uci.edu/~gohlke/pythonlibs/)
2. Look for PIL
3. Download the `.exe` corresponding to your platform (32 vs 64 bits) and Python runtime (3.2 vs 3.3)
4. Install it!

