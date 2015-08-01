# Table of contents #



# Introduction #

This page lists current standard Internet RFCs that define the FTP protocol.

pyftpdlib conforms to the FTP protocol standard as defined in [RFC-959](http://www.ietf.org/rfc/rfc959.txt) and [RFC-1123](http://www.ietf.org/rfc/rfc1123.txt) implementing all the fundamental commands and features described in them. It also implements some more recent features such as OPTS and FEAT commands ([RFC-2398](http://www.ietf.org/rfc/rfc2389.txt)), EPRT and EPSV commands covering the IPv6 support ([RFC-2428](ftp://ftp.rfc-editor.org/in-notes/rfc2428.txt)) and MDTM, MLSD, MLST and SIZE commands defined in [RFC-3659](http://www.ietf.org/rfc/rfc3659.txt).

Future plans for pyftpdlib include the gradual implementation of other standards track RFCs.

Some of the features like ACCT or SMNT commands will never be implemented deliberately. Other features described in more recent RFCs like the TLS/SSL support for securing FTP ([RFC-4217](http://www.ietf.org/rfc/rfc4217.txt)) are now implemented as a [demo script](http://code.google.com/p/pyftpdlib/source/browse/trunk/demo/tls_ftpd.py), waiting to reach the proper level of stability to be then included in the standard code base.

# RFC-959 - File Transfer Protocol #

The base specification of the current File Transfer Protocol.
  * Issued: October 1985
  * Status: STANDARD
  * Obsoletes: [RFC-765](http://www.ietf.org/rfc/rfc765.txt)
  * Updated by: [RFC-1123](http://www.ietf.org/rfc/rfc1123.txt), [RFC-2228](http://www.ietf.org/rfc/rfc2228.txt), [RFC-2640](http://www.ietf.org/rfc/rfc2640.txt), [RFC-2773](http://www.ietf.org/rfc/rfc2773.txt)
  * [Link](http://www.ietf.org/rfc/rfc959.txt)

| **Command** | **Implemented** | **Milestone** | **Description** | **Notes** |
|:------------|:----------------|:--------------|:----------------|:----------|
| ABOR        | YES             | 0.1.0         | Abort data transfer. |           |
| ACCT        | NO              | ---           | Specify account information. | It will never be implemented (useless). |
| ALLO        | YES             | 0.1.0         | Ask for server to allocate enough storage space. | Treated as a NOOP (no operation). |
| APPE        | YES             | 0.1.0         | Append data to an existing file. |           |
| CDUP        | YES             | 0.1.0         | Go to parent directory. |           |
| CWD         | YES             | 0.1.0         | Change current working directory. |           |
| DELE        | YES             | 0.1.0         | Delete file.    |           |
| HELP        | YES             | 0.1.0         | Show help.      | Accept also arguments. |
| LIST        | YES             | 0.1.0         | List files.     | Accept also bad arguments like "-ls", "-la", ... |
| MKD         | YES             | 0.1.0         | Create directory. |           |
| MODE        | YES             | 0.1.0         | Set data transfer mode. | "STREAM" mode is supported, "Block" and "Compressed" aren't. |
| NLST        | YES             | 0.1.0         | List files in a compact form. | Globbing of wildcards is not supported (for example, `NLST *.txt` will not work) |
| NOOP        | YES             | 0.1.0         | NOOP (no operation), just do nothing. |           |
| PASS        | YES             | 0.1.0         | Set user password. |           |
| PASV        | YES             | 0.1.0         | Set server in passive connection mode. |           |
| PORT        | YES             | 0.1.0         | Set server in active connection mode. |           |
| PWD         | YES             | 0.1.0         | Get current working directory. |           |
| QUIT        | YES             | 0.1.0         | Quit session.   | If file transfer is in progress, the connection will remain open until it is finished. |
| REIN        | YES             | 0.1.0         | Reinitialize user's current session. |           |
| REST        | YES             | 0.1.0         | Restart file position. |           |
| RETR        | YES             | 0.1.0         | Retrieve a file (client's download). |           |
| RMD         | YES             | 0.1.0         | Remove directory. |           |
| RNFR        | YES             | 0.1.0         | File renaming (source) |           |
| RNTO        | YES             | 0.1.0         | File renaming (destination) |           |
| SITE        | YES             | 0.5.1         | Site specific server services. | No SITE commands aside from "SITE HELP" are implemented by default.  The user willing to add support for a specific SITE command has to define a new `ftp_SITE_%CMD%` method in the `FTPHandler` subclass. |
| SMNT        | NO              | ---           | Mount file-system structure. | Will never be implemented (too much system-dependent and almost never used). |
| STAT        | YES             | 0.1.0         | Server's status information / File LIST |           |
| STOR        | YES             | 0.1.0         | Store a file (client's upload). |           |
| STOU        | YES             | 0.1.0         | Store a file with a unique name. |           |
| STRU        | YES             | 0.1.0         | Set file structure. | Supports only File type structure by doing a NOOP (no operation). Other structure types (Record and Page) are not implemented. |
| SYST        | YES             | 0.1.0         | Get system type. | Always return "UNIX Type: L8" because of the LIST output provided. |
| TYPE        | YES             | 0.1.0         | Set current type (Binary/ASCII). | Accept only Binary and ASII TYPEs. Other TYPEs such as EBCDIC are obsoleted, system-dependent and thus not implemented. |
| USER        | YES             | 0.1.0         | Set user.       | A new USER command could be entered at any point in order to change the access control flushing any user, password, and account information already supplied and beginning the login sequence again. |

<br>

<h1>RFC-1123 - Requirements for Internet Hosts</h1>

Extends and clarifies some aspects of <a href='http://www.ietf.org/rfc/rfc959.txt'>RFC-959</a>. Introduces new response codes 554 and 555.<br>
<br>
<ul><li>Issued: October 1989<br>
</li><li>Status: STANDARD<br>
</li><li><a href='http://www.ietf.org/rfc/rfc1123.txt'>Link</a></li></ul>

<table><thead><th> <b>Feature</b> </th><th> <b>Implemented</b> </th><th> <b>Milestone</b> </th><th> <b>Description</b> </th><th> <b>Notes</b> </th></thead><tbody>
<tr><td> TYPE L 8 as synonym of TYPE I </td><td> YES                </td><td> 0.2.0            </td><td> TYPE L 8 command should be treated as synonym of TYPE I ("IMAGE" or binary type). </td><td>              </td></tr>
<tr><td> PASV is per-transfer </td><td> YES                </td><td> 0.1.0            </td><td> PASV must be used for a unique transfer. </td><td> If PASV is issued twice data-channel is restarted. </td></tr>
<tr><td> Implied type for LIST and NLST </td><td> YES                </td><td> 0.1.0            </td><td> The data returned by a LIST or NLST command SHOULD use an implied TYPE AN. </td><td>              </td></tr>
<tr><td> STOU format output </td><td> YES                </td><td> 0.2.0            </td><td> Defined the exact format output which STOU response must respect ("125/150 FILE filename"). </td><td>              </td></tr>
<tr><td> Avoid 250 response type on STOU </td><td> YES                </td><td> 0.2.0            </td><td> The 250 positive response indicated in <a href='http://www.ietf.org/rfc/rfc959.txt'>RFC-959</a> has been declared incorrect in <a href='http://www.ietf.org/rfc/rfc1123.txt'>RFC-1123</a> which requires 125/150 instead. </td><td>              </td></tr>
<tr><td> Handle "Experimental" directory cmds </td><td> YES                </td><td> 0.1.0            </td><td> The server should support XCUP, XCWD, XMKD, XPWD and XRMD obsoleted commands and treat them as synonyms for CDUP, CWD, MKD, LIST and RMD commands. </td><td>              </td></tr>
<tr><td> Idle timeout   </td><td> YES                </td><td> 0.5.0            </td><td> A Server-FTP process SHOULD have a configurable  idle timeout of 5 minutes, which will terminate the process and close the control connection if the server is inactive (i.e., no command or data transfer in progress) for a long period of time. </td><td>              </td></tr>
<tr><td> Concurrency of data and control </td><td> YES                </td><td> 0.1.0            </td><td> Server-FTP should be able to process STAT or ABOR while a data transfer is in progress </td><td> Feature granted natively for ALL commands since we're in an asynchronous environment. </td></tr>
<tr><td> 554 response on wrong REST </td><td> YES                </td><td> 0.2.0            </td><td> Return a 554 reply may for a command that follows a REST command.  The reply indicates that the existing file at the Server-FTP cannot be repositioned as specified in the REST. </td><td>              </td></tr></tbody></table>

<br>

<h1>RFC-2228 - FTP Security Extensions</h1>

Specifies several security extensions to the base FTP protocol defined in <a href='http://www.ietf.org/rfc/rfc959.txt'>RFC-959</a>. New commands: AUTH, ADAT, PROT, PBSZ, CCC, MIC, CONF, and ENC. New response codes: 232, 234, 235, 334, 335, 336, 431, 533, 534, 535, 536, 537, 631, 632, and 633.<br>
<ul><li>Issued: October 1997<br>
</li><li>Status: PROPOSED STANDARD<br>
</li><li>Updates: <a href='http://www.ietf.org/rfc/rfc959.txt'>RFC-959</a>
</li><li><a href='http://www.ietf.org/rfc/rfc2228.txt'>Link</a></li></ul>

<table><thead><th> <b>Command</b> </th><th> <b>Implemented</b> </th><th> <b>Milestone</b> </th><th> <b>Description</b> </th><th> <b>Notes</b> </th></thead><tbody>
<tr><td> AUTH           </td><td> NO                 </td><td> ---              </td><td> Authentication/Security Mechanism. </td><td> Implemented as <a href='http://code.google.com/p/pyftpdlib/source/browse/trunk/demo/tls_ftpd.py'>demo script</a> by following the <a href='http://www.ietf.org/rfc/rfc4217.txt'>RFC-4217</a> guide line. </td></tr>
<tr><td> CCC            </td><td> NO                 </td><td> ---              </td><td> Clear Command Channel. </td><td>              </td></tr>
<tr><td> CONF           </td><td> NO                 </td><td> ---              </td><td> Confidentiality Protected Command. </td><td> Somewhat obsoleted by <a href='http://www.ietf.org/rfc/rfc4217.txt'>RFC-4217</a>. </td></tr>
<tr><td> EENC           </td><td> NO                 </td><td> ---              </td><td> Privacy Protected Command. </td><td> Somewhat obsoleted by <a href='http://www.ietf.org/rfc/rfc4217.txt'>RFC-4217</a>. </td></tr>
<tr><td> MIC            </td><td> NO                 </td><td> ---              </td><td> Integrity Protected Command. </td><td> Somewhat obsoleted by <a href='http://www.ietf.org/rfc/rfc4217.txt'>RFC-4217</a>. </td></tr>
<tr><td> PBSZ           </td><td> NO                 </td><td> ---              </td><td> Protection Buffer Size. </td><td> Implemented as <a href='http://code.google.com/p/pyftpdlib/source/browse/trunk/demo/tls_ftpd.py'>demo script</a> by following the <a href='http://www.ietf.org/rfc/rfc4217.txt'>RFC-4217</a> guide line as a no-op command. </td></tr>
<tr><td> PROT           </td><td> NO                 </td><td> ---              </td><td> Data Channel Protection Level. </td><td> Implemented as <a href='http://code.google.com/p/pyftpdlib/source/browse/trunk/demo/tls_ftpd.py'>demo script</a> by following the <a href='http://www.ietf.org/rfc/rfc4217.txt'>RFC-4217</a> guide line supporting only "P" and "C" protection levels. </td></tr></tbody></table>

<br>

<h1>RFC-2389 - Feature negotiation mechanism for the File Transfer Protocol</h1>

Introduces the new FEAT and OPTS commands.<br>
<ul><li>Issued: August 1998<br>
</li><li>Status: PROPOSED STANDARD<br>
</li><li><a href='http://www.ietf.org/rfc/rfc2389.txt'>Link</a></li></ul>

<table><thead><th> <b>Command</b> </th><th> <b>Implemented</b> </th><th> <b>Milestone</b> </th><th> <b>Description</b> </th><th> <b>Notes</b> </th></thead><tbody>
<tr><td> FEAT           </td><td> YES                </td><td> 0.3.0            </td><td> List new supported commands subsequent <a href='http://www.ietf.org/rfc/rfc959.txt'>RFC-959</a> </td><td>              </td></tr>
<tr><td> OPTS           </td><td> YES                </td><td> 0.3.0            </td><td> Set options for certain commands. </td><td> MLST is the only command which could be used with OPTS. </td></tr></tbody></table>

<br>

<h1>RFC-2428 - FTP Extensions for IPv6 and NATs</h1>

Introduces the new commands EPRT and EPSV extending FTP to enable its use over various network protocols, and the new response codes 522 and 229.<br>
<ul><li>Issued: September 1998<br>
</li><li>Status: PROPOSED STANDARD<br>
</li><li><a href='http://www.ietf.org/rfc/rfc2428.txt'>Link</a></li></ul>

<table><thead><th> <b>Command</b> </th><th> <b>Implemented</b> </th><th> <b>Milestone</b> </th><th> <b>Description</b> </th><th> <b>Notes</b> </th></thead><tbody>
<tr><td> EPRT           </td><td> YES                </td><td> 0.4.0            </td><td> Set active data connection over IPv4 or IPv6 </td><td>              </td></tr>
<tr><td> EPSV           </td><td> YES                </td><td> 0.4.0            </td><td> Set passive data connection over IPv4 or IPv6 </td><td>              </td></tr></tbody></table>

<br>

<h1>RFC-2577 - FTP Security Considerations</h1>

Provides several configuration and implementation suggestions to mitigate some security concerns, including limiting failed password attempts and third-party "proxy FTP" transfers, which can be used in "bounce attacks".<br>
<ul><li>Issued: May 1999<br>
</li><li>Status: INFORMATIONAL<br>
</li><li><a href='http://www.ietf.org/rfc/rfc2577.txt'>Link</a></li></ul>

<table><thead><th> <b>Feature</b> </th><th> <b>Implemented</b> </th><th> <b>Milestone</b> </th><th> <b>Description</b> </th><th> <b>Notes</b> </th></thead><tbody>
<tr><td> FTP bounce protection </td><td> YES                </td><td> 0.2.0            </td><td> Reject PORT if IP address specified in it does not match client IP address. Drop the incoming (PASV) data connection for the same reason. </td><td> Configurable. </td></tr>
<tr><td> Restrict PASV/PORT to non privileged ports </td><td> YES                </td><td> 0.2.0            </td><td> Reject connections to privileged ports. </td><td> Configurable. </td></tr>
<tr><td> Brute force protection (1) </td><td> YES                </td><td> 0.1.0            </td><td> Disconnect client after a certain number (3 or 5) of wrong authentications. </td><td> Configurable. </td></tr>
<tr><td> Brute force protection (2) </td><td> YES                </td><td> 0.5.0            </td><td> Impose a 5 second delay before replying to an invalid "PASS" command to diminish the efficiency of a brute force attack.  </td><td>              </td></tr>
<tr><td> Per-source-IP limit </td><td> YES                </td><td> 0.2.0            </td><td> Limit the total number of per-ip control connections to avoid parallel brute-force attack attempts. </td><td> Configurable. </td></tr>
<tr><td> Do not reject wrong usernames </td><td> YES                </td><td> 0.1.0            </td><td> Always return 331 to the USER command to prevent client from determining valid usernames on the server. </td><td>              </td></tr>
<tr><td> Port stealing protection </td><td> YES                </td><td> 0.1.1            </td><td> Use random-assigned local ports for data connections.  </td><td>              </td></tr></tbody></table>

<br>

<h1>RFC-2640 - Internationalization of the File Transfer Protocol</h1>

Extends the FTP protocol to support multiple character sets, in addition to the original 7-bit ASCII. Introduces the new LANG command.<br>
<ul><li>Issued: July 1999<br>
</li><li>Status: PROPOSED STANDARD<br>
</li><li>Updates: <a href='http://www.ietf.org/rfc/rfc959.txt'>RFC-959</a>
</li><li><a href='http://www.ietf.org/rfc/rfc2640.txt'>Link</a></li></ul>

<table><thead><th> <b>Feature</b> </th><th> <b>Implemented</b> </th><th> <b>Milestone</b> </th><th> <b>Description</b> </th><th> <b>Notes</b> </th></thead><tbody>
<tr><td> LANG command   </td><td> NO                 </td><td> ---              </td><td> Set current response's language. </td><td>              </td></tr>
<tr><td> Support for UNICODE  </td><td> YES                </td><td> 1.0.0            </td><td> For support of global compatibility it is rencommended that clients and servers use UTF-8 encoding when exchanging pathnames. </td><td>              </td></tr></tbody></table>

<br>

<h1>RFC-3659 - Extensions to FTP</h1>

Four new commands are added: "SIZE", "MDTM", "MLST", and "MLSD".  The existing command "REST" is modified.<br>
<ul><li>Issued: March 2007<br>
</li><li>Status: PROPOSED STANDARD<br>
</li><li>Updates: <a href='http://www.ietf.org/rfc/rfc959.txt'>RFC-959</a>
</li><li><a href='http://www.ietf.org/rfc/rfc3659.txt'>Link</a></li></ul>

<table><thead><th> <b>Feature</b> </th><th> <b>Implemented</b> </th><th> <b>Milestone</b> </th><th> <b>Description</b> </th><th> <b>Notes</b> </th></thead><tbody>
<tr><td> MDTM command   </td><td> YES                </td><td> 0.1.0            </td><td> Get file's last modification time </td><td>              </td></tr>
<tr><td> MLSD command   </td><td> YES                </td><td> 0.3.0            </td><td> Get directory list in a standardized form. </td><td>              </td></tr>
<tr><td> MLST command   </td><td> YES                </td><td> 0.3.0            </td><td> Get file information in a standardized form. </td><td>              </td></tr>
<tr><td> SIZE command   </td><td> YES                </td><td> 0.1.0            </td><td> Get file size.     </td><td> In case of ASCII TYPE it does not perform the ASCII conversion to avoid DoS conditions (see FAQs for more details). </td></tr>
<tr><td> TVSF mechanism </td><td> YES                </td><td> 0.1.0            </td><td> Provide a file system naming conventions modeled loosely upon those of the Unix file system supporting relative and absolute path names. </td><td>              </td></tr>
<tr><td> Minimum required set of MLST facts </td><td> YES                </td><td> 0.3.0            </td><td> If conceivably possible, support at least the type, perm, size, unique, and modify MLSX command facts. </td><td>              </td></tr>
<tr><td> GMT should be used for timestamps </td><td> YES                </td><td> 0.6.0            </td><td> All times reported by MDTM, LIST, MLSD and MLST commands must be in GMT times </td><td> Possibility to change time display between GMT and local time provided as <code>FTPHandler.use_gmt_times</code> attribute </td></tr></tbody></table>

<br>

<h1>RFC-4217 - Securing FTP with TLS</h1>

Provides a description on how to implement TLS as a security mechanism to secure FTP clients and/or servers.<br>
<br>
<ul><li>Issued: October 2005<br>
</li><li>Status: STANDARD<br>
</li><li>Updates: <a href='http://www.ietf.org/rfc/rfc959.txt'>RFC-959</a>, <a href='http://www.ietf.org/rfc/rfc2246.txt'>RFC-2246</a>, <a href='http://www.ietf.org/rfc/rfc2228.txt'>RFC-2228</a>
</li><li><a href='http://www.ietf.org/rfc/rfc4217.txt'>Link</a></li></ul>

<table><thead><th> <b>Command</b> </th><th> <b>Implemented</b> </th><th> <b>Milestone</b> </th><th> <b>Description</b> </th><th> <b>Notes</b> </th></thead><tbody>
<tr><td> AUTH           </td><td> YES                </td><td> ---              </td><td> Authentication/Security Mechanism. </td><td>              </td></tr>
<tr><td> CCC            </td><td> NO                 </td><td> ---              </td><td> Clear Command Channel. </td><td>              </td></tr>
<tr><td> PBSZ           </td><td> YES                </td><td> ---              </td><td> Protection Buffer Size. </td><td> Implemented as as a no-op as recommended. </td></tr>
<tr><td> PROT           </td><td> YES                </td><td> ---              </td><td> Data Channel Protection Level. </td><td> Support only "P" and "C" protection levels. </td></tr></tbody></table>

<h1>Unofficial commands</h1>

These are commands not officialy included in any RFC but many FTP servers implement them.<br>
<br>
<table><thead><th> <b>Command</b> </th><th> <b>Implemented</b> </th><th> <b>Milestone</b> </th><th> <b>Description</b> </th><th> <b>Notes</b> </th></thead><tbody>
<tr><td> SITE CHMOD command </td><td> YES                </td><td> 0.7.0            </td><td> Change file mode.  </td><td>              </td></tr>