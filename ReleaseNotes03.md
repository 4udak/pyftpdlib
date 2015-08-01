# Version: 0.3.0 - Date: 2008-01-17 #

## Major enhancements ##

  * [Issue 48](https://code.google.com/p/pyftpdlib/issues/detail?id=48): real permissions, owner, and group for files on UNIX platforms are now provided when processing LIST.
  * [Issue 51](https://code.google.com/p/pyftpdlib/issues/detail?id=51): added the new `demo/throttled_ftpd.py` script.
  * [Issue 59](https://code.google.com/p/pyftpdlib/issues/detail?id=59): iterators are now used for calculating requests requiring long time to complete (LIST and MLSD commands).
  * [Issue 61](https://code.google.com/p/pyftpdlib/issues/detail?id=61): extended the set of assignable user permissions.

## RFC-related enhancements ##

  * [Issue 42](https://code.google.com/p/pyftpdlib/issues/detail?id=42): implemented **FEAT** command defined in [RFC-2389](http://www.faqs.org/rfcs/rfc2389.html).
  * [Issue 52](https://code.google.com/p/pyftpdlib/issues/detail?id=52): implemented **MLST** and **MLSD** commands defined in [RFC-3659](http://www.faqs.org/rfcs/rfc3659.html).
  * [Issue 58](https://code.google.com/p/pyftpdlib/issues/detail?id=58): implemented **OPTS** command define in [RFC-2389](http://www.faqs.org/rfcs/rfc2389.html).

## Bugfixes ##

  * [Issue 41](https://code.google.com/p/pyftpdlib/issues/detail?id=41): error occurred on quit if user was not yet authenticated.
  * [Issue 43](https://code.google.com/p/pyftpdlib/issues/detail?id=43): hidden the server identifier when returning STAT response.
  * [Issue 44](https://code.google.com/p/pyftpdlib/issues/detail?id=44): a wrong response code was given on PORT if the data connection attempt failed.
  * [Issue 45](https://code.google.com/p/pyftpdlib/issues/detail?id=45): a wrong response code was given on HELP if argument was incorrect.
  * [Issue 46](https://code.google.com/p/pyftpdlib/issues/detail?id=46): a wrong response code was given on PASV if remote peer had a foreign internet address.
  * [Issue 47](https://code.google.com/p/pyftpdlib/issues/detail?id=47): can't use FTPServer.max\_cons option with Python 2.3.
  * [Issue 48](https://code.google.com/p/pyftpdlib/issues/detail?id=48): problem when LISTing "broken" symbolic links.
  * [Issue 49](https://code.google.com/p/pyftpdlib/issues/detail?id=49): data channel did not respect the outgoing data buffer.
  * [Issue 53](https://code.google.com/p/pyftpdlib/issues/detail?id=53): received strings having trailing white spaces was erroneously stripped.
  * [Issue 54](https://code.google.com/p/pyftpdlib/issues/detail?id=54): LIST/NLST/STAT outputs are now sorted by file name.
  * [Issue 55](https://code.google.com/p/pyftpdlib/issues/detail?id=55): path traversal vulnerability in case of symlinks.
  * [Issue 56](https://code.google.com/p/pyftpdlib/issues/detail?id=56): can't rename broken symbolic links.
  * [Issue 57](https://code.google.com/p/pyftpdlib/issues/detail?id=57): wrong LIST/NLST behavior when processing symbolic links.
  * [Issue 60](https://code.google.com/p/pyftpdlib/issues/detail?id=60): error occurred in case of bad formatted PORT command requests.

## API changes since 0.2.0 ##

  * New `IteratorProducer` and `BufferedIteratorProducer` classes have been added.
  * `DummyAuthorizer` class changes:
    * The permissions management has been changed and the set of available permissions have been extended (see [Issue #61](https://code.google.com/p/pyftpdlib/issues/detail?id=#61)). `add_user()` method now accepts "eladfm" permissions beyond the old "r" and "w".
    * `r_perm()` and `w_perm()` methods have been removed.
    * New `has_perm()` and `get_perms()` methods have been added.
  * AbstractedFS class changes:
    * `normalize()` method has been renamed in `ftpnorm()`.
    * `translate()` method has been renamed in `ftp2fs()`.
    * New methods: `fs2ftp()`, `stat()`, `lstat()`, `islink()`, `realpath()`, `lexists()`, `validpath()`.
    * `get_list_dir()`, `get_stat_dir()` and `format_list()` methods now return an iterator object instead of a string.
    * `format_list()` method has a new "ignore\_err" keyword argument.
  * global `debug()` function has been removed.

## Porting to pyftpdlib 0.3.0 ##

This section lists previously described changes that may require changes to your existent 0.2.0-based code.

The main news in 0.3.0 version is that the `DummyAuthorizer` class now provides an extended set of assignable user permissions. For example: to create a user having full r/w permissions you no longer have to do:

```
authorizer.add_user("user", "12345", "/home/user", perm=("r","w"))
```

...or:

```
authorizer.add_user("user", "12345", "/home/user", perm="rw")
```

...but instead:

```
authorizer.add_user("user", "12345", "/home/user", perm="elradfmw")
```


Folks who sub-classed `DummyAuthorizer` class to create their own custom authorizer (e.g. I've seen some SQL-based authorizers around) must note that `r_perm()` and `w_perm()` methods have been removed and replaced by the new `has_perm()` method.

Other changes are fundamentally related to `AbstractedFS` class which acquired some new methods and renamed others but this should interest only folks who sub-classed it (e.g. [Aksy library](http://walco.n--tree.net/projects/aksy/wiki) which implemented a virtual file-system).