# Requirements #

Python 2.4 or higher (Python 3.X is not supported yet). You can get the latest 2.x stable Python release from here: [http://www.python.org/download/](http://www.python.org/download/).

# Using easy\_install / setuptools #

If you have [easy\_install / setuptools](http://pypi.python.org/pypi/setuptools) on your system, installing pyftpdlib is quite simple. Just run:
```
easy_install pyftpdlib
```
This will get the most updated pyftpdlib from the Python [pypi repository](http://pypi.python.org/pypi), unpack it and install it automatically.

Note: if you already have an old version of pyftpdlib installed, easy\_install
will not automatically download the latest version. You can ask for a particular version by running, for example:
```
easy_install.py pyftpdlib==0.3.0
```

# Manual installation #

If you have downloaded a pyftpdlib package, follow the following steps:

Unpack it (Windows users could use 7Zip, WinRar or other similar program):
```
tar zxvf pyftpdlib-0.3.0.tar.gz
```

Change to the pyftpdlib directory:
```
cd pyftpdlib
```

Run setup.py to install pyftpdlib. This step need to be run as root.
```
python setup.py install
```

If you're on Windows just run:
```
setup.py install
```