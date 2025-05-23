![[Pasted image 20250422174545.png]]
empezamos con nmap tenemos 
![[Pasted image 20250422174632.png]]
Despues de mirar por toda la pagina web lo unico que encontramos es un dominio que es Board.htb nada mas
![[Pasted image 20250422174723.png]]
lo agregamos a /etc/hosts/![[Pasted image 20250422174826.png]]

redirige correctamente el dominio ![[Pasted image 20250422174847.png]]

En la pagina no hay nada mas es todo estatico a aparentemente asique realizamos enumeracion de subdominios.

encontramos ![[Pasted image 20250422175025.png]]

tenemos un panel de login de una cms llamado dolibarr ![[Pasted image 20250422175124.png]]
admin:admin son las credenciales default de esta pagina dandonos acceso a un panel de login 
![[Pasted image 20250422175216.png]]
![[Pasted image 20250422175251.png]]
la version de esta app es la 17.0.0 Dolibarr buscaremos algun exploit 

encontramos este CVE ![[Pasted image 20250422175616.png]]

https://github.com/nikn0laty/Exploit-for-Dolibarr-17.0.0-CVE-2023-30253

el anterior no parece que funcione bien pero este especificamente si explicacion muy detallada de como funciona la vulnerabilidad y si la quisieramos explotar manualmente ![[Pasted image 20250422175949.png]]

https://github.com/dollarboysushil/Dolibarr-17.0.0-Exploit-CVE-2023-30253

tenemos shell ![[Pasted image 20250422180154.png]]

![[Pasted image 20250422180202.png]]

conseguimos una buena shell 
![[Pasted image 20250422180314.png]]

En este archivo encontramos credenciales de una base de datos 
/var/www/html/crm.board.htb/htdocs/conf/conf.php
![[Pasted image 20250422180506.png]]
ssh larissa@10.10.11.11  serverfun2$2023!! 
tenemos acceso por ssh ![[Pasted image 20250422180714.png]]
El sistema es vulnerable al **[CVE-2022-37706](https://github.com/junnythemarksman/CVE-2022-37706)**

![[Pasted image 20250422181027.png]]


![[Pasted image 20250422181100.png]]

Tenemos root