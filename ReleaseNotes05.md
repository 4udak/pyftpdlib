# Version: 0.5.2 - Date: 2009-09-14 #

## Enhancements ##

  * [Issue 103](https://code.google.com/p/pyftpdlib/issues/detail?id=103): added new [demo/unix\_ftpd.py](http://code.google.com/p/pyftpdlib/source/browse/trunk/demo/unix_daemon.py) daemon script.
  * [Issue 108](https://code.google.com/p/pyftpdlib/issues/detail?id=108): added native data channel bandwidth throttling (new ThrottledDTPHandler class).

## Bugfixes ##

  * [Issue 100](https://code.google.com/p/pyftpdlib/issues/detail?id=100): fixed a race condition in FTPHandler constructor which could throw an exception in case of connection bashing (DoS).  (thanks Bram Neijt)
  * [Issue 102](https://code.google.com/p/pyftpdlib/issues/detail?id=102): FTPServer.close\_all() now removes any unfired delayed call left behind to prevent potential memory leaks.
  * [Issue 104](https://code.google.com/p/pyftpdlib/issues/detail?id=104): fixed a bug in FTPServer.handle\_accept() where socket.accept() could return None instead of a valid address causing the server to crash. (OS X only, reported by Wentao Han)
  * [Issue 104](https://code.google.com/p/pyftpdlib/issues/detail?id=104): an unhandled EPIPE exception might be thrown by asyncore.recv() when dealing with ill-behaved clients on OS X . (reported by Wentao Han)
  * [Issue 105](https://code.google.com/p/pyftpdlib/issues/detail?id=105): ECONNABORTED might be thrown by socket.accept() on FreeBSD causing the server to crash.
  * [Issue 109](https://code.google.com/p/pyftpdlib/issues/detail?id=109): an unhandled EBADF exception might be thrown when using poll() on OS X and FreeBSD.
  * [Issue 111](https://code.google.com/p/pyftpdlib/issues/detail?id=111): the license used was not MIT as stated in source files.
  * [Issue 112](https://code.google.com/p/pyftpdlib/issues/detail?id=112): fixed a MDTM related test case failure occurring on 64 bit OSes.
  * [Issue 113](https://code.google.com/p/pyftpdlib/issues/detail?id=113): fixed unix\_ftp.py which was treating anonymous as a normal user.
  * [Issue 114](https://code.google.com/p/pyftpdlib/issues/detail?id=114): MLST is now denied unless the "l" permission has been specified for the user.
  * [Issue 115](https://code.google.com/p/pyftpdlib/issues/detail?id=115): asyncore.dispatcher.close() is now called before doing any other cleanup operation when client disconnects. This way we avoid an endless loop which hangs the server in case an exception is raised in close() method. (thanks Arkadiusz Wahlig)
  * [Issue 116](https://code.google.com/p/pyftpdlib/issues/detail?id=116): extra carriage returns were added to files transferred in ASCII mode.
  * [Issue 118](https://code.google.com/p/pyftpdlib/issues/detail?id=118): CDUP always changes to "/".
  * [Issue 119](https://code.google.com/p/pyftpdlib/issues/detail?id=119): QUIT sent during a transfer caused a memory leak.

## API changes since 0.5.1 ##

  * `ThrottledDTPHandler` class has been added (see [usage example](http://code.google.com/p/pyftpdlib/source/browse/trunk/demo/throttled_ftpd.py)).
  * `FTPHandler.process_command()` method has been added.


---


# Version: 0.5.1 - Date: 2009-01-21 #

## Enhancements ##

  * [Issue 79](https://code.google.com/p/pyftpdlib/issues/detail?id=79): added two new callback methods to `FTPHandler` class to handle `on_file_sent` and `on_file_received` events.
  * [Issue 82](https://code.google.com/p/pyftpdlib/issues/detail?id=82): added table of contents in documentation.
  * [Issue 92](https://code.google.com/p/pyftpdlib/issues/detail?id=92): ASCII transfers are now 200% faster on those systems using "\r\n" as line separator (typically Windows).
  * [Issue 94](https://code.google.com/p/pyftpdlib/issues/detail?id=94): a bigger buffer size for `send()` and `recv()` has been set resulting in a considerable speedup (about 40% faster) for both incoming and outgoing data transfers.
  * [Issue 98](https://code.google.com/p/pyftpdlib/issues/detail?id=98): added preliminary support for SITE command.
  * [Issue 99](https://code.google.com/p/pyftpdlib/issues/detail?id=99): a new script implementing FTPS (FTP over TLS/SSL) has been added to the demo directory ([link](http://code.google.com/p/pyftpdlib/source/browse/trunk/demo/tls_ftpd.py)).

## Bugfixes ##

  * [Issue 78](https://code.google.com/p/pyftpdlib/issues/detail?id=78): the idle timeout of passive data connections gets stopped in case of rejected "site-to-site" connections.
  * [Issue 80](https://code.google.com/p/pyftpdlib/issues/detail?id=80): demo/md5\_ftpd.py should use hashlib module instead of the deprecated md5 module.
  * [Issue 81](https://code.google.com/p/pyftpdlib/issues/detail?id=81): fixed some tests which were failing on SunOS.
  * [Issue 84](https://code.google.com/p/pyftpdlib/issues/detail?id=84): fixed a very rare unhandled exception which could occur when retrieving the first bytes of a corrupted file.
  * [Issue 85](https://code.google.com/p/pyftpdlib/issues/detail?id=85): a positive MKD response is supposed to include the name of the new directory.
  * [Issue 87](https://code.google.com/p/pyftpdlib/issues/detail?id=87): SIZE should be rejected when the current TYPE is ASCII.
  * [Issue 88](https://code.google.com/p/pyftpdlib/issues/detail?id=88): REST should be rejected when the current TYPE is ASCII.
  * [Issue 89](https://code.google.com/p/pyftpdlib/issues/detail?id=89): "TYPE AN" was erroneously treated as synonym for "TYPE A" when "TYPE L7" should have been used instead.
  * [Issue 90](https://code.google.com/p/pyftpdlib/issues/detail?id=90): an unhandled exception can occur when using MDTM against a file modified before year 1900.
  * [Issue 91](https://code.google.com/p/pyftpdlib/issues/detail?id=91): an unhandled exception can occur in case `accept()` returns None instead of a socket (it happens sometimes).
  * [Issue 95](https://code.google.com/p/pyftpdlib/issues/detail?id=95): anonymous is now treated as any other case-sensitive user.

## API changes since 0.5.0 ##

  * `FTPHandler` gained a new `_extra_feats` private attribute.
  * `FTPHandler` gained two new methods: `on_file_sent` and `on_file_received`.


---


# Version: 0.5.0 - Date: 2008-09-20 #

## Enhancements ##

  * [Issue 72](https://code.google.com/p/pyftpdlib/issues/detail?id=72): pyftpdlib now provides configurable idle timeouts to disconnect client after a long time of inactivity.
  * [Issue 73](https://code.google.com/p/pyftpdlib/issues/detail?id=73): impose a delay before replying for invalid credentials to minimize the risk of brute force password guessing.
  * [Issue 74](https://code.google.com/p/pyftpdlib/issues/detail?id=74): it is now possible to define permission exceptions for certain directories (e.g. creating a user which does not have write permission except for one sub-directory in FTP root).
  * Improved bandwidth throttling capabilities of [demo/throttled\_ftpd.py](http://code.google.com/p/pyftpdlib/source/browse/tags/release-0.5.0/demo/throttled_ftpd.py) script by having used the new `CallLater` class which drastically reduces the number of calls to time.time().

## Bugfixes ##

  * [Issue 62](https://code.google.com/p/pyftpdlib/issues/detail?id=62): some unit tests were failing on dual core machines.
  * [Issue 71](https://code.google.com/p/pyftpdlib/issues/detail?id=71): socket handles are leaked when a data transfer is in progress and user QUITs.
  * [Issue 75](https://code.google.com/p/pyftpdlib/issues/detail?id=75): orphaned file was left behind in case STOU failed for insufficient user permissions.
  * [Issue 77](https://code.google.com/p/pyftpdlib/issues/detail?id=77): incorrect OOB data management on FreeBSD.

## API changes since 0.4.0 ##

  * `FTPHandler`, `DTPHandler`, `PassiveDTP` and `ActiveDTP` classes gained a new `timeout` class attribute.
  * `DummyAuthorizer` class gained a new `override_perm` method.
  * A new class called `CallLater` has been added.
  * `AbstractedFS.get_stat_dir` method has been removed.

## Migration notes ##

Changes applied to the 0.5.0 trunk should be fully compatible with the previous 0.4.0 version. Your existing 0.4.0 based code will most likely work without need to be modified. The new features in this release are detailed below.

### Idle timeouts and new CallLater class ###

The previous version suffered the problem of not having a mechanism to disconnect clients after a long time of inactivity.
This posed the risk for the FTP server to be easily vulnerable to DoS attacks in which a lot of connected clients could clump system's resources and sockets.

0.5.0 version solved this problem by implementing a brand new polling loop which, other than serving the connected clients, also checks if it is the proper time for scheduled functions to be called (if any). Thanks to the new loop and the new `CallLater` class implementing [timeouts](http://code.google.com/p/pyftpdlib/issues/detail?id=72) and [delays to invalid credential replies](http://code.google.com/p/pyftpdlib/issues/detail?id=73) have been possible.

`FTPHandler` class gained a new `timeout` attribute defaulting to `300` seconds which is the maximum time of inactivity a remote client may spend before being disconnected.

Also `DTPHandler` class gained a new `timeout` attribute defaulting to `300` seconds which roughly is the maximum time the data transfers can stall for with no progress.

### Permission exceptions ###

The `DummyAuthorizer` now gives the possibility to define permission exceptions for directories.
For example, if you want to create a user which does not have write permission except for one sub-directory in FTP root, you can now do as follows:

```
>>> from pyftpdlib import ftpserver
>>> authorizer = ftpserver.DummyAuthorizer()
>>> authorizer.add_user('user', 'password', '/home/ftp', perm='elr')
>>> authorizer.override_perm('user', '/home/ftp/pub', 'elradfmw', recursive=True)
```