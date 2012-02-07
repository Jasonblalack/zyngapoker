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

You need to modify your `PATH` to make Python as well as any scripts installed by Python being available globally on your Terminal.


Installing Setuptools
---------------------

You need to install the package manager "PIP" to install Python packages. PIP requires Distribute being installed first. (See also: http://www.pip-installer.org/en/latest/installing.html)

Open a Terminal and execute the following commands:

=== Homebrew

    $ cd ~/Downloads
    $ curl http://python-distribute.org/distribute_setup.py | python3
    $ curl https://raw.github.com/pypa/pip/master/contrib/get-pip.py | python3

=== Python Installer

    $ cd ~/Downloads
    $ curl http://python-distribute.org/distribute_setup.py | sudo python3
    $ curl https://raw.github.com/pypa/pip/master/contrib/get-pip.py | sudo python3

Python 3 is installed in parallel to Python2 (which is included in Mac OS since 10.5) and is made available on the command line as `python3`.


Installing Jasy
---------------

Now you should be able to install the current stable version of Jasy using _pip_ . PIP is preferred over alternatives like `easy_install` as it supports uninstalls and upgrades etc. 

    $ pip-3.2 install jasy

Try the following command on your console/terminal after installation is complete:

    $ jasy

There should be an error message `Cannot find any Jasy script with task definitions (jasyscript.py)!` which is in fact a success message showing you that Jasy is working. No start building your `jasyscript.py` inside your application.