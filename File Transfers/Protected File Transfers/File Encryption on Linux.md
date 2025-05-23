[OpenSSL](https://www.openssl.org/) is frequently included in Linux distributions, with sysadmins using it to generate security certificates, among other tasks. OpenSSL can be used to send files "nc style" to encrypt files.

To encrypt a file using `openssl` we can select different ciphers, see [OpenSSL man page](https://www.openssl.org/docs/man1.1.1/man1/openssl-enc.html). Let's use `-aes256` as an example. We can also override the default iterations counts with the option `-iter 100000` and add the option `-pbkdf2` to use the Password-Based Key Derivation Function 2 algorithm. When we hit enter, we'll need to provide a password.

#### Encrypting /etc/passwd with openssl

  Protected File Transfers

```shell-session
pepedavidphone@htb[/htb]$ openssl enc -aes256 -iter 100000 -pbkdf2 -in /etc/passwd -out passwd.enc

enter aes-256-cbc encryption password:                                                         
Verifying - enter aes-256-cbc encryption password:                              
```

Remember to use a strong and unique password to avoid brute-force cracking attacks should an unauthorized party obtain the file. To decrypt the file, we can use the following command:

#### Decrypt passwd.enc with openssl

  Protected File Transfers

```shell-session
pepedavidphone@htb[/htb]$ openssl enc -d -aes256 -iter 100000 -pbkdf2 -in passwd.enc -out passwd                    

enter aes-256-cbc decryption password:
```

We can use any of the previous methods to transfer this file, but it's recommended to use a secure transport method such as HTTPS, SFTP, or SSH. As always, practice the examples in this section against target hosts in this or other modules and reproduce what you can (such as the `openssl` examples using the Pwnbox. The following section will cover different ways to transfer files over HTTP and HTTPS.