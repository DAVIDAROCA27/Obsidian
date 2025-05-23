

![[Pasted image 20250517091625.png]]

![[Pasted image 20250517091639.png]]
Tenemos acceso anonymous al ftp
podemos subir archivos


 ![[Pasted image 20250517091923.png]]
IIS7 pagina default parece que tenmos de primeras ![[Pasted image 20250517091543.png]]

Como vemos los contenidos de la web entendemos si subimos un archivo con extension aspx con una shell y accedemos a la ruta ejecutara el codigo ![[Pasted image 20250517092118.png]]
creamos el payload a subir ![[Pasted image 20250517092521.png]]

asique usando metasploit nos ponemos a la escucha 
subimos el codigo y lo ejecutamos accediendo a la ruta
![[Pasted image 20250517092629.png]]
![[Pasted image 20250517092635.png]]

tenemos ya session con meterpreter
![[Pasted image 20250517092651.png]]
![[Pasted image 20250517092927.png]]
Buscamos el post para escalar privs
![[Pasted image 20250517093159.png]]
tenemos session activa con meterpreter
![[Pasted image 20250517093259.png]]

Y tenemos root ![[Pasted image 20250517093311.png]]





babis:414b7feb2a122bfb49ebf5d4fdc04b42
root:a7cb5315163dd024405c53916b84b786
