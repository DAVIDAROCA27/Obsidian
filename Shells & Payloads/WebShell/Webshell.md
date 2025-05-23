## Working with Laudanum

The Laudanum files can be found in the `/usr/share/laudanum` directory. For most of the files within Laudanum, you can copy them as-is and place them where you need them on the victim to run. For specific files such as the shells, you must edit the file first to insert your `attacking` host IP address to ensure you can access the web shell or receive a callback in the instance that you use a reverse shell. Before using the different files, be sure to read the contents and comments to ensure you take the proper actions.


```shell-session
pepedavidphone@htb[/htb]$ ls /usr/share/nishang/Antak-WebShell

antak.aspx  Readme.md
```



https://github.com/WhiteWinterWolf/wwwolf-php-webshell/blob/master/webshell.php