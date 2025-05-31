Hunting for credentials is one of the first steps once we have access to the system. These low-hanging fruits can give us elevated privileges within seconds or minutes. Among other things, this is part of the local privilege escalation process that we will cover here. However, it is important to note here that we are far from covering all possible situations and therefore focus on the different approaches.

We can imagine that we have successfully gained access to a system via a vulnerable web application and have therefore obtained a reverse shell, for example. Therefore, to escalate our privileges most efficiently, we can search for passwords or even whole credentials that we can use to log in to our target. There are several sources that can provide us with credentials that we put in four categories. These include, but are not limited to:

|**`Files`**|**`History`**|**`Memory`**|**`Key-Rings`**|
|---|---|---|---|
|Configs|Logs|Cache|Browser stored credentials|
|Databases|Command-line History|In-memory Processing||
|Notes||||
|Scripts||||
|Source codes||||
|Cronjobs||||
|SSH Keys|
Enumerating all these categories will allow us to increase the probability of successfully finding out with some ease credentials of existing users on the system. There are countless different situations in which we will always see different results. Therefore, we should adapt our approach to the circumstances of the environment and keep the big picture in mind. Above all, it is crucial to keep in mind how the system works, its focus, what purpose it exists for, and what role it plays in the business logic and the overall network. For example, suppose it is an isolated database server. In that case, we will not necessarily find normal users there since it is a sensitive interface in the management of data to which only a few people are granted access.

---

## Files

One core principle of Linux is that everything is a file. Therefore, it is crucial to keep this concept in mind and search, find and filter the appropriate files according to our requirements. We should look for, find, and inspect several categories of files one by one. These categories are the following:

![[Pasted image 20250527153255.png]]

Configuration files are the core of the functionality of services on Linux distributions. Often they even contain credentials that we will be able to read. Their insight also allows us to understand how the service works and its requirements precisely. Usually, the configuration files are marked with the following three file extensions (`.config`, `.conf`, `.cnf`). However, these configuration files or the associated extension files can be renamed, which means that these file extensions are not necessarily required. Furthermore, even when recompiling a service, the required filename for the basic configuration can be changed, which would result in the same effect. However, this is a rare case that we will not encounter often, but this possibility should not be left out of our search.

The most crucial part of any system enumeration is to obtain an overview of it. Therefore, the first step should be to find all possible configuration files on the system, which we can then examine and analyze individually in more detail. There are many methods to find these configuration files, and with the following method, we will see we have reduced our search to these three file extensions.

#### Configuration Files

```shell-session
cry0l1t3@unixclient:~$ for l in $(echo ".conf .config .cnf");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "lib\|fonts\|share\|core" ;done
```

Optionally, we can save the result in a text file and use it to examine the individual files one after the other. Another option is to run the scan directly for each file found with the specified file extension and output the contents. In this example, we search for three words (`user`, `password`, `pass`) in each file with the file extension `.cnf`.

#### Credentials in Configuration Files

```shell-session
cry0l1t3@unixclient:~$ for i in $(find / -name *.cnf 2>/dev/null | grep -v "doc\|lib");do echo -e "\nFile: " $i; grep "user\|password\|pass" $i 2>/dev/null | grep -v "\#";done
```
We can apply this simple search to the other file extensions as well. Additionally, we can apply this search type to databases stored in files with different file extensions, and we can then read those.

#### Databases
```shell-session
cry0l1t3@unixclient:~$ for l in $(echo ".sql .db .*db .db*");do echo -e "\nDB File extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share\|man";done
```
Depending on the environment we are in and the purpose of the host we are on, we can often find notes about specific processes on the system. These often include lists of many different access points or even their credentials. However, it is often challenging to find notes right away if stored somewhere on the system and not on the desktop or in its subfolders. This is because they can be named anything and do not have to have a specific file extension, such as `.txt`. Therefore, in this case, we need to search for files including the `.txt` file extension and files that have no file extension at all.

#### Notes

