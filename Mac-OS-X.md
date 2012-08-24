The installation process on Mac OS X includes the installation of:

1. XCode
2. Homebrew
3. Python3
4. Python Setuptools (Distribute and PIP)
5. Jasy 


Installing XCode
----------------

Before installing Jasy you must install XCode to have all relevant developer tools like the GNU compilers ready on your system. XCode is available via the Mac App Store.

Installing Python3
------------------

We suggest you installing Python3 via Homebrew. That installs a Python3 matching your XCode and system environment and makes installing native Python modules like PIL or Misaka possible. Open a terminal and execute the following command:

    $ brew install python3

Python 3 is installed in parallel to Python2 (which is included in Mac OS since 10.5) and is made available on the command line as `python3`. Scripts like `jasy` are installed to `/usr/local/share/python3`. You need to add it to your `PATH` to make `jasy` easily available. For details have a look at this documentation: https://github.com/mxcl/homebrew/wiki/Homebrew-and-Python. Homebrew is typically installed in a folder which does not require admin rights. All further installations should happen **without `sudo`**.

You need to install the package manager "PIP" to install Python packages. PIP requires Distribute being installed first. (See also: http://www.pip-installer.org/en/latest/installing.html)

Open a Terminal and execute the following commands:

    $ cd ~/Downloads
    $ curl http://python-distribute.org/distribute_setup.py | python3
    $ curl https://raw.github.com/pypa/pip/master/contrib/get-pip.py | python3


Installing Jasy
---------------

Now you should be able to install the current stable version of Jasy using _pip_ . PIP is preferred over alternatives like `easy_install` as it supports uninstalls and upgrades etc. 

    $ pip-3.2 install jasy

Try the following command on your console/terminal after installation is complete:

    $ jasy

This should print out the help screen.

Optional: Installing Misaka for generating JavaScript API docs
--------------------------------------------------------------


Optional: Installing PIL for sprite sheet generation
----------------------------------------------------

1. Install libjpeg using Homebrew
2. use pip to install a python3 port of PIL :
`sudo pip-3.2 install -e git+git://github.com/sloonz/pil-py3k.git#egg=pil`