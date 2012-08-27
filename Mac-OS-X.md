The installation process on Mac OS X includes the installation of:

1. XCode (AppStore)
2. Homebrew
3. Python 3
4. Jasy 


Installing XCode
----------------

Before installing Jasy you must install XCode to have all relevant developer tools like the GNU compilers ready on your system. XCode is available via the [Mac App Store](http://itunes.apple.com/de/app/xcode/id497799835?mt=12).

Alternative: Install the "Command Line Tools for Xcode package" instead from [Apple's develper portal](http://developer.apple.com/downloads) (Requires a free Apple ID)


Installing Homebrew
-------------------

Homebrew is an easy to use package manager for Mac OS X. You can easily install Homebrew via:

```
$ ruby <(curl -fsSkL raw.github.com/mxcl/homebrew/go)
```

You should not use `sudo` for installing Homebrew. This makes tons of problems. Homebrew installs itself into `/usr/local`, so make sure the directory rights are set correctly (`chmod -R a+w /usr/local`). All further installations with Homebrew should happen **without `sudo`** as well!


Installing Python3
------------------

We suggest building and installing Python3 via Homebrew. That installs a Python3 matching your XCode and system environment and makes installing native Python modules like PIL or Misaka possible. Open a terminal and execute the following command:

```bash
$ brew install python3
```

Python 3 is installed in parallel to Python2 (which is included in Mac OS since 10.5) and is made available on the command line as `python3`. 

Scripts like `jasy` are installed to `/usr/local/share/python3`. You need to add it to your `PATH` to make `jasy` easily available.

```bash
$ echo 'export PATH="/usr/local/share/python3:$PATH"' >> ~/.profile
```

Installing Jasy
---------------

Now you should be able to install the current stable version of Jasy using _pip_ . PIP is preferred over alternatives like `easy_install` as it supports uninstalls and upgrades etc. 

```bash
$ pip-3.2 install jasy
```

Try the following command on your console/terminal after installation is complete:

```bash
$ jasy
```

This should print out the help screen.


Optional Packages
-----------------

### Misaka for generating JavaScript API docs

Use pip to install Misaka: 

```bash
$ pip-3.2 install misaka
```

### PIL for sprite sheet generation

Install `libjpeg` using Homebrew and PIL via `pip`:

```bash
$ brew install libjpeg
$ pip-3.2 install -e git+git://github.com/sloonz/pil-py3k.git#egg=pil
```

