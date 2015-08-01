# Table of contents #



# Introduction #

The **pyftpdlib-inetd** branch contains support for starting **pyftpdlib** from various sorts of service launching processes including the classic Unix **inetd**, the more recent **xinetd** and the **launchd** service offered in Mac OSX and Darwin.  (For the rest of this document, any mention of **inetd** refers to any of **inetd**, **xinetd** or **launchd** except where noted otherwise) Starting your ftp service from **inetd** offers a number of advantages, including but not limited to:

  * **inetd** handles all the forking and process detachment necessary to run your service in the background.
  * **inetd** can arrange for your service to be cleanly re-started if the service dies unexpectedly.
  * Privileged ports can be opened by **inetd**, which runs as the superuser, before your service is started allowing your service to by run as a less privileged user.
  * **inetd** offers convenient management of the resources and environment available to your service.

The sections below detail how to implement various sorts of service that will be launched by **inetd**.  They assume that you are familiar with the general structure of **pyftpdlib** and how it is used.

# Starting services under inetd #

Services started by inetd can be run in one of two ways:
  * **no\_wait** services are run once for each incoming connection. The service is passed an open socket for the accepted connection from the client.
  * **wait** services are more like traditional servers and are responsible for listening for and accepting new client connections. The service is passed one bound, listening socket for the service.
**pyftpdlib** can be run in either of these modes. Which one is appropriate depends on your specific circumstances.

# Running pyftpdlib from inetd #

As with running pyftpdlib as a stand-alone server, when running under inetd it can either be run as a limited, anonymous server using a simple command line or more complex configurations can be created by writing a small python program.

## Simple examples ##

### Simple **no-wait** FTP server from the command line ###

A simple, anonymous FTP server run once per new connection can be started from inetd by giving inetd the command line:

```
python -m pyftpdlib.ftpserver --inetd
```

This will serve the contents of the current working directory. The other standard command line options can be used to provide read/write access, use different passive port ranges etc.

### Simple **wait** FTP service from the command line ###

A simple, anonymous FTP server started by inetd and left to run indefinately by passing inetd the command line:

```
python -m pyftpdlib.ftpserver --inetd-wait
```

As before, other command line options my be used to adjust the configuration. Additionally, it is often useful for services started by inetd to exit if they are idle for a period of time, in order to save resources.  By using the command line:

```
python -m pyftpdlib.ftpserver --inetd-wait --idle-timeout=600
```

if the FTP server does not have any active client connection for ten minutes (600 seconds) then the server will exit cleanly. The inetd process will then continue to listen on the FTP server's behalf and restart the service as needed.

## Advanced examples ##

When inetd starts a service in the 'no-wait' mode the new process is passed the connected socket as both stdin and stdout. In order to use pyftpdlib in this mode we must:
  * Ensure that no error output will be sent to stdout, since this will go back to the client
  * Construct a shell FTPServer object without any listening socket
  * Construct an FTPHandler object based on the socket that was passed to the process
  * Instruct the shell FTPServer to serve the FTP protocol

In order to assist with the construction of a python socket object from the python file wrapper that wraps the stdin file descriptor, the ftpserver module provides a utility function, `socket_for_file()`, to handle the details of this process.

Below is an example of how this might be done:

```
#!/usr/bin/env python
   
"""A basic FTP server started from inetd as a 'no-wait' service
"""
   
import os
from pyftpdlib import ftpserver
   
if __name__ == "__main__":
   
    # Set up a dummy authorizer for some users
    authorizer = ftpserver.DummyAuthorizer()
    authorizer.add_user('user', '12345', os.getcwd(), perm='elradfmw')
    authorizer.add_anonymous(os.getcwd())
   
    # Instantiate FTP handler class
    ftp_handler = ftpserver.FTPHandler
    ftp_handler.authorizer = authorizer
   
    # Ensure any logging goes to the error stream, not back to the client
    sys.stdout = sys.stderr

    # Construct an FTPServer object without a socket of its own
    ftpd = FTPServer(None, FTPHandler)

    # Construct a handler for the socket passed as stdin
    ftp_connection = ftp_handler(ftpserver.socket_for_file(sys.stdin),ftpd)

    # Start the handling of the connection
    ftp_connection.handle()

    # Start ftp server. This will exit when the client disconnects.
    ftpd.serve_forever()
```

When inetd starts a service in the 'wait' mode, rather than receiving a connected socket the new process is handed the listening server socket as both stdin and stdout. In order to use pyftpdlib in this mode we must:
  * Ensure that no error output will be sent to stdout, since this is a listening socket and writing to it is an error
  * Construct a shell FTPServer object without any listening socket
  * Set the FTPServer listening socket to be the socket passed as stdin
  * Instruct the shell FTPServer to serve the FTP protocol with a suitable idle timeout.

