
## File Encryption on Windows

Many different methods can be used to encrypt files and information on Windows systems. One of the simplest methods is the [Invoke-AESEncryption.ps1](https://www.powershellgallery.com/packages/DRTools/4.0.2.3/Content/Functions%5CInvoke-AESEncryption.ps1) PowerShell script. This script is small and provides encryption of files and strings.

 #### Invoke-AESEncryption.ps1
 
 https://www.powershellgallery.com/packages/DRTools/4.0.2.3/Content/Functions%5CInvoke-AESEncryption.ps1
 
 
 We can use any previously shown file transfer methods to get this file onto a target host. After the script has been transferred, it only needs to be imported as a module, as shown below.
 
 #### Import Module Invoke-AESEncryption.ps1

  Protected File Transfers

```powershell-session
PS C:\htb> Import-Module .\Invoke-AESEncryption.ps1
```

After the script is imported, it can encrypt strings or files, as shown in the following examples. This command creates an encrypted file with the same name as the encrypted file but with the extension "`.aes`."

#### File Encryption Example

  Protected File Transfers

```powershell-session
PS C:\htb> Invoke-AESEncryption -Mode Encrypt -Key "p4ssw0rd" -Path .\scan-results.txt

File encrypted to C:\htb\scan-results.txt.aes
PS C:\htb> ls

    Directory: C:\htb

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        11/18/2020  12:17 AM           9734 Invoke-AESEncryption.ps1
-a----        11/18/2020  12:19 PM           1724 scan-results.txt
-a----        11/18/2020  12:20 PM           3448 scan-results.txt.aes
```

Using very `strong` and `unique` passwords for encryption for every company where a penetration test is performed is essential. This is to prevent sensitive files and information from being decrypted using one single password that may have been leaked and cracked by a third party.