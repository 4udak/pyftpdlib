<a href='Hidden comment: 
Reminder as to how I calculated these numbers:

old = 1524.05
new = 1652.72

...aka:

1524.05 : 100 = 1652.72 : x

calculus:

100 * 1652.72 / 1524.05 = 108,4426..  (aka 108%, aka 8% speedup)

...to get the "x" notation:

(108,442636396 - 100) / 100 = +0.08x

A python program:

a = 1524.05
b = 1652.72
perc = 100.0 * b / a
print "+%.2f" % ((perc - 100) / 100)
'></a>

_All the benchmarks were conducted on a Linux Ubuntu 12.04  Intel core duo - 3.1 Ghz box._<br><i>Results where pyftpdlib wins are marked in</i><font color='green'>green</font>, else in <font color='red'>red</font>.<br>
<br>
<br>
<h1>pyftpdlib 0.7.0 vs. pyftpdlib 1.0.0</h1>

<table><thead><th> <b>benchmark type</b> </th><th> <b>0.7.0</b> </th><th> <b>1.0.0</b> </th><th> <b>speedup</b> </th></thead><tbody>
<tr><td> STOR (client -> server)                </td><td>              528.63 MB/sec </td><td> 585.90 MB/sec </td><td> <font color='green'>+0.1x</font> </td></tr>
<tr><td> RETR (server -> client)                </td><td>             1702.07 MB/sec </td><td> 1652.72 MB/sec </td><td> <font color='red'>-0.02x</font> </td></tr>
<tr><td> 300 concurrent clients (connect, login)  </td><td>              1.70 secs </td><td> 0.19 secs    </td><td> <font color='green'>+8x</font> </td></tr>
<tr><td> STOR (1 file with 300 idle clients)      </td><td>             60.77 MB/sec </td><td> 585.59 MB/sec </td><td> <font color='green'>+8.6x</font> </td></tr>
<tr><td> RETR (1 file with 300 idle clients)        </td><td>           63.46 MB/sec </td><td> 1497.58 MB/sec </td><td> <font color='green'>+22.5x</font> </td></tr>
<tr><td> 300 concurrent clients (RETR 10M file)       </td><td>          4.68 secs </td><td> 3.41 secs    </td><td> <font color='green'>+0.3x</font> </td></tr>
<tr><td> 300 concurrent clients (STOR 10M file)         </td><td>       10.13 secs </td><td> 8.78 secs    </td><td> <font color='green'>+0.1x</font> </td></tr>
<tr><td> 300 concurrent clients (QUIT)                    </td><td>      0.02 secs </td><td> 0.02 secs    </td><td> 0x             </td></tr></tbody></table>

<h1>pyftpdlib vs. proftpd 1.3.4</h1>

<table><thead><th> <b>benchmark type</b> </th><th> <b>pyftpdlib</b> </th><th> <b>proftpd</b> </th><th> <b>speedup</b> </th></thead><tbody>
<tr><td> STOR (client -> server)                      </td><td>        585.90 MB/sec </td><td> 600.49 MB/sec  </td><td> <font color='red'>-0.02x</font> </td></tr>
<tr><td> RETR (server -> client)                      </td><td>       1652.72 MB/sec </td><td> 1524.05 MB/sec </td><td> <font color='green'>+0.08</font> </td></tr>
<tr><td> 300 concurrent clients (connect, login)      </td><td>          0.19 secs </td><td> 9.98 secs      </td><td> <font color='green'>+51x</font> </td></tr>
<tr><td> STOR (1 file with 300 idle clients)          </td><td>        585.59 MB/sec </td><td> 518.55 MB/sec  </td><td> <font color='green'>+0.1x</font> </td></tr>
<tr><td> RETR (1 file with 300 idle clients)          </td><td>       1497.58 MB/sec </td><td> 1478.19 MB/sec </td><td> <font color='black'>0x</font> </td></tr>
<tr><td> 300 concurrent clients (RETR 10M file)       </td><td>          3.41 secs </td><td> 3.60 secs      </td><td> <font color='green'>+0.05x</font> </td></tr>
<tr><td> 300 concurrent clients (STOR 10M file)       </td><td>          8.60 secs </td><td> 11.56 secs     </td><td> <font color='green'>+0.3x</font> </td></tr>
<tr><td> 300 concurrent clients (QUIT)                </td><td>          0.03 secs </td><td> 0.39 secs      </td><td> <font color='green'>+12x</font> </td></tr></tbody></table>

