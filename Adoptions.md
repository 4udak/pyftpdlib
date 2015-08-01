

Here comes a list of softwares and systems using pyftpdlib.
In case you want to add your software to such list add a comment below.
Please help us in keeping such list updated.

# Packages #

Following lists the packages of pyftpdlib from various platforms.

## Debian ##

![http://pyftpdlib.googlecode.com/svn-history/wiki/images/debian.png](http://pyftpdlib.googlecode.com/svn-history/wiki/images/debian.png)

[Debian](http://www.debian.org/) is a famous Linux based operating system.
Debian development team created an [.deb packaged version of pyftpdlib](http://packages.debian.org/sid/python-pyftpdlib) and added it into their official repositories to make users easily install it with:

```
apt-get install python-pyftpdlib
```

Packaged version of pyftpdlib is currently available for the unstable branch only.


---


## Fedora ##

![http://pyftpdlib.googlecode.com/svn-history/wiki/images/fedora.png](http://pyftpdlib.googlecode.com/svn-history/wiki/images/fedora.png)

A [RPM packaged version](https://admin.fedoraproject.org/pkgdb/packages/name/pyftpdlib) is available for Fedora and users can be easily installed it with:

```
yum install pyftpdlib
```


---


## FreeBSD ##

![http://pyftpdlib.googlecode.com/svn-history/wiki/images/freebsd.gif](http://pyftpdlib.googlecode.com/svn-history/wiki/images/freebsd.gif)

[FreeBSD](http://www.freebsd.org/) is an advanced operating system for x86 compatible architectures. It is derived from BSD, the version of UNIX developed at the University of California, Berkeley.
The [FreeBSD Ports Collection](http://www.freebsd.org/ports/) offers a simple way for users and administrators to install applications.

FreeBSD Ports team created a [pyftpdlib port](http://www.freshports.org/ftp/py-pyftpdlib/) to make users can easily install and use it on FreeBSD systems by issuing the commands:

```
cd /usr/ports/ftp/py-pyftpdlib/ && make install clean
pkg_add -r py25-pyftpdlib
```

[Freshports](http://www.freshports.org) is a news site about FreeBSD's ports. Latest news about "py-pyftpdlib" port can be found here:

http://www.freshports.org/ftp/py-pyftpdlib/


---


## GNU Darwin ##

![http://pyftpdlib.googlecode.com/svn-history/wiki/images/gnudarwin.png](http://pyftpdlib.googlecode.com/svn-history/wiki/images/gnudarwin.png)

[GNU Darwin](http://www.gnu-darwin.org) is a Unix distribution which focuses on the porting of free software to Darwin and Mac OS X.
pyftpdlib has been recently included in the official repositories to make users can easily install and use it on GNU Darwin systems.

http://www.gnu-darwin.org


---


# Softwares #

Following lists the softwares adopting pyftpdlib.

## Google Chrome ##

![http://pyftpdlib.googlecode.com/svn-history/wiki/images/chrome.jpg](http://pyftpdlib.googlecode.com/svn-history/wiki/images/chrome.jpg)

[Google Chrome](http://www.google.com/chrome) is the new free and open source web browser developed by Google. [Google Chromium](http://code.google.com/intl/it-IT/chromium/), the open source project behind Google Chrome, included pyftpdlib in the code base to develop Google Chrome's FTP client unit tests.

http://www.google.com/chrome


---


## Smartfile ##

![http://pyftpdlib.googlecode.com/svn-history/wiki/images/smartfile.jpg](http://pyftpdlib.googlecode.com/svn-history/wiki/images/smartfile.jpg)

[Smartfile](http://www.smartfile.com) is a market leader in FTP and online file storage that has a robust and easy-to-use web interface. We utilize pyftpdlib as the underpinnings of our FTP service. Pyftpdlib gives us the flexibility we require to integrate FTP with the rest of our application.

[http://www.smartfile.com](http://www.smartfile.com)


---


## Bazaar ##

![http://pyftpdlib.googlecode.com/svn-history/wiki/images/bazaar.jpg](http://pyftpdlib.googlecode.com/svn-history/wiki/images/bazaar.jpg)

[Bazaar](http://bazaar-vcs.org/) is a distributed version control system similar to Subversion which supports different protocols among which FTP.
As for [Google Chrome](http://www.google.com/chrome), Bazaar recently adopted pyftpdlib as base FTP server to implement internal FTP unit tests.

http://bazaar-vcs.org/


---


## Python for OpenVMS ##

![http://pyftpdlib.googlecode.com/svn-history/wiki/images/pyopenvms.png](http://pyftpdlib.googlecode.com/svn-history/wiki/images/pyopenvms.png)

[OpenVMS](http://h71000.www7.hp.com/index.html?jumpid==/go/openvms) is an operating system that runs on the [VAX](http://en.wikipedia.org/wiki/VAX) and [Alpha](http://en.wikipedia.org/wiki/DEC_Alpha) families of computers, now owned by [Hewlett-Packard](http://en.wikipedia.org/wiki/Hewlett-Packard).
[vmspython](http://www.vmspython.org/) is a porting of the original cPython interpreter that runs on OpenVMS platforms.
pyftpdlib recently became a standard library module installed by default on every new vmspython installation.

http://www.vmspython.org/DownloadAndInstallationPython


---


## OpenERP ##

![http://pyftpdlib.googlecode.com/svn-history/wiki/images/openerp.jpg](http://pyftpdlib.googlecode.com/svn-history/wiki/images/openerp.jpg)

[OpenERP](http://openerp.com) is an Open Source enterprise management software.  It covers and integrates most enterprise needs and processes: accounting, hr, sales, crm, purchase, stock, production, services management, project management, marketing campaign, management by affairs.

OpenERP recently included pyftpdlib as plug in to serve documents via FTP.

http://openerp.com


---


## Plumi ##

![http://pyftpdlib.googlecode.com/svn-history/wiki/images/plumi.jpg](http://pyftpdlib.googlecode.com/svn-history/wiki/images/plumi.jpg)

[Plumi](http://plumi.org/wiki) is a video sharing Content Management System based on [Plone](http://plone.org) that enables you to create your own sophisticated video sharing site.

pyftpdlib has been included in Plumi to allow resumable large video file uploads into [Zope](http://www.zope.org/).

http://plumi.org/wiki
http://blog.plumi.org/


---


## put.io FTP connector ##

![http://pyftpdlib.googlecode.com/svn-history/wiki/images/putio.png](http://pyftpdlib.googlecode.com/svn-history/wiki/images/putio.png)

A proof of concept FTP server that proxies FTP clients requests to [putio](http://put.io/) via HTTP, or in other words an FTP interface to [put.io](http://put.io/). Put.io is a storage service that fetches media files remotely and lets you stream them immediately. More info can be found [here](http://mashable.com/2010/08/25/putio/).

https://github.com/ybrs/putio-ftp-connector <br />
[blog entry](http://ybrs.in/2011/01/27/putio-ftp-connector/)


---


## Rackspace Cloud's FTP ##

![http://pyftpdlib.googlecode.com/svn-history/wiki/images/rackspace-cloud-hosting.jpg](http://pyftpdlib.googlecode.com/svn-history/wiki/images/rackspace-cloud-hosting.jpg)

[ftp-cloudfs](http://github.com/chmouel/ftp-cloudfs) is a ftp server acting as a proxy to Rackspace [Cloud Files](http://www.rackspacecloud.com). It allows you to connect via any FTP client to do upload/download or create containers.

[http://github.com/rackspace/python-cloudfiles](http://github.com/rackspace/python-cloudfiles)

[http://www.rackspacecloud.com](http://www.rackspacecloud.com)


---


## Far Manager ##

![http://pyftpdlib.googlecode.com/svn-history/wiki/images/farmanager.png](http://pyftpdlib.googlecode.com/svn-history/wiki/images/farmanager.png)

[Far Manager](http://farmanager.com/) is a program for managing files and archives in Windows operating systems.

Far Manager recently included pyftpdlib as [plug-in](http://www.farmanager.com/enforum/viewtopic.php?t=640&highlight=&sid=12d4d90f27f421243bcf7a0e3c516efb) for making the current directory accessible through FTP. Convenient for exchanging files with virtual machines.

http://farmanager.com/


---


## Google Pages FTPd ##

![http://pyftpdlib.googlecode.com/svn-history/wiki/images/google-pages.gif](http://pyftpdlib.googlecode.com/svn-history/wiki/images/google-pages.gif)

[gpftpd](http://arkadiusz-wahlig.blogspot.com/2008/04/hosting-files-on-google.html) is a pyftpdlib based FTP server you can connect to using your Google e-mail account.
It redirects you to all files hosted on your [Google Pages](http://pages.google.com) account giving you access to download them and upload new ones.

http://arkadiusz-wahlig.blogspot.com/2008/04/hosting-files-on-google.html


---


## Peerscape ##

![http://pyftpdlib.googlecode.com/svn-history/wiki/images/peerscape.gif](http://pyftpdlib.googlecode.com/svn-history/wiki/images/peerscape.gif)

[Peerscape](http://www.peerscape.org/) is an experimental peer-to-peer social network implemented as an extension to the Firefox web browser. It implements a kind of serverless read-write web supporting third-party AJAX application development. Under the hood, your computer stores copies of your data, the data of your friends and the groups you have joined, and some data about, e.g., friends of friends. It also caches copies of other data that you navigate to. Computers that store the same data establish connections among themselves to keep it in sync.

http://www.peerscape.org/


---


## feitp-server ##

An extra layer on top of pyftpdlib introducing multi processing capabilities and overall higher performances.

http://code.google.com/p/feitp-server/


---


## Symbian Python FTP server ##

![http://pyftpdlib.googlecode.com/svn-history/wiki/images/symbianftp.png](http://pyftpdlib.googlecode.com/svn-history/wiki/images/symbianftp.png)

An FTP server for Symbian OS.

http://code.google.com/p/sypftp/


---


## ftp-cloudfs ##

An FTP server acting as a proxy to Rackspace Cloud Files or to OpenStack Swift. It allow you to connect via any FTP client to do upload/download or create containers.

https://github.com/chmouel/ftp-cloudfs


---


## Sierramobilepos ##

The goal of this project is to extend Openbravo POS to use Windows Mobile Professional or Standard devices. It will import the data from Ob POS (originally in Postgres, later MySql). This data will reside in a database using sqlite3. Later a program will allow to sync by FTP or using a USB cable connected to the WinMob device.

http://forge.openbravo.com/plugins/mwiki/index.php/MobilePOS


---


## Faetus ##

Faetus is an FTP server that translates FTP commands into Amazon S3 API calls providing an FTP interface on top of Amazon S3 storage.

http://tomatohater.com/2010/07/15/faetus-v05-released/


---


## Pyfilesystem ##

Pyfilesystem is a Python module that provides a common interface to many types of filesystem, and provides some powerful features such as exposing filesystems over an internet connection, or to the native filesystem.
It uses pyftpdlib as a backend for testing its FTP component.

http://code.google.com/p/pyfilesystem/


---


## Manent ##

[Manent](http://trac.manent-backup.com/) is an algorithmically strong backup and archival program which can offer remote backup via a pyftpdlib-based S/FTP server.

http://trac.manent-backup.com/


---


## Aksy ##

Aksy is a Python module to control S5000/S6000, Z4/Z8 and MPC4000 Akai sampler models with System Exclusive over USB.  Aksy introduced the possibility to mount samplers as web folders and manage files on the sampler via FTP.

http://walco.n--tree.net/projects/aksy/


---


## Imgserve ##

[Imgserve](http://github.com/wuzhe/imgserve/tree/master) is a python image processing server designed to provide image processing service. It can utilize modern multicore CPU to achieve higher throughput and possibly better performance.

It uses pyftpdlib to permit image downloading/uploading through FTP/FTPS.

http://github.com/wuzhe/imgserve/tree/master


---


## Shareme ##

Ever needed to share a directory between two computers? Usually this is done using NFS, FTP or Samba, which could be a pain to setup when you just want to move some files around.
[Shareme](http://bbs.archlinux.org/viewtopic.php?id=56623) is a small FTP server that, without configuration files or manuals to learn, will publish your directory, and users can download from it and upload files and directory.
Just open a shell and run `shareme -d ~/incoming/` ...and that's it!

http://bbs.archlinux.org/viewtopic.php?id=56623


---


## Zenftp ##

A simple service that bridges an FTP client with zenfolio via SOAP.
Start zenftp.py, providing the name of the target photoset on Zenfolio, and then connect to localhost with your FTP client.

http://code.irondojo.com/


---


## ftpmaster ##

A very simple FTP-based content management system (CMS) including an LDAP authorizer.

https://github.com/MarkLIC/ftpmaster


---


## ShareFTP ##

A program functionally equivalent to Shareme project.

http://git.logfish.net/shareftp.git/


---


## EasyFTPd ##

An end-user UNIX FTP server with focus on simplicity.  It basically provides a configuration file interface over pyftpdlib to easily set up an FTP daemon.

http://code.google.com/p/easyftpd/


---


## Eframe ##

[Eframe](http://code.google.com/p/adqmisc/wiki/eframe) offers Python support for the BT EFrame 1000 digital photo frame.

http://code.google.com/p/adqmisc/wiki/eframe


---


## Fastersync ##

A tool to synchronize data between desktop PCs, laptops, USB drives, remote FTP/SFTP servers, and different online data storages.

http://code.google.com/p/fastersync/

## bftpd ##

A small easy to configure FTP server.

http://bftpd.sourceforge.net/


---


# Web sites using pyftpdlib #

## www.bitsontherun.com ##

![http://pyftpdlib.googlecode.com/svn-history/wiki/images/bitsontherun.png](http://pyftpdlib.googlecode.com/svn-history/wiki/images/bitsontherun.png)

[www.bitsontherun.com](http://www.bitsontherun.com)


---


## www.adcast.tv ##

![http://pyftpdlib.googlecode.com/svn-history/wiki/images/adcast.png](http://pyftpdlib.googlecode.com/svn-history/wiki/images/adcast.png)

[http://www.adcast.tv](http://www.adcast.tv)


---


## www.netplay.it ##

![http://pyftpdlib.googlecode.com/svn/wiki/images/netplay.jpg](http://pyftpdlib.googlecode.com/svn/wiki/images/netplay.jpg)

[www.netplay.it](http://www.netplay.it)