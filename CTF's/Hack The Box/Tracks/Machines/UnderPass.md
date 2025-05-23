┌──(root㉿kali)-[/home/kali/Desktop/UnderPass]
└─# nmap 10.10.11.48                             
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-29 09:41 EDT
Nmap scan report for underpass.htb (10.10.11.48)
Host is up (0.048s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http


┌──(kali㉿kali)-[~/Desktop/UnderPass]
└─$ nmap -sU -p 161 10.10.11.48                                  
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-29 09:49 EDT
Nmap scan report for underpass.htb (10.10.11.48)
Host is up (0.048s latency).

PORT    STATE SERVICE
161/udp open  snmp

Nmap done: 1 IP address (1 host up) scanned in 0.36 seconds
                                                                                        
┌──(kali㉿kali)-[~/Desktop/UnderPass]
└─$ 
![[Pasted image 20250429154952.png]]
Ha primera vista con el escaneo solamente tenemos el puerto ssh que es seguro y la pagina por defecto de ubuntu.Aqui no hay absolutamente nada no vemos tampoco que haciendo fuzzing nos salga nada ni tengamos informacion escondida si corremos el escaneo con UDP vemos que el puerto 161 esta abierto y corriendo un servicio dentro de este 

![[Pasted image 20250429161608.png]]

Encontramos que esta corriendo daloradius una pagina web ahora hay que hacer fuzzing sobre fuzzing 
![[Pasted image 20250430083917.png]]

no tenemos permiso pero si que podemos seguri haciendo fuzzing y ir viendo si hay mas rutas.

![[Pasted image 20250429161520.png]]
/library              (Status: 301) [Size: 323] [--> http://10.10.11.48/daloradius/library/]                                                                                    
/doc                  (Status: 301) [Size: 319] [--> http://10.10.11.48/daloradius/doc/]
/app                  (Status: 301) [Size: 319] [--> http://10.10.11.48/daloradius/app/]
/contrib              (Status: 301) [Size: 323] [--> http://10.10.11.48/daloradius/contrib/]                                                                                    
/setup                (Status: 301) [Size: 321] [--> http://10.10.11.48/daloradius/setup/]   

de estas rutas la unica que contiene algo es http://10.10.11.48/daloradius/app

![[Pasted image 20250429162156.png]]
y http://10.10.11.48/daloradius/doc/install esta ruta parece que no lleva a ningun sitio 

#encontramos esta ruta que tiene un panel de login pero no tenemos aun credenciales 
http://10.10.11.48/daloradius/app/users/login.php


![[Pasted image 20250429162733.png]]
![[Pasted image 20250429162814.png]]

![[Pasted image 20250429162841.png]]
![[Pasted image 20250430084039.png]]
/app/users/login.php pero no tenemos credenciales ni podemos hacer nada

la ruta buena es esta ![[Pasted image 20250429163016.png]]
en esta ruta de aqui encontramos un panel de login que tiene por defecto las credenciales administrator:radius

![[Pasted image 20250430084255.png]]

svcMosh:098F6BCD4621D373CADE4E832627B4F6
svcMosh:underwaterfriends

Encontramos estas credenciales y ya podemos acceder por ssh 
![[Pasted image 20250430084403.png]]
svcMosh@underpass:~$ sudo -l
Matching Defaults entries for svcMosh on localhost:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User svcMosh may run the following commands on localhost:
    (ALL) NOPASSWD: /usr/bin/mosh-server
Buscando un poco vemos lo que hace este binario que hace posible que se levante un servidor como lo podemos ejecutar con root obtenemos acceso rapido a la maquina 

https://medium.com/@momo334678/mosh-server-sudo-privilege-escalation-82ef833bb246

este es el que segui para conseguirlo vamos a hacerlo en un momento 
![[Pasted image 20250430084912.png]]
┌──(kali㉿kali)-[~/Desktop/Nocturnal]
└─$ MOSH_KEY=kdd+ve1bO6aE0weCShsO8g mosh-client 10.10.11.48 60001


![[Pasted image 20250430085137.png]]