<h1>pyftpdlib vs. vsftpd 2.3.5</h1>

<table><thead><th> <b>benchmark type</b> </th><th> <b>pyftpdlib</b> </th><th> <b>vsftpd</b> </th><th> <b>speedup</b> </th></thead><tbody>
<tr><td> STOR (client -> server)                      </td><td>        585.90 MB/sec </td><td> 611.73 MB/sec </td><td> <font color='red'>-0.04x</font> </td></tr>
<tr><td> RETR (server -> client)                      </td><td>       1652.72 MB/sec </td><td> 1512.92 MB/sec </td><td> <font color='green'>+0.09</font> </td></tr>
<tr><td> 300 concurrent clients (connect, login)      </td><td>          0.19 secs </td><td> 20.39 secs    </td><td> <font color='green'>+106x</font> </td></tr>
<tr><td> STOR (1 file with 300 idle clients)          </td><td>        585.59 MB/sec </td><td> 610.23 MB/sec </td><td> <font color='red'>-0.04x</font> </td></tr>
<tr><td> RETR (1 file with 300 idle clients)          </td><td>       1497.58 MB/sec </td><td> 1493.01 MB/sec </td><td> <font color='black'>0x</font> </td></tr>
<tr><td> 300 concurrent clients (RETR 10M file)       </td><td>          3.41 secs </td><td> 3.67 secs     </td><td> <font color='green'>+0.07x</font> </td></tr>
<tr><td> 300 concurrent clients (STOR 10M file)       </td><td>          8.60 secs </td><td> 9.82 secs     </td><td> <font color='green'>+0.07x</font> </td></tr>
<tr><td> 300 concurrent clients (QUIT)                </td><td>          0.03 secs </td><td> 0.01 secs     </td><td> <font color='red'>+0.14x</font> </td></tr></tbody></table>

<h1>pyftpdlib vs. Twisted 12.3</h1>

By using <i>sendfile()</i> (Twisted <b>does not</b> support sendfile()):<br>
<br>
<table><thead><th> <b>benchmark type</b> </th><th> <b>pyftpdlib</b> </th><th> <b>twisted</b> </th><th> <b>speedup</b> </th></thead><tbody>
<tr><td> STOR (client -> server)                      </td><td>        585.90 MB/sec </td><td> 496.44 MB/sec  </td><td> <font color='green'>+0.01x</font> </td></tr>
<tr><td> RETR (server -> client)                      </td><td>       1652.72 MB/sec </td><td> 283.24 MB/sec  </td><td> <font color='green'>+4.8x</font> </td></tr>
<tr><td> 300 concurrent clients (connect, login)      </td><td>          0.19 secs </td><td> 0.19 secs      </td><td> <font>+0x</font> </td></tr>
<tr><td> STOR (1 file with 300 idle clients)          </td><td>        585.59 MB/sec </td><td> 506.55 MB/sec  </td><td> <font color='green'>+0.16x</font> </td></tr>
<tr><td> RETR (1 file with 300 idle clients)          </td><td>       1497.58 MB/sec </td><td> 280.63 MB/sec  </td><td> <font color='green'>+4.3x</font> </td></tr>
<tr><td> 300 concurrent clients (RETR 10M file)       </td><td>          3.41 secs </td><td> 11.40 secs     </td><td> <font color='green'>+2.3x</font> </td></tr>
<tr><td> 300 concurrent clients (STOR 10M file)       </td><td>          8.60 secs </td><td> 9.22 secs      </td><td> <font color='green'>+0.07x</font> </td></tr>
<tr><td> 300 concurrent clients (QUIT)                </td><td>          0.03 secs </td><td> 0.09 secs      </td><td> <font color='green'>+2x</font> </td></tr></tbody></table>

