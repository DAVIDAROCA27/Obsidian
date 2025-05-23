## RDP

If we are connected from Linux, we can use `xfreerdp` or `rdesktop`. At the time of writing, `xfreerdp` and `rdesktop` allow copy from our target machine to the RDP session, but there may be scenarios where this may not work as expected.

As an alternative to copy and paste, we can mount a local resource on the target RDP server. `rdesktop` or `xfreerdp` can be used to expose a local folder in the remote RDP session.

#### Mounting a Linux Folder Using rdesktop

  Miscellaneous File Transfer Methods

```shell-session
pepedavidphone@htb[/htb]$ rdesktop 10.10.10.132 -d HTB -u administrator -p 'Password0@' -r disk:linux='/home/user/rdesktop/files'
```

#### Mounting a Linux Folder Using xfreerdp

  Miscellaneous File Transfer Methods

```shell-session
pepedavidphone@htb[/htb]$ xfreerdp /v:10.10.10.132 /d:HTB /u:administrator /p:'Password0@' /drive:linux,/home/plaintext/htb/academy/filetransfer
```

To access the directory, we can connect to `\\tsclient\`, allowing us to transfer files to and from the RDP session.

![[Pasted image 20250422161541.png]]
Alternatively, from Windows, the native [mstsc.exe](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/mstsc) remote desktop client can be used.

![[Pasted image 20250422161552.png]]
After selecting the drive, we can interact with it in the remote session that follows.