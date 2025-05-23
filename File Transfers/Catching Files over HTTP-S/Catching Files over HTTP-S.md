Create Web server using Apache or Nginx
## HTTP/S

Web transfer is the most common way most people transfer files because `HTTP`/`HTTPS` are the most common protocols allowed through firewalls. Another immense benefit is that, in many cases, the file will be encrypted in transit. There is nothing worse than being on a penetration test, and a client's network IDS picks up on a sensitive file being transferred over plaintext and having them ask why we sent a password to our cloud server without using encryption.

We have already discussed using the Python3 [uploadserver module](https://github.com/Densaugeo/uploadserver) to set up a web server with upload capabilities, but we can also use Apache or Nginx. This section will cover creating a secure web server for file upload operations.


## Nginx - Enabling PUT

A good alternative for transferring files to `Apache` is [Nginx](https://www.nginx.com/resources/wiki/) because the configuration is less complicated, and the module system does not lead to security issues as `Apache` can.

When allowing `HTTP` uploads, it is critical to be 100% positive that users cannot upload web shells and execute them. `Apache` makes it easy to shoot ourselves in the foot with this, as the `PHP` module loves to execute anything ending in `PHP`. Configuring `Nginx` to use PHP is nowhere near as simple.
#### Create a Directory to Handle Uploaded Files
```shell-session
pepedavidphone@htb[/htb]$ sudo mkdir -p /var/www/uploads/SecretUploadDirectory
```
#### Change the Owner to www-data
```shell-session
pepedavidphone@htb[/htb]$ sudo chown -R www-data:www-data /var/www/uploads/SecretUploadDirectory
```
#### Create Nginx Configuration File

Create the Nginx configuration file by creating the file `/etc/nginx/sites-available/upload.conf` with the contents:

```shell-session
server {
    listen 9001;
    
    location /SecretUploadDirectory/ {
        root    /var/www/uploads;
        dav_methods PUT;
    }
}
```

#### Symlink our Site to the sites-enabled Directory

```shell-session
pepedavidphone@htb[/htb]$ sudo ln -s /etc/nginx/sites-available/upload.conf /etc/nginx/sites-enabled/
```
#### Start Nginx

```shell-session
pepedavidphone@htb[/htb]$ sudo systemctl restart nginx.service
```

#### Verifying Errors

```shell-session
pepedavidphone@htb[/htb]$ tail -2 /var/log/nginx/error.log

2020/11/17 16:11:56 [emerg] 5679#5679: bind() to 0.0.0.0:`80` failed (98: A`ddress already in use`)
2020/11/17 16:11:56 [emerg] 5679#5679: still could not bind()
```

```shell-session
pepedavidphone@htb[/htb]$ ss -lnpt | grep 80

pepedavidphone@htb[/htb]$ ps -ef | grep 2811

```

#### Remove NginxDefault Configuration

```shell-session
pepedavidphone@htb[/htb]$ sudo rm /etc/nginx/sites-enabled/default
```

#### Upload File Using cURL

```shell-session
pepedavidphone@htb[/htb]$ curl -T /etc/passwd http://localhost:9001/SecretUploadDirectory/users.txt
```
```shell-session
pepedavidphone@htb[/htb]$ sudo tail -1 /var/www/uploads/SecretUploadDirectory/users.txt 

user65:x:1000:1000:,,,:/home/user65:/bin/bash
```