By using plain <i>send()</i>:<br>
<br>
<table><thead><th> <b>benchmark type</b> </th><th> <b>pyftpdlib</b> </th><th> <b>twisted</b> </th><th> <b>speedup</b> </th></thead><tbody>
<tr><td> RETR (server -> client)                      </td><td>       894.29 MB/sec </td><td> 283.24 MB/sec  </td><td> <font color='green'>+2.1x</font> </td></tr>
<tr><td> RETR (1 file with 300 idle clients)          </td><td>       900.98 MB/sec </td><td> 280.63 MB/sec  </td><td> <font color='green'>+2.1x</font> </td></tr></tbody></table>


<h1>Memory usage</h1>

<i>Values on UNIX are calculated as (rss - shared).</i>

<table><thead><th> <b>benchmark type</b> </th><th> <b>pyftpdlib</b> </th><th> <b>proftpd 1.3.4</b> </th><th> <b>vsftpd 2.3.5</b> </th><th> <b>twisted 12.3</b> </th></thead><tbody>
<tr><td> Starting with                                  </td><td> <font color='red'>6.7M</font>  </td><td> 1.4M                 </td><td> 352.0K              </td><td> 13.4M               </td></tr>
<tr><td> STOR (1 client)                        </td><td> <font color='red'>6.7M</font>  </td><td> 8.5M                 </td><td> 816.0K              </td><td> 13.5M               </td></tr>
<tr><td> RETR (1 client)                        </td><td> <font color='red'>6.8M</font>  </td><td> 8.5M                 </td><td> 816.0K              </td><td> 13.5M               </td></tr>
<tr><td> 300 concurrent clients (connect, login)        </td><td> <font color='green'>8.8M</font>  </td><td> 568.6M               </td><td> 140.9M              </td><td> 13.5M               </td></tr>
<tr><td> STOR (1 file with 300 idle clients)            </td><td> <font color='green'>8.8M</font>  </td><td> 570.6M               </td><td> 141.4M              </td><td> 13.5M               </td></tr>
<tr><td> RETR (1 file with 300 idle clients)            </td><td> <font color='green'>8.8M</font>  </td><td> 570.6M               </td><td> 141.4M              </td><td> 13.5M               </td></tr>
<tr><td> 300 concurrent clients (RETR 10.0M file)       </td><td> <font color='green'>10.8M</font>  </td><td> 568.6M               </td><td> 140.9M              </td><td> 24.5M               </td></tr>
<tr><td> 300 concurrent clients (STOR 10.0M file)       </td><td> <font color='green'>12.6</font>  </td><td> 568.7M               </td><td> 140.9M              </td><td> 24.7M               </td></tr></tbody></table>

<h1>Interpreting the results</h1>

