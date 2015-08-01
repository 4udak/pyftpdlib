# Version: 0.6.0 - Date: 2011-01-24 #

## Enhancements ##

  * [Issue 68](https://code.google.com/p/pyftpdlib/issues/detail?id=68): added full FTPS (FTP over SSL/TLS) support.
  * [Issue 86](https://code.google.com/p/pyftpdlib/issues/detail?id=86): pyftpdlib now reports all ls and MDTM timestamps as GMT times, as recommended in RFC-3659.  A `FTPHandler.use_gmt_times` attributed has been added and can be set to False in case local times are desired instead.
  * [Issue 124](https://code.google.com/p/pyftpdlib/issues/detail?id=124): pyftpdlib now accepts command line options to configure a stand alone anonymous FTP server when running pyftpdlib with python's -m option.
  * [Issue 125](https://code.google.com/p/pyftpdlib/issues/detail?id=125): logs are now provided in a standardized format parsable by log analyzers. FTPHandler class provides two new methods to standardize both commands and transfers logging: log\_cmd() and log\_transfer().
  * [Issue 127](https://code.google.com/p/pyftpdlib/issues/detail?id=127): `added FTPHandler.masquerade_address_map` option which allows you to define multiple 1 to 1 mappings in case you run a FTP server with multiple private IP addresses behind a NAT firewall with multiple public IP addresses.
  * [Issue 128](https://code.google.com/p/pyftpdlib/issues/detail?id=128): files and directories owner and group names and `os.readlink` are now resolved via `AbstractedFS` methods instead of in `format_list()`.
  * [Issue 129](https://code.google.com/p/pyftpdlib/issues/detail?id=129): added 3 new callbacks to `FTPHandler` class: `on_incomplete_file_sent()`, `on_incomplete_file_received()` and `on_login()`.
  * [Issue 130](https://code.google.com/p/pyftpdlib/issues/detail?id=130): added `UnixAuthorizer` and `WindowsAuthorizer` classes defined in the new `pyftpdlib.contrib.authorizers` module.
  * [Issue 131](https://code.google.com/p/pyftpdlib/issues/detail?id=131): pyftpdlib is now able to serve both IPv4 and IPv6 at the same time by using a single socket.
  * [Issue 133](https://code.google.com/p/pyftpdlib/issues/detail?id=133): `AbstractedFS` constructor now accepts two argumets: `root` and `cmd_channel` breaking compatibility with previous version.  Also, `root` and `cwd` attributes became properties.  The previous bug consisting in re-setting the root from the ftp handler after user login has been fixed to ease the development of subclasses.
  * [Issue 134](https://code.google.com/p/pyftpdlib/issues/detail?id=134): enabled TCP\_NODELAY socket option for the FTP command channels resulting in pyftpdlib being twice faster.
  * [Issue 135](https://code.google.com/p/pyftpdlib/issues/detail?id=135): Python 2.3 support has been removed.
  * [Issue 137](https://code.google.com/p/pyftpdlib/issues/detail?id=137): added new `pyftpdlib.contrib.filesystems` module within `UnixFilesystem` class which permits the client to escape its home directory and navigate the real filesystem.
  * [Issue 138](https://code.google.com/p/pyftpdlib/issues/detail?id=138): added `DTPHandler.get_elapsed_time()` method which returns the transfer elapsed time in seconds.
  * [Issue 144](https://code.google.com/p/pyftpdlib/issues/detail?id=144): a "username" parameter is now passed to authorizer's terminate\_impersonation() method.
  * [Issue 149](https://code.google.com/p/pyftpdlib/issues/detail?id=149): ftpserver.proto\_cmds dictionary refactoring and get rid of `_CommandProperty` class.

## Bugfixes ##

  * [Issue 120](https://code.google.com/p/pyftpdlib/issues/detail?id=120): an `ActiveDTP` instance is not garbage collected in case a client issuing PORT disconnects before establishing the data connection.
  * [Issue 122](https://code.google.com/p/pyftpdlib/issues/detail?id=122): a wrong variable name was used in `AbstractedFS.validpath` method.
  * [Issue 123](https://code.google.com/p/pyftpdlib/issues/detail?id=123): PORT command doesn't bind to correct address in case an alias is created for the local network interface.
  * [Issue 140](https://code.google.com/p/pyftpdlib/issues/detail?id=140): pathnames returned in PWD response should have double-quotes '"' escaped.
  * [Issue 143](https://code.google.com/p/pyftpdlib/issues/detail?id=143): EINVAL not properly handled causes server crash on OSX.
  * [Issue 146](https://code.google.com/p/pyftpdlib/issues/detail?id=146): SIZE and MDTM commands are now rejected unless the "l" permission has been specified for the user.

## API changes since 0.5.2 ##

_Backward incompatible changes are marked in red_

  * <font color='red'>removed support for <b>Python 2.3</b>.</font>
  * all classes are now new-style classes.
  * added a new package in pyftpdlib namespace: "**contrib**". Modules (and classes) defined here:
    * pyftpdlib.contrib.handlers.py (**TLS\_FTPHandler**)
    * pyftpdlib.contrib.authorizers.py (**UnixAuthorizer**, **WindowsAuthorizer**)
    * pyftpdlib.contrib.filesystems.py (**UnixFilesystem**)
  * **AbstractedFS** class:
    * <font color='red'><b><code>__init__</code></b> method now accepts two arguments: <b>root</b> and <b>cmd_channel</b>.</font>
    * <font color='red'><b>root</b> and <b>cwd</b> attributes are now read-only properties.</font>
    * 3 new methods have been added:
      * **get\_user\_by\_uid()**
      * **get\_group\_by\_gid()**
      * **readlink()**
  * **FTPHandler** class:
    * new class attributes:
      * **use\_gmt\_times**
      * **tcp\_no\_delay**
      * **masquerade\_address\_map**
    * new methods:
      * **on\_incomplete\_file\_sent()**
      * **on\_incomplete\_file\_received()**
      * **on\_login()**
      * **log\_cmd()**
      * **log\_transfer()**
    * <font color='red'><b>proto_cmds</b> class attribute has been added.  The <b>FTPHandler</b> class no longer relies on <b>ftpserver.proto_cmds</b> global dictionary but on <b>ftpserver.FTPHandler.proto_cmds</b> instead.</font>
  * **FTPServer** class:
    * <font color='red'><b>max_cons</b> attribute defaults to 512 by default instead of 0 (unlimited).</font>
    * <font color='red'><b>serve_forever()</b>'s <b>map</b> attribute is gone.</font>
  * **DummyAuthorizer** class
    * <font color='red'><b>ValueError</b> is now raised instead of <b>AuthorizerError</b> in case of bad arguments.</font>
    * <font color='red'><b>terminate_impersonation()</b> method now expects a "username" parameter.</font>
  * **DTPHandler.get\_elapsed\_time()** method was added.


## Migration notes ##

Some of the changes introduced in 0.6.0 break compatibility with previous 0.5.x serie.  In particular you should be careful in case you're using a customized file system class defining an `__init__` method since `AbstractedFS` constructor now expects two arguments. For everything else take a look at the items aboved marked in red.

## FTPS (FTP over TLS/SSL) support ##

Finally, this has been officially included in main library namespace, granting a long-term support, bugfixes, backward compatible maintanance etc. In order to use FTPS you need [PyOpenSSL](http://pypi.python.org/pypi/pyOpenSSL) module. Here is an example of an FTPS server:

```
from pyftpdlib import ftpserver
from pyftpdlib.contrib.handlers import TLS_FTPHandler

def main():
    authorizer = ftpserver.DummyAuthorizer()
    authorizer.add_user('user', '12345', '.', perm='elradfmw')
    authorizer.add_anonymous('.')
    handler = TLS_FTPHandler
    handler.certfile = 'keycert.pem'
    handler.authorizer = authorizer
    ftpd = ftpserver.FTPServer(('', 21), handler)
    ftpd.serve_forever()

if __name__ == '__main__':
    main()
```

## Real UNIX FTP server thanks to new UnixAuthorizer and UnixFileSystem classes ##

`UnixAuthorizer` has been moved from demo directory into main pyftpdlib namespace, including a finer access control system and per-user options:

```
>>> from pyftpdlib.contrib.authorizers import UnixAuthorizer
>>> # accept all except root
>>> auth = UnixAuthorizer(rejected_users=["root"])
>>> # accept some users only
>>> auth = UnixAuthorizer(allowed_users=["matt", "jay"])
>>> # accept everybody and don't care if they have not a valid shell
>>> auth = UnixAuthorizer(require_valid_shell=False)
>>> # set different options for a specific user
>>> auth.override_user("matt", password="foo", perm="elr")
```

`UnixAuthorizer` can be used in conjunction with new `UnixFileSystem` class to implement a "real" UNIX FTP server:

```
from pyftpdlib import ftpserver
from pyftpdlib.contrib.authorizers import UnixAuthorizer
from pyftpdlib.contrib.filesystems import UnixFilesystem

def main():
    authorizer = UnixAuthorizer(rejected_users=["root"], require_valid_shell=True)
    handler = ftpserver.FTPHandler
    handler.authorizer = authorizer
    handler.abstracted_fs = UnixFilesystem
    server = ftpserver.FTPServer(('', 21), handler)
    server.serve_forever()

if __name__ == "__main__":
    main()
```

## Windows authorizer ##

As for Unix authorizer, also Windows authorizer has been moved in main pyftpdlib namespace.
Thanks to `pyftpdlib.contrib.authorizers.WindowsAuthorizer` class you can use "real" Windows system users:

```
>>> from pyftpdlib.contrib.authorizers import WindowsAuthorizer
>>> # accept all except Administrator
>>> auth = UnixAuthorizer(rejected_users=["Administrator"])
>>>
>>> # accept some users only
>>> auth = UnixAuthorizer(allowed_users=["matt", "jay"])
>>>
>>> # set specific options for a user
>>> auth.override_user("matt", password="foo", perm="elr")
```

## Command line options ##

pyftpdlib now accepts command line options to configure a stand alone anonymous FTP server when running pyftpdlib with python's -m option.
Some examples.

Read-only anonymous FTP serving the current working directory:

```
giampaolo@ubuntu:~/svn/pyftpdlib$ sudo python -m pyftpdlib.ftpserver 
Serving FTP on 0.0.0.0:21
```

Anonymous FTP server with write access:

```
giampaolo@ubuntu:~$ sudo python -m pyftpdlib.ftpserver -w
/usr/local/lib/python2.6/dist-packages/pyftpdlib/ftpserver.py:520: RuntimeWarning: write permissions assigned to anonymous user.
  RuntimeWarning)
Serving FTP on 0.0.0.0:21
```

Set a different address/port and home directory:

```
giampaolo@ubuntu:~/svn/pyftpdlib$ python -m pyftpdlib.ftpserver -i 127.0.0.1 -p 8021 -d /home/giampaolo
Serving FTP on 127.0.0.1:8021
```


## New event callbacks ##

3 new event callbacks for user login/logout and incomplete file transfers have been added. Follows an example using all of them:

```
from pyftpdlib import ftpserver

class MyHandler(ftpserver.FTPHandler):

    def on_login(self, username):
        # do something when user login
        pass

    def on_logout(self, username):
        # do something when user logs out
        pass
        
    def on_file_sent(self, file):
        # do something when a file has been sent
        pass

    def on_file_received(self, file):
        # do something when a file has been received
        pass
            
    def on_incomplete_file_sent(self, file):
        # do something when a file is partially sent
        pass

    def on_incomplete_file_received(self, file):
        # remove partially uploaded files
        os.remove(file)


def main():
    authorizer = ftpserver.DummyAuthorizer()
    authorizer.add_anonymous(homedir='.')
    handler = MyHandler
    handler.authorizer = authorizer
    server = ftpserver.FTPServer(('', 21), handler)
    server.serve_forever()

if __name__ == "__main__":
    main()
```