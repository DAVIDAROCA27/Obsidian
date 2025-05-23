
![[Pasted image 20250429175346.png]]
Yo en esta maquina me he vuelto loco a probar cosas 
![[Pasted image 20250430093803.png]]
Caemos aqui de primeras la pagina bastante estatica no hay mucha info solo tenemos un panel de registro y otro de login 
![[Pasted image 20250430093754.png]]
![[Pasted image 20250430093914.png]]

![[Pasted image 20250429175327.png]]

y ya aqui es cuando perdi la cabeza porque como tal la web no es vulnerable por el file upload no hay ejecucion de comandos no interpreta el codigo php no se pude hacer nada.

http://nocturnal.htb/view.php?file=uploads/php-reverse-shell.php.pdf

http://nocturnal.htb/view.php?username=ramon3&file=el-viejo-y-el-mar-ernest-hemingway.pdf

![[Pasted image 20250502165827.png]]

esto lleva a una ruta yo vi una cosa buena y esque al acceder a esta ruta de determinada forma me daba errores de usuario pero no cai en esto exactamente vemos que se interpreta el valor username=ramon3 si somos capaces de hacer fuzzing a esto![[Pasted image 20250430094844.png]]


Haciendo fuzzing de  usuarios  vemos que podemos llegar aqui.

![[Pasted image 20250502165934.png]]
Donde tenemos un archivo llamado privacy.odt de un usuario llamado amanda vamos  a revisar el contenido.

![[Pasted image 20250502170029.png]]

![[Pasted image 20250502170157.png]]
Vemos aqui informacion relevante para hacer login como admin aparentemente 

arHkG7HAI68X8s1J

![[Pasted image 20250502170234.png]]
vemos un panel de admin  aqui 

donde podemos  bajarnos una copia de seguridad de los archivos que hayan subidos a la plataforma ![[Pasted image 20250502170309.png]]

El archivo admin.php tiene varias vulnerabilidades como la ejecucion de comandos y el printeo de errores donde se pueden ver rutas.

Tenemos ejecucion remota de comandos a traves del input ![[Pasted image 20250502171634.png]]


![[Pasted image 20250502172346.png]]
tenemos la shelll ahora solo hay que ejecutarla 
![[Pasted image 20250502172711.png]]
encontramos una base de datos con credenciales 
![[Pasted image 20250502172842.png]]
admin1 e00cf25ad42683b3df678c61f42c6bda  
ramon  66575d3c2b8a34f83817458f96152b1  
e0Al   5101ad4543a96a7fd84908fd0d802e7d  
kavi   f38cde1654b39fea2bd4f72f1ae4cdda  
tobias 55c82b1ccd55ab219b3b109b07d5061d  
amanda df8b20aa0c935023f99ea58358fb63c4  
admin  d725aeba143f575736b07e045d8ceebb

![[Pasted image 20250502173006.png]]
vemos que el usuario tobias existe

tobias:55c82b1ccd55ab219b3b109b07d5061d  

tobias:slowmotionapocalypse

tenemos credenciales para acceder por ssh
![[Pasted image 20250502173114.png]]
## Escalacion de privilegios 

![[Pasted image 20250502173241.png]]
![[Pasted image 20250502173842.png]]

tunel ssh para poder ver desde mi maquina redirigiendo el puerto para poder vulnerar el servicio  

![[Pasted image 20250502173919.png]]
http://localhost:9090/login

![[Pasted image 20250502173940.png]]
vemos la version del servicio 3.2 ispconfig

las credenciales del panel son admin:slowmotionapocalypse

![[Pasted image 20250502174341.png]]

![[Pasted image 20250502174333.png]]


ssh tobias@nocturnal.htb -L 9090:127.0.0.1:8080 
![[Pasted image 20250502180327.png]]

 me he agobiado un poco con esta maquina lo aparco demomento