# Table of contents #



# 1.0 - Introduction #

pyftpdlib implements the server side of the FTP protocol as defined in [RFC-959](http://www.faqs.org/rfcs/rfc959.html).  pyftpdlib consist of a single file, [ftpserver.py](http://code.google.com/p/pyftpdlib/source/browse/trunk/pyftpdlib/ftpserver.py), which contains a hierarchy of classes which implement the backend functionality for the ftpd. <br> This document is intended to serve as a simple <a href='http://code.google.com/p/billiejoex/wiki/Tutorial#2.0_-_API_reference'>API reference</a> of most important classes and functions.  Also included is an introduction to <a href='http://code.google.com/p/billiejoex/wiki/Tutorial#3.0_-_Customizing_your_FTP_server'>customization</a> through the use of some example scripts.<br>
Some of them are included in <a href='http://code.google.com/p/pyftpdlib/source/browse/#svn/trunk/demo'>demo</a> directory of pyftpdlib source distribution.<br>
<br>
If you have written a customized configuration you think could be useful to the community feel free to share it by adding a comment at the end of this document.<br>
<br>
<h1>2.0 - API reference</h1>

<h2>2.1 - Logging</h2>

<i>function</i> pyftpdlib.ftpserver.<b>log</b><font size='3'><b><code>(</code></b></font><i>msg</i><font size='3'><b><code>)</code></b></font>
<blockquote>Log messages intended for the end user.<br>
<hr />
<i>function</i> pyftpdlib.ftpserver.<b>logline</b><font size='3'><b><code>(</code></b></font><i>msg</i><font size='3'><b><code>)</code></b></font>
Log commands and responses passing through the command channel.<br>
<hr />
<i>function</i> pyftpdlib.ftpserver.<b>logerror</b><font size='3'><b><code>(</code></b></font><i>msg</i><font size='3'><b><code>)</code></b></font>
Log traceback outputs occurring in case of errors.<br>
<hr /></blockquote>

<h2>2.2 - Users</h2>

<i>class</i> pyftpdlib.ftpserver.<b>DummyAuthorizer</b><font size='3'><b><code>()</code></b></font>

<blockquote>Basic "dummy" authorizer class, suitable for subclassing to create your own custom authorizers. An "authorizer" is a class handling authentications and permissions of the FTP server.  It is used inside <code>FTPHandler</code> class for verifying user's password, getting users home directory, checking user permissions when a filesystem read/write event occurs and changing user before accessing the filesystem. <code>DummyAuthorizer</code> is the base authorizer, providing a platform independent interface for managing "virtual" FTP users.  Typically the first thing you have to do is create an instance of this class and start adding ftp users:</blockquote>

<pre><code>&gt;&gt;&gt; from pyftpdlib import ftpserver<br>
&gt;&gt;&gt; authorizer = ftpserver.DummyAuthorizer()<br>
&gt;&gt;&gt; authorizer.add_user('user', 'password', '/home/user', perm='elradfmw')<br>
&gt;&gt;&gt; authorizer.add_anonymous('/home/nobody')<br>
</code></pre>

<ul><li><b>add_user</b><font size='3'><b><code>(</code></b></font><i>username</i>, <i>password</i>, <i>homedir</i><font size='3'><b><code>[</code></b></font>, <i>perm="elr"</i><font size='3'><b><code>[</code></b></font>, <i>msg_login="Login successful."</i><font size='3'><b><code>[</code></b></font>, <i>msg_quit="Goodbye."</i> <font size='3'><b><code>]]])</code></b></font>      <br>Add a user to the virtual users table.  <code>AuthorizerError</code> exceptions raised on error conditions such as insufficient permissions or duplicate usernames.  Optional <code>perm</code> argument is a set of letters referencing the user's permissions.  Every letter is used to indicate that the access rights the current FTP user has over the following specific actions are granted.  The available permissions are the following listed below:</li></ul>

<blockquote>Read permissions:<br>
<ul><li><b>"e"</b> = change directory (CWD, CDUP commands)<br>
</li><li><b>"l"</b> = list files (LIST, NLST, STAT, MLSD, MLST, SIZE commands)<br>
</li><li><b>"r"</b> = retrieve file from the server (RETR command)<br>
</li></ul>Write permissions<br>
<ul><li><b>"a"</b> = append data to an existing file (APPE command)<br>
</li><li><b>"d"</b> = delete file or directory (DELE, RMD commands)<br>
</li><li><b>"f"</b> = rename file or directory (RNFR, RNTO commands)<br>
</li><li><b>"m"</b> = create directory (MKD command)<br>
</li><li><b>"w"</b> = store a file to the server (STOR, STOU commands)<br>
</li><li><b>"M"</b> = change mode/permission (SITE CHMOD command)  <i><b>New in 0.7.0</b></i></li></ul></blockquote>

<blockquote>Optional <code>msg_login</code> and <code>msg_quit</code> arguments can be specified to provide customized response strings when user log-in and quit.  The <code>perm</code> argument of the <code>add_user()</code> method refers to user's permissions. Every letter is used to indicate that the access rights the current FTP user has over the following specific actions are granted.</blockquote>

<ul><li><b>add_anonymous</b><font size='3'><b><code>(</code></b></font><i>homedir</i><font size='3'><b><code>[</code></b></font>, <code>**</code><i>kwargs</i><font size='3'><b><code>])</code></b></font><br>Add an anonymous user to the virtual users table.  <code>AuthorizerError</code> exception raised on error conditions such as insufficient permissions, missing home directory, or duplicate anonymous users.  The keyword arguments in <code>kwargs</code> are the same expected by <code>add_user()</code> method: <code>perm</code>, <code>msg_login</code> and <code>msg_quit</code>.  The optional <i>perm</i> keyword argument is a string defaulting to <code>"elr"</code> referencing "read-only" anonymous user's permission.  Using a "write" value results in a <code>RuntimeWarning</code>.</li></ul>

<ul><li><b>override_perm</b><font size='3'><b><code>(</code></b></font><i>username</i>, <i>directory</i>, <i>perm</i><font size='3'><b><code>[</code></b></font>, <i>recursive=False</i><font size='3'><b><code>])</code></b></font><br>Override user permissions for a given directory.</li></ul>

<ul><li><b>validate_authentication</b><font size='3'><b><code>(</code></b></font><i>username</i>, <i>password</i><font size='3'><b><code>)</code></b></font><br>Return <code>True</code> if the supplied <code>username</code> and <code>password</code> match the stored credentials.</li></ul>

<ul><li><b>impersonate_user</b><font size='3'><b><code>(</code></b></font><i>username</i>, <i>password</i><font size='3'><b><code>)</code></b></font><br>Impersonate another user (noop).  It is always called before accessing the filesystem.  By default it does nothing.  The subclass overriding this method is expected to provide a mechanism to change the current user.</li></ul>

<ul><li><b>terminate_impersonation</b><font size='3'><b><code>(</code></b></font><i>username</i><font size='3'><b><code>)</code></b></font><br>Terminate impersonation (noop).  It is always called after having accessed the filesystem.  By default it does nothing.  The subclass overriding this method is expected to provide a mechanism to switch back to the original user.</li></ul>

<ul><li><b>remove_user</b><font size='3'><b><code>(</code></b></font><i>username</i><font size='3'><b><code>)</code></b></font><br>Remove a user from the virtual user table.<br>
<hr /></li></ul>

<h2>2.3 - Control connection</h2>

class pyftpdlib.ftpserver.<b>FTPHandler</b><font size='3'><b><code>(</code></b></font><i>conn, server</i><font size='3'><b><code>)</code></b></font>

<blockquote>This class implements the FTP server Protocol Interpreter (see <a href='http://www.faqs.org/rfcs/rfc959.html'>RFC-959</a>), handling commands received from the client on the control channel by calling the command's corresponding method (e.g. for received command "MKD pathname", <code>ftp_MKD()</code> method is called with <code>pathname</code> as the argument).  All relevant session information are stored in instance variables.  <code>conn</code> is the underlying socket object instance of the newly established connection, <code>server</code> is the <code>FTPServer</code> class instance.  Basic usage simply requires creating an instance of <code>FTPHandler</code> class and specify which authorizer instance it will going to use:</blockquote>

<pre><code>&gt;&gt;&gt; handler = ftpserver.FTPHandler<br>
&gt;&gt;&gt; handler.authorizer = authorizer<br>
</code></pre>

<blockquote>All relevant session information is stored in class attributes reproduced below and can be modified before instantiating this class:</blockquote>

<ul><li><b>timeout</b><br>The timeout which is the maximum time a remote client may spend between FTP commands. If the timeout triggers, the remote client will be kicked off (defaults to <code>300</code> seconds). <i><b>New in version 5.0</b></i></li></ul>

<ul><li><b>banner</b><br>String sent when client connects (default <code>"pyftpdlib %s ready." %__ver__</code>).</li></ul>

<ul><li><b>max_login_attempts</b><br>Maximum number of wrong authentications before disconnecting (default <code>3</code>).</li></ul>

<ul><li><b>permit_foreign_addresses</b><br>Whether enable <a href='http://www.proftpd.org/docs/howto/FXP.html'>FXP</a> feature (default <code>False</code>).</li></ul>

<ul><li><b>permit_privileged_ports</b><br>Set to <code>True</code> if you want to permit active connections (PORT) over privileged ports (not recommended, default <code>False</code>).</li></ul>

<ul><li><b>masquerade_address</b><br>The "masqueraded" IP address to provide along PASV reply when pyftpdlib is running behind a NAT or other types of gateways.  When configured pyftpdlib will hide its local address and instead use the public address of your NAT (default <code>None</code>).</li></ul>

<ul><li><b>masquerade_address_map</b><br>In case the server has multiple IP addresses which are all behind a NAT router, you may wish to specify individual masquerade_addresses for each of them. The map expects a dictionary containing private IP addresses as keys, and their corresponding public (masquerade) addresses as values (defaults to <code>{}</code>). <i><b>New in version 0.6.0</b></i></li></ul>

<ul><li><b>passive_ports</b><br>What ports ftpd will use for its passive data transfers.  Value expected is a list of integers (e.g. <code>range(60000, 65535)</code>).  When configured pyftpdlib will no longer use kernel-assigned random ports (default <code>None</code>).</li></ul>

<ul><li><b>use_gmt_times</b><br>When True causes the server to report all ls and MDTM times in GMT and not local time (default <code>True</code>). <i><b>New in version 0.6.0</b></i></li></ul>

<ul><li><b>tcp_no_delay</b><br>Controls the use of the TCP_NODELAY socket option which disables the Nagle algorithm resulting in significantly better performances (default <code>True</code> on all platforms where it is supported). <i><b>New in version 0.6.0</b></i></li></ul>

<ul><li><b>use_sendfile</b><br>when True uses sendfile() system call to send a file resulting in faster uploads (from server to client). Works on UNIX only and requires pysendfile module to be installed separately: <a href='http://code.google.com/p/pysendfile/'>http://code.google.com/p/pysendfile/</a> <i><b>New in version 0.7.0</b></i></li></ul>

<blockquote><br>Follows a list of callback methods that can be overridden in a subclass. For blocking operations read the FAQ on how to <a href='http://code.google.com/p/pyftpdlib/wiki/FAQ#How_can_I_run_long-running_tasks_without_blocking_the_server?'>run time consuming tasks</a></blockquote>

<ul><li><b>on_login</b><font size='3'><b><code>(</code></b></font><i>username</i><font size='3'><b><code>)</code></b></font><br>Called on user login. <i><b>New in version 0.6.0</b></i></li></ul>

<ul><li><b>on_login_failed</b><font size='3'><b><code>(</code></b></font><i>username</i>, <i>password</i><font size='3'><b><code>)</code></b></font><br>Called on failed user login. <i><b>New in version 0.7.0</b></i></li></ul>

<ul><li><b>on_logout</b><font size='3'><b><code>(</code></b></font><i>username</i><font size='3'><b><code>)</code></b></font><br>Called when user logs out due to QUIT or USER issued twice. This is not called if client just disconnects without issuing QUIT first. <br /> <i><b>New in version 0.6.0</b></i></li></ul>

<ul><li><b>on_file_sent</b><font size='3'><b><code>(</code></b></font><i>file</i><font size='3'><b><code>)</code></b></font><br>Called every time a file has been successfully sent. <code>file</code> is the absolute name of that file.</li></ul>

<ul><li><b>on_file_received</b><font size='3'><b><code>(</code></b></font><i>file</i><font size='3'><b><code>)</code></b></font><br>Called every time a file has been successfully received. <code>file</code> is the absolute name of that file.</li></ul>

<ul><li><b>on_incomplete_file_sent</b><font size='3'><b><code>(</code></b></font><i>file</i><font size='3'><b><code>)</code></b></font><br>Called every time a file has not been entirely sent (e.g. transfer aborted by client). <code>file</code> is the absolute name of that file. <i><b>New in version 0.6.0</b></i></li></ul>

<ul><li><b>on_incomplete_file_received</b><font size='3'><b><code>(</code></b></font><i>file</i><font size='3'><b><code>)</code></b></font><br>Called every time a file has not been entirely received (e.g. transfer aborted by client). <code>file</code> is the absolute name of that file. <i><b>New in version 0.6.0</b></i></li></ul>

<hr />
<h2>2.4 - Data connection</h2>

<i>class</i> pyftpdlib.ftpserver.<b>DTPHandler</b><font size='3'><b><code>(</code></b></font><i>sock_obj, cmd_channel</i><font size='3'><b><code>)</code></b></font>

<blockquote>This class handles the server-data-transfer-process (server-DTP, see <a href='http://www.faqs.org/rfcs/rfc959.html'>RFC-959</a>) managing all transfer operations regarding the data channel.  <code>sock_obj</code> is the underlying socket object instance of the newly established connection, <code>cmd_channel</code> is the <code>FTPHandler</code> class instance.</blockquote>

<ul><li><b>timeout</b><br>The timeout which roughly is the maximum time we permit data transfers to stall for with no progress. If the timeout triggers, the remote client will be kicked off.</li></ul>

<ul><li><b>ac_in_buffer_size</b><br>
</li><li><b>ac_out_buffer_size</b><br>The buffer sizes to use when receiving and sending data (both defaulting to <code>65536</code> bytes). For LANs you may want this to be fairly large.  Depending on available memory and number of connected clients setting them to a lower value can result in better performances.</li></ul>

<hr />
<i>class</i> pyftpdlib.ftpserver.<b>ThrottledDTPHandler</b><font size='3'><b><code>(</code></b></font><i>sock_obj, cmd_channel</i><font size='3'><b><code>)</code></b></font>

<blockquote>A <code>DTPHandler</code> subclass which wraps sending and receiving in a data counter and temporarily "sleeps" the channel so that you burst to no more than x Kb/sec average. Use it instead of <code>DTPHandler</code> to set transfer rates limits for both downloads and/or uploads (see the <a href='http://code.google.com/p/pyftpdlib/source/browse/trunk/demo/throttled_ftpd.py'>demo script</a> showing the example usage).</blockquote>

<ul><li><b>read_limit</b><br>The maximum number of bytes to read (receive) in one second (defaults to 0 == no limit)</li></ul>

<ul><li><b>write_limit</b><br>The maximum number of bytes to write (send) in one second (defaults to 0 == no limit).<br>
<hr />
<h2>2.4 - Server (acceptor)</h2></li></ul>

<i>class</i> pyftpdlib.ftpserver.<b>FTPServer</b><font size='3'><b><code>(</code></b></font><i>address, handler</i><font size='3'><b><code>)</code></b></font>

<blockquote>This class is an <code>asyncore.dispatcher</code> subclass.  It creates a FTP socket listening on <code>address</code> (a tuple containing the ip:port pair), dispatching the requests to a "handler" (typically <code>FTPHandler</code> class object).  It is typically used for starting asyncore polling loop:</blockquote>

<pre><code>&gt;&gt;&gt; address = ('127.0.0.1', 21)<br>
&gt;&gt;&gt; ftpd = ftpserver.FTPServer(address, handler)<br>
&gt;&gt;&gt; ftpd.serve_forever()<br>
</code></pre>

<ul><li><b>max_cons</b><br>Number of maximum simultaneous connections accepted (default <code>512</code>).</li></ul>

<ul><li><b>max_cons_per_ip</b><br>Number of maximum connections accepted for the same IP address (default <code>0</code> == <i>no limit</i>).</li></ul>

<ul><li><b>serve_forever(</b><font size='3'><b><code>[</code></b></font><i>timeout=1</i><font size='3'><b><code>[</code></b></font>, <i>use_poll=False</i><font size='3'><b><code>[</code></b></font>, <i>count=None</i><font size='3'><b><code>]])</code></b></font><br>A wrap around asyncore.loop(); starts the asyncore polling loop including running the scheduler. The arguments are the same expected by original <a href='http://docs.python.org/library/asyncore.html#asyncore.loop'>asyncore.loop()</a> function.  <i><b>Changed in version 0.7.0</b>: turned into a classmethod</i></li></ul>

<ul><li><b>close</b><font size='3'><b><code>()</code></b></font><br>Stop serving without disconnecting currently connected clients.</li></ul>

<ul><li><b>close_all(</b><font size='3'><b><code>[</code></b></font>, <i>ignore_all=False</i><font size='3'><b><code>]])</code></b></font><br>Stop serving disconnecting also the currently connected clients. The <code>map</code> parameter is a dictionary whose items are the channels to close. If <code>map</code> is omitted, the default <code>asyncore.socket_map</code> is used. Having <code>ignore_all</code> parameter set to <code>False</code> results in raising exception in case of unexpected errors.<br>
<hr />
<h2>2.6 - File system</h2></li></ul>

<i>class</i> pyftpdlib.ftpserver.<b>AbstractedFS(</b><i>root</i>, <i>cmd_channel</i><b>)</b>

<blockquote>A class used to interact with the file system, providing a cross-platform interface compatible with both Windows and UNIX style filesystems where all paths use "/" separator. <br><code>AbstractedFS</code> distinguishes between "real" filesystem paths and "virtual" ftp paths emulating a UNIX chroot jail where the user can not escape its home directory (example: real "/home/user" path will be seen as "/" by the client). <br>It also provides some utility methods and wraps around all <code>os.*</code> calls involving operations against the filesystem like creating files or removing directories. The contructor accepts two arguments: <code>root</code> which is the user "real" home directory (e.g. '/home/user') and <code>cmd_channel</code> which is the <code>FTPHandler</code> class instance.</blockquote>

<blockquote><i><b>Changed in version 0.6.0</b>: <code>root</code> and <code>cmd_channel</code> arguments were added.</i></blockquote>

<ul><li><b>root</b><br>User's home directory ("real").  <i><b>Changed in version 0.7.0</b>: support setattr()</i></li></ul>

<ul><li><b>cwd</b><br>User's current working directory ("virtual").  <i><b>Changed in version 0.7.0</b>: support setattr()</i></li></ul>

<ul><li><b>ftpnorm</b><font size='3'><b><code>(</code></b></font><i>ftppath</i><font size='3'><b><code>)</code></b></font><br>Normalize a "virtual" ftp pathname depending on the current working directory (e.g. having "/foo" as current working directory "x" becomes "/foo/x").</li></ul>

<ul><li><b>ftp2fs</b><font size='3'><b><code>(</code></b></font><i>ftppath</i><font size='3'><b><code>)</code></b></font><br>Translate a "virtual" ftp pathname into equivalent absolute "real" filesystem pathname (e.g. having "/home/user" as root directory "x" becomes  "/home/user/x").</li></ul>

<ul><li><b>fs2ftp</b><font size='3'><b><code>(</code></b></font><i>fspath</i><font size='3'><b><code>)</code></b></font><br>Translate a "real" filesystem pathname into equivalent absolute "virtual" ftp pathname depending on the user's root directory (e.g. having "/home/user" as root directory "/home/user/x" becomes "/x".</li></ul>

<ul><li><b>validpath</b><font size='3'><b><code>(</code></b></font><i>path</i><font size='3'><b><code>)</code></b></font><br>Check whether the path belongs to user's home directory. Expected argument is a "real" filesystem path. If path is a symbolic link it is resolved to check its real destination. Pathnames escaping from user's root directory are considered not valid (return <code>False</code>).</li></ul>

<ul><li><b>open</b><font size='3'><b><code>(</code></b></font><i>filename, mode</i><font size='3'><b><code>)</code></b></font><br>Wrapper around <a href='http://docs.python.org/library/functions.html#open'>open()</a> builtin.</li></ul>

<ul><li><b>mkdir</b><font size='3'><b><code>(</code></b></font><i>path</i><font size='3'><b><code>)</code></b></font><br><b>chdir</b><font size='3'><b><code>(</code></b></font><i>path</i><font size='3'><b><code>)</code></b></font><br><b>listdir</b><font size='3'><b><code>(</code></b></font><i>path</i><font size='3'><b><code>)</code></b></font><br><b>rmdir</b><font size='3'><b><code>(</code></b></font><i>path</i><font size='3'><b><code>)</code></b></font><br><b>remove</b><font size='3'><b><code>(</code></b></font><i>path</i><font size='3'><b><code>)</code></b></font><br><b>rename</b><font size='3'><b><code>(</code></b></font><i>src, dst</i><font size='3'><b><code>)</code></b></font><br><b>chmod</b><font size='3'><b><code>(</code></b></font><i>path, mode</i><font size='3'><b><code>)</code></b></font><br><b>stat</b><font size='3'><b><code>(</code></b></font><i>path</i><font size='3'><b><code>)</code></b></font><br><b>lstat</b><font size='3'><b><code>(</code></b></font><i>path</i><font size='3'><b><code>)</code></b></font><br><b>readlink</b><font size='3'><b><code>(</code></b></font><i>path</i><font size='3'><b><code>)</code></b></font><br>Wrappers around corresponding <a href='http://docs.python.org/library/os.html'>os</a> module functions.</li></ul>

<ul><li><b>isfile</b><font size='3'><b><code>(</code></b></font><i>path</i><font size='3'><b><code>)</code></b></font><br><b>islink</b><font size='3'><b><code>(</code></b></font><i>path</i><font size='3'><b><code>)</code></b></font><br><b>isdir</b><font size='3'><b><code>(</code></b></font><i>path</i><font size='3'><b><code>)</code></b></font><br><b>getsize</b><font size='3'><b><code>(</code></b></font><i>path</i><font size='3'><b><code>)</code></b></font><br><b>getmtime</b><font size='3'><b><code>(</code></b></font><i>path</i><font size='3'><b><code>)</code></b></font><br><b>realpath</b><font size='3'><b><code>(</code></b></font><i>path</i><font size='3'><b><code>)</code></b></font><br><b>lexists</b><font size='3'><b><code>(</code></b></font><i>path</i><font size='3'><b><code>)</code></b></font><br>Wrappers around corresponding <a href='http://docs.python.org/library/os.path.html'>os.path</a> module functions.</li></ul>

<ul><li><b>mkstemp</b><font size='3'><b><code>(</code></b></font>suffix='', prefix='', dir=None, mode='wb'<font size='3'><b><code>)</code></b></font><br>Wrapper around <a href='http://docs.python.org/library/tempfile.html#tempfile.mkstemp'>tempfile.mkstemp</a>.</li></ul>

<hr />

<h1>2.1 - Contrib package</h1>

Starting from version 0.6.0 a new <code>contrib</code> package has been added to <code>pyftpdlib</code> namespace which extends base <code>ftpserver.py</code> module. Modules contained in here usually requires third-party modules to be installed separately or are specific for a given Python version or operating system.<br>
<br>
<hr />

<h2>2.2 - pyftpdlib.contrib.handlers module</h2>

This module provides basic support for FTPS (FTP over SSL/TLS) as described in <a href='http://www.ietf.org/rfc/rfc4217.txt'>RFC-4217</a> implementing AUTH, PBSZ and PROT commands.<br>
In order to make it work <a href='http://pypi.python.org/pypi/pyOpenSSL'>PyOpenSSL module</a> is required to be installed. <a href='http://code.google.com/p/billiejoex/wiki/Tutorial?ts=1284112249&updated=Tutorial#3.7_-_FTPS_(FTP_over_TLS/SSL)_server'>Example below</a> shows how to setup an FTPS server.<br>
<br>
<i>class</i> pyftpdlib.contrib.handlers.<b>TLS_FTPHandler(</b><i>conn</i>, <i>server</i><b>)</b>
<blockquote>A <code>ftpserver.FTPHandler</code> subclass supporting TLS/SSL. Configurable attributes:</blockquote>

<ul><li><b>certfile</b><br>The path to a file which contains a certificate to be used to identify the local side of the connection. This must always be specified, unless context is provided instead.</li></ul>

<ul><li><b>keyfile</b><br>The path of the file containing the private RSA key; can be omittetted if certfile already contains the private key (defaults: <code>None</code>).</li></ul>

<ul><li><b>tls_control_required</b><br>When <code>True</code> requires SSL/TLS to be established on the control channel, before logging in. This means the user will have to issue AUTH before USER/PASS (default <code>False</code>).</li></ul>

<ul><li><b>tls_data_required</b><br>When <code>True</code> requires SSL/TLS to be established on the data channel. This means the user will have to issue PROT before PASV or PORT (default <code>False</code>).</li></ul>

<hr />

<h2>2.3 - pyftpdlib.contrib.authorizers module</h2>

This module contains two classes for handling users on Unix and Windows systems. Users are no longer supposed to be explicitly added as when using <code>DummyAuthorizer</code>.<br>All FTP users are the same defined on the UNIX or Windows system so if you access on your system by using "john" as username and "12345" as password those same credentials can be used for accessing the FTP server as well.<br>The user home directories will be automatically determined when user logins (e.g. /home/user on Unix, C:\Documents and settings\user on Windows).<br>Every time a filesystem operation occurs (e.g. a file is created or deleted) the id of the process is temporarily changed to the effective user id and whether the operation will succeed depends on user and file permissions. This is why full read and write permissions are granted by default in the class constructors.<br>
<br>
<i>class</i> pyftpdlib.contrib.authorizers.<b>UnixAuthorizer(</b><i>global_perm="elradfmw"</i>, <i>allowed_users=<code>[]</code></i>, <i>rejected_users=<code>[]</code></i>, <i>require_valid_shell=True</i>, <i>anonymous_user=None</i>, ,<i>msg_login="Login successful."</i>, <i>msg_quit="Goodbye."</i><b>)</b>

<blockquote><code>global_perm</code> is a series of letters referencing the users permissions; defaults to <code>"elradfmw"</code> which means full read and write access for everybody (except anonymous). <code>allowed_users</code> and <code>rejected_users</code> options expect a list of users which are accepted or rejected for authenticating against the FTP server; defaults both to <code>[]</code> (no restrictions). <code>require_valid_shell</code> deny access for those users which do not have a valid shell binary listed in /etc/shells. If /etc/shells cannot be found this is a no-op.  Anonymous user is not subject to this option, and is free to not have a valid shell defined. Defaults to <code>True</code> (a valid shell is required for login). <code>anonymous_user</code> can be specified if you intend to provide anonymous access. The value expected is a string representing the system user to use for managing anonymous sessions; defaults to <code>None</code>  (anonymous access disabled).<br> Note that in order to use this class super user privileges are required.</blockquote>

<blockquote><i><b>New in version 0.6.0</b></i></blockquote>

<ul><li><b>override_user(</b><i>username=None</i>, <i>password=None</i>, <i>homedir=None</i>, <i>perm=None</i>, <i>anonymous_user=None</i>, <i>msg_login=None</i>, <i>msg_quit=None</i><b>)</b><br>Overrides one or more options specified in the class constructor for a specific user.</li></ul>

<blockquote>Examples:</blockquote>

<pre><code>&gt;&gt;&gt; from pyftpdlib.contrib.authorizers import UnixAuthorizer<br>
&gt;&gt;&gt; auth = UnixAuthorizer(rejected_users=["root"])<br>
&gt;&gt;&gt; auth = UnixAuthorizer(allowed_users=["matt", "jay"])<br>
&gt;&gt;&gt; auth = UnixAuthorizer(require_valid_shell=False)<br>
&gt;&gt;&gt; auth.override_user("matt", password="foo", perm="elr")<br>
</code></pre>

<hr />

<i>class</i> pyftpdlib.contrib.authorizers.<b>WindowsAuthorizer(</b><i>global_perm="elradfmw"</i>, <i>allowed_users=<code>[]</code></i>, <i>rejected_users=<code>[]</code></i>, <i>anonymous_user=None</i>, <i>anonymous_password=""</i>, <i>msg_login="Login successful."</i>, <i>msg_quit="Goodbye."</i><b>)</b>:<br>
<br>
<blockquote>Same as <code>UnixAuthorizer</code> except for <i>anonymous_password</i> argument which must be specified when defining the <i>anonymous_user</i>.<br>Also <i>requires_valid_shell</i> option is not available. In order to use this class <a href='http://sourceforge.net/projects/pywin32/'>pywin32 extension</a> must be installed.</blockquote>

<blockquote><i><b>New in version 0.6.0</b></i></blockquote>

<hr />

<h2>2.4 - pyftpdlib.contrib.filesystems module</h2>

class pyftpdlib.contrib.filesystems.<b>UnixFilesystem(</b><i>root</i>, <i>cmd_channel</i><b>)</b>
<blockquote>Represents the real UNIX filesystem. Differently from AbstractedFS the client will login into  <code>/home/&lt;username&gt;</code> and will be able to escape its home directory and navigate the real filesystem. Use it in conjuction with <code>UnixAuthorizer</code> to implement a "real" UNIX FTP server (see <a href='http://pyftpdlib.googlecode.com/svn/trunk/demo/unix_ftpd.py'>demo/unix_ftpd.py</a>).</blockquote>

<blockquote><i><b>New in version 0.6.0</b></i></blockquote>

<hr />

<h1>3.0 - Customizing your FTP server</h1>

Below is a set of example scripts showing some of the possible customizations that can be done with pyftpdlib.  Some of them are included in <a href='http://code.google.com/p/pyftpdlib/source/browse/#svn/trunk/demo'>demo</a> directory of pyftpdlib source distribution.<br>
<br>
<h2>3.1 - Building a Base FTP server</h2>

The script below is a basic configuration, and it's probably the best starting point for understanding how things work. It uses the base <code>DummyAuthorizer</code> for adding a bunch of "virtual" users.<br>
<br>
It also sets a limit for connections by overriding <code>FTPServer.max_cons</code> and <code>FTPServer.max_cons_per_ip</code> attributes which are intended to set limits for maximum connections to handle simultaneously and maximum connections from the same IP address.<br>
Overriding these variables is always a good idea (they default to <code>0</code>, or "no limit") since they are a good workaround for avoiding DoS attacks.<br>
<br>
<a href='http://pyftpdlib.googlecode.com/svn/trunk/demo/basic_ftpd.py'>download script</a>

<pre><code>from pyftpdlib import ftpserver<br>
<br>
def main():<br>
    # Instantiate a dummy authorizer for managing 'virtual' users<br>
    authorizer = ftpserver.DummyAuthorizer()<br>
<br>
    # Define a new user having full r/w permissions and a read-only<br>
    # anonymous user<br>
    authorizer.add_user('user', password="12345", homedir='.', perm='elradfmw')<br>
    authorizer.add_anonymous(homedir='.')<br>
<br>
    # Instantiate FTP handler class<br>
    handler = ftpserver.FTPHandler<br>
    handler.authorizer = authorizer<br>
<br>
    # Define a customized banner (string returned when client connects)<br>
    handler.banner = "pyftpdlib %s based ftpd ready." %ftpserver.__ver__<br>
<br>
    # Specify a masquerade address and the range of ports to use for<br>
    # passive connections.  Decomment in case you're behind a NAT.<br>
    #handler.masquerade_address = '151.25.42.11'<br>
    #handler.passive_ports = range(60000, 65535)<br>
<br>
    # Instantiate FTP server class and listen to 0.0.0.0:21<br>
    address = ('', 21)<br>
    server = ftpserver.FTPServer(address, handler)<br>
<br>
    # set a limit for connections<br>
    server.max_cons = 256<br>
    server.max_cons_per_ip = 5<br>
<br>
    # start ftp server<br>
    server.serve_forever()<br>
<br>
if __name__ == '__main__':<br>
    main()<br>
</code></pre>


<h2>3.2 - Logging management</h2>

As mentioned, ftpserver.py comes with 3 different functions intended for a separate logging system: <code>log()</code>, <code>logline()</code> and <code>logerror()</code>.<br>
Let's suppose you don't want to print FTPd messages on screen but you want to write them into different files: <i>"/var/log/ftpd.log"</i> will be main log file, <i>"/var/log/ftpd.lines.log"</i> the one where you'll want to store commands and responses passing through the control connection.<br>
<br>
Here's one method this could be implemented:<br>
<br>
<pre><code>import os<br>
import time<br>
<br>
from pyftpdlib import ftpserver<br>
<br>
now = lambda: time.strftime("[%Y-%b-%d %H:%M:%S]")<br>
f1 = open('ftpd.log', 'a')<br>
f2 = open('ftpd.lines.log', 'a')<br>
<br>
def standard_logger(msg):<br>
    f1.write("%s %s\n" %(now(), msg))<br>
    f1.flush()<br>
<br>
def line_logger(msg):<br>
    f2.write("%s %s\n" %(now(), msg))<br>
    f2.flush()<br>
<br>
def main():<br>
    ftpserver.log = standard_logger<br>
    ftpserver.logline = line_logger<br>
    authorizer = ftpserver.DummyAuthorizer()<br>
    authorizer.add_anonymous(os.getcwd())<br>
    handler = ftpserver.FTPHandler<br>
    handler.authorizer = authorizer<br>
    server = ftpserver.FTPServer(('', 21), handler)<br>
    server.serve_forever()<br>
<br>
<br>
if __name__ == "__main__":<br>
    main()<br>
</code></pre>


<h2>3.3 - Storing passwords as hash digests</h2>

Using FTP server library with the default <code>DummyAuthorizer</code> means that password will be stored in clear-text. An end-user ftpd using the default dummy authorizer would typically require a configuration file for authenticating users and their passwords but storing clear-text passwords is of course undesirable.<br>
<br>
The most common way to do things in such case would be first creating new users and then storing their usernames + passwords as hash digests into a file or wherever you find it convenient.<br>
<br>
The example below shows how to easily create an encrypted account storage system by storing passwords as one-way hashes by using md5 algorithm.<br>
This could be easily done by using the <b>hashlib</b> module included with Python stdlib and by sub-classing the original <code>DummyAuthorizer</code> class overriding its <code>validate_authentication()</code> method.<br>
<br>
<a href='http://pyftpdlib.googlecode.com/svn/trunk/demo/md5_ftpd.py'>download script</a>

<pre><code>import os<br>
try:<br>
    from hashlib import md5<br>
except ImportError:<br>
    from md5 import new as md5  # Python &lt; 2.5<br>
<br>
from pyftpdlib import ftpserver<br>
<br>
<br>
class DummyMD5Authorizer(ftpserver.DummyAuthorizer):<br>
<br>
    def validate_authentication(self, username, password):<br>
        hash = md5(password).hexdigest()<br>
        return self.user_table[username]['pwd'] == hash<br>
<br>
def main():<br>
    # get a hash digest from a clear-text password<br>
    hash = md5('12345').hexdigest()<br>
    authorizer = DummyMD5Authorizer()<br>
    authorizer.add_user('user', hash, os.getcwd(), perm='elradfmw')<br>
    authorizer.add_anonymous(os.getcwd())<br>
    handler = ftpserver.FTPHandler<br>
    handler.authorizer = authorizer<br>
    server = ftpserver.FTPServer(('', 21), handler)<br>
    server.serve_forever()<br>
<br>
if __name__ == "__main__":<br>
    main()<br>
</code></pre>


<h2>3.4 - Unix FTP Server</h2>

If you're running a Unix system you may want to configure your ftpd to include support for "real" users existing on the system and navigate the real filesystem.<br>
<br>
The example below uses <code>UnixAuthorizer</code> and <code>UnixFilesystem</code> classes to do so.<br>
<br>
<pre><code>from pyftpdlib import ftpserver<br>
from pyftpdlib.contrib.authorizers import UnixAuthorizer<br>
from pyftpdlib.contrib.filesystems import UnixFilesystem<br>
<br>
<br>
def main():<br>
    authorizer = UnixAuthorizer(rejected_users=["root"], require_valid_shell=True)<br>
    handler = ftpserver.FTPHandler<br>
    handler.authorizer = authorizer<br>
    handler.abstracted_fs = UnixFilesystem<br>
    server = ftpserver.FTPServer(('', 21), handler)<br>
    server.serve_forever()<br>
<br>
if __name__ == "__main__":<br>
    main()<br>
</code></pre>

<h2>3.5 - Windows NT FTP Server</h2>

The following code shows how to implement a basic authorizer for a Windows NT workstation to authenticate against existing Windows user accounts. This code requires Mark Hammond's <a href='http://starship.python.net/crew/mhammond/win32/'>pywin32</a> extension to be installed.<br>
<br>
<a href='http://pyftpdlib.googlecode.com/svn/trunk/demo/winnt_ftpd.py'>download script</a>

<pre><code>from pyftpdlib import ftpserver<br>
from pyftpdlib.contrib.authorizers import WindowsAuthorizer<br>
<br>
<br>
def main():<br>
    authorizer = WindowsAuthorizer()<br>
    # Use Guest user with empty password to handle anonymous sessions.<br>
    # Guest user must be enabled first, empty password set and profile<br>
    # directory specified.<br>
    #authorizer = WindowsAuthorizer(anonymous_user="Guest", anonymous_password="")<br>
    handler = ftpserver.FTPHandler<br>
    handler.authorizer = authorizer<br>
    ftpd = ftpserver.FTPServer(('', 21), handler)<br>
    ftpd.serve_forever()<br>
<br>
if __name__ == "__main__":<br>
    main()<br>
</code></pre>

<h2>3.6 - Throttle bandwidth</h2>

An important feature for an ftpd is limiting the speed for downloads and uploads affecting the data channel. Starting from version 0.5.2 it is possible to use the new <code>ThrottledDTPHandler</code> class to set such limits.<br>
The basic idea behind <code>ThrottledDTPHandler</code> is to wrap sending and receiving in a data counter and temporary "sleep" the data channel so that you burst to no more than x Kb/sec average. When it realizes that more than x Kb in a second are being transmitted it temporary blocks the transfer for a certain number of seconds.<br>
<br>
<pre><code>import os<br>
<br>
from pyftpdlib import ftpserver<br>
<br>
<br>
def main():<br>
    authorizer = ftpserver.DummyAuthorizer()<br>
    authorizer.add_user('user', '12345', os.getcwd(), perm='elradfmw')<br>
    authorizer.add_anonymous(os.getcwd())<br>
<br>
    dtp_handler = ftpserver.ThrottledDTPHandler<br>
    dtp_handler.read_limit = 30720  # 30 Kb/sec (30 * 1024)<br>
    dtp_handler.write_limit = 30720  # 30 Kb/sec (30 * 1024)<br>
<br>
    ftp_handler = ftpserver.FTPHandler<br>
    ftp_handler.authorizer = authorizer<br>
    # have the ftp handler use the alternative dtp handler class<br>
    ftp_handler.dtp_handler = dtp_handler<br>
<br>
    ftpd = ftpserver.FTPServer(('', 21), ftp_handler)<br>
    ftpd.serve_forever()<br>
<br>
if __name__ == '__main__':<br>
    main()<br>
</code></pre>

<h2>3.7 - FTPS (FTP over TLS/SSL) server</h2>

Starting from version 0.6.0 pyftpdlib finally includes full FTPS support implementing both TLS and SSL protocols and <b>AUTH</b>, <b>PBSZ</b> and <b>PROT</b> commands as defined in <a href='http://www.ietf.org/rfc/rfc4217.txt'>RFC-4217</a>. This has been implemented by using <a href='http://pypi.python.org/pypi/pyOpenSSL'>PyOpenSSL</a> module, which is required in order to run the code below. <code>TLS_FTPHandler</code> class requires at least a <code>certfile</code> to be specified and optionally a <code>keyfile</code>. <a href='http://www.modssl.org/docs/2.7/ssl_faq.html#ToC24'>Apache FAQs</a> provide instructions on how to generate them. If you don't care about having your personal self-signed certificates you can use the one in the demo directory which include both and is available <a href='http://pyftpdlib.googlecode.com/svn/trunk/demo/keycert.pem'>here</a>.<br>
<br>
<a href='http://pyftpdlib.googlecode.com/svn/trunk/demo/tls_ftpd.py'>download script</a>

<pre><code>#!/usr/bin/env python<br>
<br>
"""<br>
An RFC-4217 asynchronous FTPS server supporting both SSL and TLS.<br>
Requires PyOpenSSL module (http://pypi.python.org/pypi/pyOpenSSL).<br>
"""<br>
<br>
from pyftpdlib import ftpserver<br>
from pyftpdlib.contrib.handlers import TLS_FTPHandler<br>
<br>
<br>
def main():<br>
    authorizer = ftpserver.DummyAuthorizer()<br>
    authorizer.add_user('user', '12345', '.', perm='elradfmw')<br>
    authorizer.add_anonymous('.')<br>
    handler = TLS_FTPHandler<br>
    handler.certfile = 'keycert.pem'<br>
    handler.authorizer = authorizer<br>
    # requires SSL for both control and data channel<br>
    #handler.tls_control_required = True<br>
    #handler.tls_data_required = True<br>
    ftpd = ftpserver.FTPServer(('', 21), handler)<br>
    ftpd.serve_forever()<br>
<br>
if __name__ == '__main__':<br>
    main()<br>
</code></pre>

<h2>3.8 - Event callbacks</h2>

A small example which shows how to use callback methods via <code>FTPHandler</code> subclassing:<br>
<br>
<pre><code>from pyftpdlib import ftpserver<br>
<br>
class MyHandler(ftpserver.FTPHandler):<br>
<br>
    def on_login(self, username):<br>
        # do something when user login<br>
        pass<br>
<br>
    def on_logout(self, username):<br>
        # do something when user logs out<br>
        pass<br>
        <br>
    def on_file_sent(self, file):<br>
        # do something when a file has been sent<br>
        pass<br>
<br>
    def on_file_received(self, file):<br>
        # do something when a file has been received<br>
        pass<br>
            <br>
    def on_incomplete_file_sent(self, file):<br>
        # do something when a file is partially sent<br>
        pass<br>
<br>
    def on_incomplete_file_received(self, file):<br>
        # remove partially uploaded files<br>
        import os<br>
        os.remove(file)<br>
<br>
<br>
def main():<br>
    authorizer = ftpserver.DummyAuthorizer()<br>
    authorizer.add_user('user', '12345', homedir='.', perm='elradfmw')<br>
    authorizer.add_anonymous(homedir='.')<br>
<br>
    handler = MyHandler<br>
    handler.authorizer = authorizer<br>
    server = ftpserver.FTPServer(('', 21), handler)<br>
    server.serve_forever()<br>
<br>
if __name__ == "__main__":<br>
    main()<br>
</code></pre>

<h2>3.9 - Command line usage</h2>

Starting from version 0.6.0 pyftpdlib can be run as a simple stand-alone server via Python's -m option, which is particularly useful when you want to quickly share a directory. Some examples.<br>
<br>
Anonymous FTPd sharing current directory:<br>
<br>
<pre><code>python -m pyftpdlib.ftpserver<br>
</code></pre>

Anonymous FTPd with write permission:<br>
<br>
<pre><code>python -m pyftpdlib.ftpserver -w<br>
</code></pre>

Set a different address/port and home directory:<br>
<br>
<pre><code>python -m pyftpdlib.ftpserver -i localhost -p 8021 -d /home/someone<br>
</code></pre>

See <code>python -m pyftpdlib.ftpserver -h</code> for a complete list of options.