Below is an example of how this might be done:

```
#!/usr/bin/env python
   
"""A basic FTP server started from inetd as a 'wait' service
"""

import os
from pyftpdlib import ftpserver
   
if __name__ == "__main__":
   
    # Set up a dummy authorizer for some users
    authorizer = ftpserver.DummyAuthorizer()
    authorizer.add_user('user', '12345', os.getcwd(), perm='elradfmw')
    authorizer.add_anonymous(os.getcwd())
   
    # Instantiate FTP handler class
    ftp_handler = ftpserver.FTPHandler
    ftp_handler.authorizer = authorizer
   
    # Ensure any logging goes to the error stream, not to the server socket
    sys.stdout = sys.stderr

    # Construct an FTPServer object without a socket of its own
    ftpd = ftpserver.FTPServer(None, FTPHandler)

    # Set the server socket to be the socket passed as stdin
    ftpd.set_server_socket(ftpserver.socket_for_file(sys.stdin))

    # Start ftp server, but exit if idle for ten minutes.
    ftpd.serve_forever(idle_timeout=600)
```

# Sample service configurations #

In general you should refer to your local man pages for the specifics of your service configuration. These examples are offered as a rough guide.

## inetd examples ##

To run pyftpdlib in no-wait mode, as the unprivileged user 'nobody', from the original inetd you will need to add a line to the inetd.conf file like this:
```
ftp          stream  tcp   nowait nobody  /usr/bin/python        python -m pyftpdlib.ftpserver -I
```

Starting pyftpd in wait mode the line would look like this:
```
ftp          stream  tcp   wait nobody  /usr/bin/python        python -m pyftpdlib.ftpserver -W -t 600
```

## xinetd examples ##

The xinetd service provides similar functionality to the traditional inetd but offers a great deal more flexibility and control. Services can be independently enabled and disabled, resource constraints can be applied to sub-processes and it can even control from which IP addresses connections will be accepted.

Here is an example of how to run pyftpdlib in the no-wait mode, as the user nobody, from xinetd:

```
service ftp
{
        disable = no
        socket_type = stream
        protocol    = tcp
        wait        = no
        user        = nobody
        group     = nobody
        server      = /usr/bin/python
        server_args = python -m pyftpdlib.ftpserver -I
}
```

Here is the similar configuration running in the wait mode:

```
service ftp
{
        disable = no
        socket_type = stream
        protocol    = tcp
        wait        = yes
        user        = nobody
        group     = nobody
        server      = /usr/bin/python
        server_args = python -m pyftpdlib.ftpserver -W -t 600
}
```


## launchd examples ##

The launchd service subsumes the functionality of a number of different Unix processes, including (x)inetd. Configuration files for launchd are a little less readable in the raw than for xinetd since they are XML property list files. They do however offer a great deal of flexibility in terms of configuring the services. Here is an example a plist file that instructs launchd to run the ftp service as a no-wait service. The process will be started with a user ID of _nobody_, the stderr output will be directed to `/var/log/ftpd.log`, the current working directory of the new process will be `/tmp/ftpstuff` and the service will be advertised over Bonjour multicast DNS service discovery mechanism.  This description file would typically be saved as, e.g. `ftpd.plist` and activated by running the command `launchctl load ftpd.plist`. Alternatively, on Mac OS X the service description could be saved in /Library/LaunchDaemons (with file ownership set to root:wheel) and the service will be loaded at boot time.

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
        <key>StandardErrorPath</key>
        <string>/var/log/ftpd.log</string>
        <key>Disabled</key>
        <false/>
        <key>UserName</key>
        <string>nobody</string>
        <key>WorkingDirectory</key>
        <string>/tmp/ftpdstuff</string>
        <key>Label</key>
        <string>org.python.pyftpd</string>
        <key>Program</key>
        <string>/usr/bin/python</string>
        <key>ProgramArguments</key>
        <array>
                <string>python</string>
                <string>-m</string>
                <string>pyftpdlib.ftpserver</string>
                <string>-I</string>
        </array>
        <key>inetdCompatibility</key>
        <dict>
                <key>Wait</key>
                <false/>
        </dict>
        <key>Sockets</key>
        <dict>
                <key>Listeners</key>
                <dict>
                        <key>SockServiceName</key>
                        <string>ftp</string>
                        <key>Bonjour</key>
                        <true/>
                </dict>
        </dict>
</dict>
</plist>
```