pyftpdlib and <a href='http://www.proftpd.org/'>proftpd</a> / <a href='https://security.appspot.com/vsftpd.html'>vsftpd</a> looks pretty much equally fast. The huge difference is noticeable in scalability though, because of the concurrency model adopted.<br>
Both proftpd and vsftpd spawn a new process for every connected client, where pyftpdlib doesn't (see <a href='http://www.kegel.com/c10k.html'>the C10k problem</a>).<br>
The outcome is well noticeable on connect/login benchmarks and memory benchmarks.<br>
<br>
The huge differences between <a href='http://pyftpdlib.googlecode.com/files/pyftpdlib-0.7.0.tar.gz'>0.7.0</a> and <a href='http://pyftpdlib.googlecode.com/files/pyftpdlib-1.0.0.tar.gz'>1.0.0</a> versions of pyftpdlib are due to fix of <a href='https://code.google.com/p/pyftpdlib/issues/detail?id=203'>issue 203</a>. On Linux we now use <i><a href='http://linux.die.net/man/4/epoll'>epoll()</a></i> which scales considerably better than <i><a href='http://linux.die.net/man/2/select'>select()</a></i>.<br>
The fact that we're downloading a file with 300 idle clients doesn't make any difference for <i>epoll()</i>. We might as well had 5000 idle clients and the result would have been the same.<br>
On Windows, where we still use select(), 1.0.0 still wins hands down as the asyncore loop was reimplemented from scratch in order to support fd un/registration and modification (see <a href='http://code.google.com/p/pyftpdlib/issues/detail?id=203'>issue 203</a>).<br>
<br>
All the benchmarks were conducted on a Linux Ubuntu 12.04  Intel core duo - 3.1 Ghz box.<br>
<br>
<h1>Setup</h1>

The following setup was used before running every benchmark:<br>
<br>
<h3>proftpd</h3>

<pre><code># /etc/proftpd/proftpd.conf<br>
<br>
MaxInstances        2000<br>
</code></pre>

...followed by:<br>
<br>
<pre><code>$ sudo service proftpd restart<br>
</code></pre>

<h3>vsftpd</h3>

<pre><code># /etc/vsftpd.conf<br>
<br>
local_enable=YES<br>
write_enable=YES<br>
max_clients=2000<br>
max_per_ip=2000<br>
</code></pre>

...followed by:<br>
<br>
<pre><code>$ sudo service vsftpd restart<br>
</code></pre>

<h3>twisted FTP server</h3>

<pre><code>from twisted.protocols.ftp import FTPFactory, FTPRealm<br>
from twisted.cred.portal import Portal<br>
from twisted.cred.checkers import AllowAnonymousAccess, FilePasswordDB<br>
from twisted.internet import reactor<br>
import resource<br>
<br>
soft, hard = resource.getrlimit(resource.RLIMIT_NOFILE)<br>
resource.setrlimit(resource.RLIMIT_NOFILE, (hard, hard))<br>
open('pass.dat', 'w').write('user:some-passwd')<br>
p = Portal(FTPRealm('./'),<br>
           [AllowAnonymousAccess(), FilePasswordDB("pass.dat")])<br>
f = FTPFactory(p)<br>
reactor.listenTCP(21, f)<br>
reactor.run()<br>
</code></pre>

...followed by:<br>
<pre><code>$ sudo python twist_ftpd.py<br>
</code></pre>


<h3>pyftpdlib</h3>

The following patch was applied first:<br>
<br>
<pre><code>Index: pyftpdlib/servers.py<br>
===================================================================<br>
--- pyftpdlib/servers.py	(revisione 1154)<br>
+++ pyftpdlib/servers.py	(copia locale)<br>
@@ -494,3 +494,10 @@<br>
 <br>
             def _map_len(self):<br>
                 return len(multiprocessing.active_children())<br>
+<br>
+import resource<br>
+soft, hard = resource.getrlimit(resource.RLIMIT_NOFILE)<br>
+resource.setrlimit(resource.RLIMIT_NOFILE, (hard, hard))<br>
+FTPServer.max_cons = 0<br>
</code></pre>

...followed by:<br>
<pre><code>$ sudo python demo/unix_daemon.py<br>
</code></pre>

The <a href='http://pyftpdlib.googlecode.com/svn/trunk/test/bench.py'>benchmark script</a> was run as:<br>
<br>
<pre><code>python test/bench.py -u USERNAME -p PASSWORD -b all -n 300<br>
</code></pre>

...and for the memory test:<br>
<br>
<pre><code>python test/bench.py -u USERNAME -p PASSWORD -b all -n 300 -k FTP_SERVER_PID<br>
</code></pre>