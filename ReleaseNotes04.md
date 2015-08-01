# Version: 0.4.0 - Date: 2008-05-16 #

## Major enhancements ##

  * [Issue 65](https://code.google.com/p/pyftpdlib/issues/detail?id=65): it is now possible to assume the id of real users when using system dependent authorizers.
  * [Issue 67](https://code.google.com/p/pyftpdlib/issues/detail?id=67): added IPv6 support ([RFC-2428](http://www.faqs.org/rfcs/rfc2428.html)).

## Bugfixes ##

  * [Issue 64](https://code.google.com/p/pyftpdlib/issues/detail?id=64): issue when authenticating as anonymous user using user-defined authorizers.
  * [Issue 66](https://code.google.com/p/pyftpdlib/issues/detail?id=66): WinNTAuthorizer does not determine the real user home directory.
  * [Issue 69](https://code.google.com/p/pyftpdlib/issues/detail?id=69): DummyAuthorizer incorrectly uses class attribute instead of instance attribute for user\_table dictionary.
  * [Issue 70](https://code.google.com/p/pyftpdlib/issues/detail?id=70): wrong NOOP response code.

## API changes since 0.3.0 ##

  * `impersonate_user()` and `terminate_impersonation()` methods have been added to the `DummyAuthorizer` class.

## Migration notes ##

Changes applied to the 0.4.0 trunk are fully compatible with the previous 0.3.0 version. Your existing 0.3.0 based code will most likely work without need to be modified.
The new features in this release are detailed below.

### IPv6 ###

Starting from version 0.4.0 pyftpdlib supports IPv6 ([RFC-2428](http://www.faqs.org/rfcs/rfc2428.html)). If you use IPv6 and want your FTP server to do so just pass a valid IPv6 address to the `FTPServer` class constructor which now accepts both IPv4 and IPv6 address types. Example:

```
>>> from pyftpdlib import ftpserver
>>> address = ("::1", 21)  # listen on localhost, port 21
>>> ftpd = ftpserver.FTPServer(address, ftpserver.FTPHandler)
>>> ftpd.serve_forever()
Serving FTP on ::1:21
```

### Real users impersonation ###

Two new methods have been added to the original `DummyAuthorizer` class: `impersonate_user()` and `terminate_impersonation()`.

System dependent authorizers subclassing the dummy authorizer can now assume the id of real users by overriding them as necessary.

Every time the FTP server is going to access the filesystem (e.g. for creating or renaming a file) it will temporarily impersonate the currently logged on user, execute the filesystem call and then switch back to the user who originally started the server.

Example UNIX and Windows FTP servers contained in the [demo directory](http://code.google.com/p/pyftpdlib/source/browse/tags/release-0.4.0/demo/) implement real user impersonation.