```shell-session
cry0l1t3@unixclient:~$ find /home/* -type f -name "*.txt" -o ! -name "*.*"
```
Scripts are files that often contain highly sensitive information and processes. Among other things, these also contain credentials that are necessary to be able to call up and execute the processes automatically. Otherwise, the administrator or developer would have to enter the corresponding password each time the script or the compiled program is called.

#### Scripts

```shell-session
cry0l1t3@unixclient:~$ for l in $(echo ".py .pyc .pl .go .jar .c .sh");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share";done
```


Cronjobs are independent execution of commands, programs, scripts. These are divided into the system-wide area (`/etc/crontab`) and user-dependent executions. Some applications and scripts require credentials to run and are therefore incorrectly entered in the cronjobs. Furthermore, there are the areas that are divided into different time ranges (`/etc/cron.daily`, `/etc/cron.hourly`, `/etc/cron.monthly`, `/etc/cron.weekly`). The scripts and files used by `cron` can also be found in `/etc/cron.d/` for Debian-based distributions.

#### Cronjobs

```shell-session
cry0l1t3@unixclient:~$ cat /etc/crontab 
```

```shell-session
cry0l1t3@unixclient:~$ ls -la /etc/cron.*/
```

#### SSH Keys

SSH keys can be considered "access cards" for the SSH protocol used for the public key authentication mechanism. A file is generated for the client (`Private key`) and a corresponding one for the server (`Public key`). However, these are not the same, so knowing the `public key` is insufficient to find a `private key`. The `public key` can verify signatures generated by the private SSH key and thus enables automatic login to the server. Even if unauthorized persons get hold of the public key, it is almost impossible to calculate the matching private one from it. When connecting to the server using the private SSH key, the server checks whether the private key is valid and lets the client log in accordingly. Thus, passwords are no longer needed to connect via SSH.

Since the SSH keys can be named arbitrarily, we cannot search them for specific names. However, their format allows us to identify them uniquely because, whether public key or private key, both have unique first lines to distinguish them.

#### SSH Private Keys

```shell-session
cry0l1t3@unixclient:~$ grep -rnw "PRIVATE KEY" /home/* 2>/dev/null | grep ":1"
```
#### SSH Public Keys

```shell-session
cry0l1t3@unixclient:~$ grep -rnw "ssh-rsa" /home/* 2>/dev/null | grep ":1"
```
## History

All history files provide crucial information about the current and past/historical course of processes. We are interested in the files that store users' command history and the logs that store information about system processes.

In the history of the commands entered on Linux distributions that use Bash as a standard shell, we find the associated files in `.bash_history`. Nevertheless, other files like `.bashrc` or `.bash_profile` can contain important information.

#### Bash History

```shell-session
cry0l1t3@unixclient:~$ tail -n5 /home/*/.bash*
```
#### Logs

An essential concept of Linux systems is log files that are stored in text files. Many programs, especially all services and the system itself, write such files. In them, we find system errors, detect problems regarding services or follow what the system is doing in the background. The entirety of log files can be divided into four categories:

|**Application Logs**|**Event Logs**|**Service Logs**|**System Logs**|
|---|---|---|---|

Many different logs exist on the system. These can vary depending on the applications installed, but here are some of the most important ones:

|**Log File**|**Description**|
|---|---|
|`/var/log/messages`|Generic system activity logs.|
|`/var/log/syslog`|Generic system activity logs.|
|`/var/log/auth.log`|(Debian) All authentication related logs.|
|`/var/log/secure`|(RedHat/CentOS) All authentication related logs.|
|`/var/log/boot.log`|Booting information.|
|`/var/log/dmesg`|Hardware and drivers related information and logs.|
|`/var/log/kern.log`|Kernel related warnings, errors and logs.|
|`/var/log/faillog`|Failed login attempts.|
|`/var/log/cron`|Information related to cron jobs.|
|`/var/log/mail.log`|All mail server related logs.|
|`/var/log/httpd`|All Apache related logs.|
|`/var/log/mysqld.log`|All MySQL server related logs.|

Covering the analysis of these log files in detail would be inefficient in this case. So at this point, we should familiarize ourselves with the individual logs, first examining them manually and understanding their formats. However, here are some strings we can use to find interesting content in the logs:

