# Version: 0.7.0 - Date: 2012-01-25 #

## Enhancements ##

  * [Issue 152](https://code.google.com/p/pyftpdlib/issues/detail?id=152): uploads (from server to client) on UNIX are now from **2x** (Linux) to **3x** (OSX) **faster** because of sendfile(2) system call usage.
  * [Issue 155](https://code.google.com/p/pyftpdlib/issues/detail?id=155): AbstractedFS "root" and "cwd" are no longer read-only properties but can be set via setattr().
  * [Issue 168](https://code.google.com/p/pyftpdlib/issues/detail?id=168): added FTPHandler.logerror() method. It can be overridden to provide more information (e.g. username) when logging exception tracebacks.
  * [Issue 174](https://code.google.com/p/pyftpdlib/issues/detail?id=174): added support for SITE CHMOD command (change file mode).
  * [Issue 177](https://code.google.com/p/pyftpdlib/issues/detail?id=177): setuptools is now used in setup.py
  * [Issue 178](https://code.google.com/p/pyftpdlib/issues/detail?id=178): added anti flood script in demo directory.
  * [Issue 181](https://code.google.com/p/pyftpdlib/issues/detail?id=181): added CallEvery class to call a function every x seconds.
  * [Issue 185](https://code.google.com/p/pyftpdlib/issues/detail?id=185): pass Debian licenscheck tool.
  * [Issue 189](https://code.google.com/p/pyftpdlib/issues/detail?id=189): the internal scheduler has been rewritten from scratch and it is an order of magnitude faster, especially for operations like cancel() which are involved when clients are disconnected (hence invoked very often).  Also, a single scheduled function now consumes 1/3 of the memory thanks to slots usage.
  * [Issue 196](https://code.google.com/p/pyftpdlib/issues/detail?id=196): added callback for failed login attempt.
  * [Issue 200](https://code.google.com/p/pyftpdlib/issues/detail?id=200): FTPServer.server\_forever() is now a class method.
  * [Issue 202](https://code.google.com/p/pyftpdlib/issues/detail?id=202): added [benchmark script](http://code.google.com/p/pyftpdlib/source/browse/trunk/test/bench.pybenchmark).

## Bugfixes ##

  * [Issue 156](https://code.google.com/p/pyftpdlib/issues/detail?id=156): data connection must be closed before sending 226/426 reply. This was against RFC-959 and was causing problems with older FTP clients.
  * [Issue 161](https://code.google.com/p/pyftpdlib/issues/detail?id=161): MLSD 'unique' fact can provide the same value for files having a similar device/inode but that in fact are different.  (patch by Andrew Scheller)
  * [Issue 162](https://code.google.com/p/pyftpdlib/issues/detail?id=162): (FTPS) SSL shutdown() is not invoked for the control connection.
  * [Issue 163](https://code.google.com/p/pyftpdlib/issues/detail?id=163): FEAT erroneously reports MLSD. (patch by Andrew Scheller)
  * [Issue 166](https://code.google.com/p/pyftpdlib/issues/detail?id=166): (FTPS) an exception on send() can cause server to crash (DoS).
  * [Issue 167](https://code.google.com/p/pyftpdlib/issues/detail?id=167): fix some typos returned on HELP.
  * [Issue 170](https://code.google.com/p/pyftpdlib/issues/detail?id=170): PBSZ and PROT commands are now allowed before authentication fixing problems with non-compliant FTPS clients.
  * [Issue 171](https://code.google.com/p/pyftpdlib/issues/detail?id=171): (FTPS) an exception when shutting down the SSL layer can cause server to crash (DoS).
  * [Issue 173](https://code.google.com/p/pyftpdlib/issues/detail?id=173): file last modification time shown in LIST response might be in a language different than English causing problems with some clients.
  * [Issue 175](https://code.google.com/p/pyftpdlib/issues/detail?id=175): FEAT response now omits to show those commands which are removed from proto\_cmds map.
  * [Issue 176](https://code.google.com/p/pyftpdlib/issues/detail?id=176): SO\_REUSEADDR option is now used for passive data sockets to prevent server running out of free ports when using passive\_ports directive.
  * [Issue 187](https://code.google.com/p/pyftpdlib/issues/detail?id=187): match proftpd LIST format for files having last modification time > 6 months.
  * [Issue 188](https://code.google.com/p/pyftpdlib/issues/detail?id=188): fix maximum recursion depth exceeded exception occurring if client quickly connects and disconnects data channel.
  * [Issue 191](https://code.google.com/p/pyftpdlib/issues/detail?id=191): (FTPS) during SSL shutdown() operation the server can end up in an infinite loop hogging CPU resources.
  * [Issue 199](https://code.google.com/p/pyftpdlib/issues/detail?id=199): UnixAuthorizer with require\_valid\_shell option is broken.

## Major API changes since 0.6.0 ##

  * New FTPHandler.use\_sendfile attribute.
    * sendfile() is now automatically used instead of plain send() if [pysendfile module](http://code.google.com/p/pysendfile/) is installed.
  * AbstractedFS `root` and `cwd` properties can now be set via setattr().
  * `FTPServer.serve_forever()` is a classmethod.
  * New `CallEvery` class.
  * New `FTPHandler.on_login_failed(username, password)` method.
  * New `FTPHandler.logerror(msg)` method.
  * New `FTPHandler.log_exception(instance)` method.


# New features in details #

## sendfile() ##

You should upgrade for this alone. 0.7.0 version finally introduces sendfile(2) system call usage. sendfile(2) provides a "zero-copy" way of copying data from one file descriptor to another (a socket). The phrase "zero-copy" refers to the fact that all of the copying of data between the two descriptors is done entirely by the kernel, with no copying of data into userspace buffers, resuting in file transfers (RETR, hence from server to client) being **from 2x to 3x faster**.
To enable sendfile you must install [pysendfile module](http://code.google.com/p/pysendfile/) first:

```
$ easy_install pysendfile
```

pyftpdlib will detect its presence and automatically use pysendfile for uploads (RETR).

Differences between send() and sendfile() are quite impressive. Here is a [benchmark](http://code.google.com/p/pyftpdlib/source/browse/trunk/test/bench.py) comparing pyftpdlib 0.7.0, pyftpdlib 0.6.0 [vsftpd](https://security.appspot.com/vsftpd.html) and [proftpd](http://www.proftpd.org/) servers (all of them using sendfile).
I'm not sure why, but it seems pyftpdlib is actually faster than proftpd and vsftpd (they're written in C and use sendfile()):

| **pyftpdlib 0.7.0** | **pyftpdlib 0.6.0** | **vsftpd 2.3.2** | **proftpd 1.3.4rc2** |
|:--------------------|:--------------------|:-----------------|:---------------------|
| 1694.14 MB/sec      | 693.41 MB/sec       | 1505.18 MB/sec   | 1313.77 MB/sec       |

## Faster scheduler ##

The internal scheduler, governed by `CallLater` and `CallEvery` classes, has been rewritten from scratch and it is an order of magnitue faster, especially for operations like CallLater.cancel() which are involved when clients are disconnected (hence invoked very often).  Some benchmarks:

| **schedule** | +0.5x |
|:-------------|:------|
| **reschedule** | +1.7x |
| **cancel**   | +477x  (with 1 milion scheduled functions) |
| **run**      | +8x   |

Also, a single scheduled function now consumes 1/3 of the memory thanks to `__slots__` usage.
For further details see [issue 189](https://code.google.com/p/pyftpdlib/issues/detail?id=189).

## SITE CHMOD ##

This new version supports SITE CHMOD command, meaning the client is now able to change file mode bits by issuing "SITE CHMOD path mode" command.
The authorizer now accepts a new "M" permission bit, which, when specidied, enables SITE CHMOD usage:

```
authorizer = DummyAuthorizer()
authorizer.add_user('user', 'password', '/home/user', perm='elradfmwM')
```

## FTPServer.server\_forever() is a class method ##

This was raised on the mailing list [here ](https://groups.google.com/forum/#!searchin/pyftpdlib/classmethod/pyftpdlib/Y6bZpkJfNWA/TbIF896A5qMJ). Basically, this is useful when one wants to listen on multiple sockets.
Instead of doing:

```
 >>> ftpd1 = ftpserver.FTPServer(address1, ftp_handler)
 >>> ftpd2 = ftpserver.FTPServer(address2, ftp_handler)
 >>> ftpd2.serve_forever()  # start both servers
```

....you can now do:

```
 >>> ftpd1 = ftpserver.FTPServer(address1, ftp_handler)
 >>> ftpd2 = ftpserver.FTPServer(address2, ftp_handler)
 >>> ftpserver.FTPServer.serve_forever()  # start both servers
```

## CallEvery class ##

This is the same as pyftpdlib.ftpserver.CallLater exept it keeps calling a scheduled function every X seconds.

## on\_failed\_login() callback ##

A new callback to handle incorrect login has been introduced.  Example usage:

```
from pyftpdlib import ftpserver

class MyHandler(ftpserver.FTPHandler):

    def on_failed_login(self, username, password):
        # do something
        pass


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

## setuptools ##

[setuptools](http://pypi.python.org/pypi/setuptools) is now used in setup.py script.
Users having setuptools installed might want to take advange of it.
Basically I did this in order to be able to use:

```
$ python setup.py develop
```

This lets you edit the source checkout and do development without having to actually install the package:
http://mail.python.org/pipermail/distutils-sig/2005-July/004692.html

## Anti flood demo script ##

A FTP server banning clients in case of commands flood. Check it out here: http://pyftpdlib.googlecode.com/svn/trunk/demo/anti_flood_ftpd.py

## FTPS bug fixes ##

After 0.6.0 was released we discovered FTPS support was quite buggy. It was also suffering a DoS vulnerability (see [issue 191](https://code.google.com/p/pyftpdlib/issues/detail?id=191)).
A different number of issues were fixed ([issue 162](https://code.google.com/p/pyftpdlib/issues/detail?id=162), [issue 166](https://code.google.com/p/pyftpdlib/issues/detail?id=166), [issue 170](https://code.google.com/p/pyftpdlib/issues/detail?id=170), [issue 171](https://code.google.com/p/pyftpdlib/issues/detail?id=171), [issue 191](https://code.google.com/p/pyftpdlib/issues/detail?id=191)).

# Migration notes #

0.7.0 should be fully backward compatible with previous 0.6.0 version with one exception.  If [pysendfile module](http://code.google.com/p/pysendfile/) is installed the data channel will use sendfile() system call by default instead of plain old socket.send() method. This shouldn't make any difference if you send **regular files**. If that's not the case (e.g. you overriden AbstractedFS.open() method so that it returns a file-like object) the data channel won't be able to transmit any data.
Also, use of sendfile() might introduce some unexpected issues with "non regular filesystems" such as NFS, SMBFS/Samba, CIFS and network mounts in general, see: http://www.proftpd.org/docs/howto/Sendfile.html.
If you bump into one this problems the fix consists in disabling sendfile() usage via `FTPHandler.use_sendfile` option:

```
from pyftpdlib import ftpserver
handler = ftpserver.FTPHandler
handler.use_senfile = False
...
```