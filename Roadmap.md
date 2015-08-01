# Releases roadmap #

## Possible ideas for future versions ##

  * Multi processes support ([issue 148](http://code.google.com/p/pyftpdlib/issues/detail?id=148)).
  * Include a graphical Tk-based ftp server into demo directory.
  * MODE Z ([link)](http://tools.ietf.org/html/draft-preston-ftpext-deflate-04)
  * Impicit SSL
  * CCC command support ([issue 172](http://code.google.com/p/pyftpdlib/issues/detail?id=172))
  * x/inetd support ([issue 147](http://code.google.com/p/pyftpdlib/issues/detail?id=147)).
  * file based configuration
  * _(?)_ Add the possibility to run the server as a daemon at library level.
  * _(?)_ **LANG** command support ([RFC 2640](http://www.faqs.org/rfcs/rfc2640.html)).
  * _(?)_ Support for **ADAT**, **MIC**, **CONF** and **ENC** commands ([RFC-2228](http://www.ietf.org/rfc/rfc2228.txt)).

## 1.0.0 _(current release)_ ##

  * Python 3 porting ([issue 76](http://code.google.com/p/pyftpdlib/issues/detail?id=76)).
  * Unicode support.
  * rewritten IO loop
  * ThreadedFTPServer
  * MultiprocessFTPServer
  * split ftpserver.py into submodules.
  * use stdlib logging module

## 0.7.0 ##

  * Use of sendfile(2) for uploads (from server to client).
  * Faster scheduler.
  * SITE CHMOD.
  * CallEvery class.
  * on\_login\_failed() callback.
  * Fix some FTPS outstanding bugs.
  * Benchmark script.

## 0.6.0 ##

  * Command line parameters.
  * Add SSL/TLS support at library level (pyftpdlib.contrib package).
  * Make Unix and Windows authorizers be part of the library (pyftpdlib.contrib package).
  * Use GMT times.
  * Standardized logging.
  * Callbacks for incomplete transfers.
  * Server both IPv4 and IPv6 by using a single socket.
  * Enable TCP\_NODELAY socket option (2x speedup).
  * Unix filesystem.

## 0.5.2 ##

  * Set transfer speed limits thanks to addition of ThrottledDTPHandler class.
  * Add unix\_daemon.py daemon script.

## 0.5.1 ##

  * Add a new demo script implementing FTPS (FTP over TLS/SSL) supporting **AUTH**, **PROT** and **PBSZ** commands.
  * Set bigger buffer sizes to speedup data transfers.
  * Speedup ASCII data transfers on Windows.
  * Add preliminary support for SITE command.
  * Fix various bugs.

## 0.5.0 ##

  * Provide configurable idle timeouts to disconnect clients after a long time of inactivity.
  * Minimize the risk of brute force password guessing by imposing a delay before replying to invalid credentials.
  * Adds to the DummyAuthorizer the possibility to create permission overriding for directories (e.g. define a user which does not have write permission except for one sub-directory in FTP root).

## 0.4.0 ##

  * Assume the id of real users when using system dependent authorizers.
  * Add IPv6 support including **EPRT** & **EPSV** commands as defined in [RFC 2428](http://www.faqs.org/rfcs/rfc2428.html).

## 0.3.0 ##

  * Extend the set of assignable user permissions aside from the generic "read" and "write" (e.g. create/remove/rename/list/change directory, append to existing file and so on...).
  * Implement **MLST** and **MLSD** commands defined in [RFC-3659](http://www.ietf.org/rfc/rfc3659.txt).
  * Implement **FEAT** and **OPTS** commands defined in [RFC 2389](http://www.faqs.org/rfcs/rfc2389.html).
  * Use iterators for calculating requests requiring long time to complete (**LIST** and **MLSD** commands).
  * Provide real permissions, owner, and group for files on UNIX platforms processing the **LIST** command.

## 0.2.0 ##

  * Support for **FXP**, site-to-site transfers.
  * NAT/Firewall support with **PASV** (passive) mode connections.
  * Configurable range of ports to use for passive data transfers.
  * Per-user messages configurability.
  * Maximum connections limit.
  * Per-source-IP limits.
  * Maximum login attempts limit.
  * Change account when **USER** is provided a second time after authenticating.
  * Accept **HELP** command arguments.
  * Implement directory listing over the command channel for **STAT** command provided with argument.

## 0.1.1 ##

  * Randomize **PASV** port selection.
  * Release pyftpdlib under a license the most freely possible (MIT).

## 0.1.0 ##

  * Create an asynchronous FTP server library compliant with [RFC 959](http://www.faqs.org/rfcs/rfc959.html).