https://web.archive.org/web/20231116172005/https://www.crackmapexec.wiki/https://web.archive.org/web/20231116172005/https://www.crackmapexec.wiki/


#### CrackMapExec Usage

The general format for using CrackMapExec is as follows:

```shell-session
$ crackmapexec smb -h
crackmapexec  <proto> -h
```
```shell-session
[!bash!]$ crackmapexec <proto> <target-IP> -u <user or userlist> -p <password or passwordlist>
```


# Password Mutations

#### Password List

  Password Mutations

```shell-session
pepedavidphone@htb[/htb]$ cat password.list

password
```

We can use a very powerful tool called [Hashcat](https://hashcat.net/hashcat/) to combine lists of potential names and labels with specific mutation rules to create custom wordlists. To become more familiar with Hashcat and discover the full potential of this tool, we recommend the module [Cracking Passwords with Hashcat](https://academy.hackthebox.com/course/preview/cracking-passwords-with-hashcat). Hashcat uses a specific syntax for defining characters and words and how they can be modified. The complete list of this syntax can be found in the official [documentation](https://hashcat.net/wiki/doku.php?id=rule_based_attack) of Hashcat. However, the ones listed below are enough for us to understand how Hashcat mutates words.

|**Function**|**Description**|
|---|---|
|`:`|Do nothing.|
|`l`|Lowercase all letters.|
|`u`|Uppercase all letters.|
|`c`|Capitalize the first letter and lowercase others.|
|`sXY`|Replace all instances of X with Y.|
|`$!`|Add the exclamation character at the end.|
#### Generating Rule-based Wordlist

  Password Mutations

```shell-session
pepedavidphone@htb[/htb]$ hashcat --force password.list -r custom.rule --stdout | sort -u > mut_password.list
pepedavidphone@htb[/htb]$ cat mut_password.list
```
#### Hashcat Existing Rules

```shell-session
pepedavidphone@htb[/htb]$ ls /usr/share/hashcat/rules/
```

We can now use another tool called CeWL to scan potential words from the company's website and save them in a separate list. We can then combine this list with the desired rules and create a customized password list that has a higher probability of guessing a correct password. We specify some parameters, like the depth to spider (-d), the minimum length of the word (-m), the storage of the found words in lowercase (--lowercase), as well as the file where we want to store the results (-w).

## Generating Wordlists Using CeWL
 
pepedavidphone@htb[/htb]$ cewl https://www.inlanefreight.com -d 4 -m 6 --lowercase -w inlane.wordlist
pepedavidphone@htb[/htb]$ wc -l inlane.wordlist