```shell-session
cry0l1t3@unixclient:~$ for i in $(ls /var/log/* 2>/dev/null);do GREP=$(grep "accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|COMMAND\=\|logs" $i 2>/dev/null); if [[ $GREP ]];then echo -e "\n#### Log file: " $i; grep "accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|COMMAND\=\|logs" $i 2>/dev/null;fi;done
```

## Memory and Cache

Many applications and processes work with credentials needed for authentication and store them either in memory or in files so that they can be reused. For example, it may be the system-required credentials for the logged-in users. Another example is the credentials stored in the browsers, which can also be read. In order to retrieve this type of information from Linux distributions, there is a tool called [mimipenguin](https://github.com/huntergregal/mimipenguin) that makes the whole process easier. However, this tool requires administrator/root permissions.

#### Memory - Mimipenguin

```shell-session
cry0l1t3@unixclient:~$ sudo python3 mimipenguin.py
[sudo] password for cry0l1t3: 

[SYSTEM - GNOME]	cry0l1t3:WLpAEXFa0SbqOHY


cry0l1t3@unixclient:~$ sudo bash mimipenguin.sh 
[sudo] password for cry0l1t3: 

MimiPenguin Results:
[SYSTEM - GNOME]          cry0l1t3:WLpAEXFa0SbqOHY
```

An even more powerful tool we can use that was mentioned earlier in the Credential Hunting in Windows section is `LaZagne`. This tool allows us to access far more resources and extract the credentials. The passwords and hashes we can obtain come from the following sources but are not limited to:

## 🗂️ Tabla de Herramientas y Componentes

| 🧠 Categoría        | 🔐 Credenciales / Configs     | 🌐 Navegadores / Clientes | 🛠️ Sistema / Servicios     |
|--------------------|-------------------------------|---------------------------|-----------------------------|
| Wi-Fi              | Wpa_supplicant                | Chromium-based            | Grub                        |
| Git                | Env_variable                  | CLI                       | Fstab                       |
| AWS                | Libsecret / Kwallet / Keyrings| Mozilla / Thunderbird     | SSH                         |
| Apache             | Shadow                        | Filezilla / Gftp          | Docker                      |
| KeePass            | Mimipy                        |                           |                             |
| Sessions           |                               |                           |                             |


For example, `Keyrings` are used for secure storage and management of passwords on Linux distributions. Passwords are stored encrypted and protected with a master password. It is an OS-based password manager, which we will discuss later in another section. This way, we do not need to remember every single password and can save repeated password entries.

#### Memory - LaZagne

```shell-session
cry0l1t3@unixclient:~$ sudo python2.7 laZagne.py all
```
![[Pasted image 20250527153659.png]]

#### Browsers

Browsers store the passwords saved by the user in an encrypted form locally on the system to be reused. For example, the `Mozilla Firefox` browser stores the credentials encrypted in a hidden folder for the respective user. These often include the associated field names, URLs, and other valuable information.

For example, when we store credentials for a web page in the Firefox browser, they are encrypted and stored in `logins.json` on the system. However, this does not mean that they are safe there. Many employees store such login data in their browser without suspecting that it can easily be decrypted and used against the company.

#### Firefox Stored Credentials

```shell-session
cry0l1t3@unixclient:~$ ls -l .mozilla/firefox/ | grep default 
```

```shell-session
cry0l1t3@unixclient:~$ cat .mozilla/firefox/1bplpd86.default-release/logins.json | jq .
```
The tool [Firefox Decrypt](https://github.com/unode/firefox_decrypt) is excellent for decrypting these credentials, and is updated regularly. It requires Python 3.9 to run the latest version. Otherwise, `Firefox Decrypt 0.7.0` with Python 2 must be used.

#### Decrypting Firefox Credentials

```shell-session
pepedavidphone@htb[/htb]$ python3.9 firefox_decrypt.py
```
Alternatively, `LaZagne` can also return results if the user has used the supported browser.

#### Browsers - LaZagne

```shell-session
cry0l1t3@unixclient:~$ python3 laZagne.py browsers
```