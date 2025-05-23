![[Pasted image 20250513081022.png]]
![[Pasted image 20250513081111.png]]
Puertos 22 y 80 abiertos 
![[Pasted image 20250513081305.png]]
Pagina de apache por defecto
![[Pasted image 20250513081343.png]]
Fuzzeamos vemos que corre un wordpress por detras
![[Pasted image 20250513081409.png]]
![[Pasted image 20250513081427.png]]
Vemos ya que la version del wordpress que corre es la 6.5.3 que es vulnerable. Aunque yo por esta via no consegui hackearla  va por otro lado la maquina aunque entiendo que se podria hacer de esa forma tambien 
![[Pasted image 20250513081632.png]]
No hay plugins vamos a ver si conseguimos algun usuario enumerando 

![[Pasted image 20250513082029.png]]
Vemos un usuario llamado dylan:password1
![[Pasted image 20250513082151.png]]

![[Pasted image 20250513082209.png]]
Este plugin nos deja subir contenido a la pagina Aunque  diria que los otros dos tambien son vulnerables.
![[Pasted image 20250513082257.png]]
subimos nuestra shell y ahora  tenemos que ir a la ruta para poder ejecutarla y ponernos a la escucha![[Pasted image 20250513082246.png]]

![[Pasted image 20250513082419.png]]





debian:f3e431cd1129e9879e482fcb2cc151e8
root:39531504f0ddd4778a542fae02fe4733
