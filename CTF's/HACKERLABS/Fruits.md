Puertos 22 y 80 abiertos
![[Pasted image 20250508145711.png]]
nos encontramos con esta web donde tenemos un input donde poder buscar frutas que no hace nada ![[Pasted image 20250508145738.png]]
![[Pasted image 20250508145748.png]]

![[Pasted image 20250508145847.png]]

vemos que la url si que carga y esta es vulnerable a local file inclusion
![[Pasted image 20250508145917.png]]
ya tenemos un usuario ![[Pasted image 20250508145950.png]]

ahora la forma de resolver esta maquina porque ya no hay nada mas es haciendo fuerza bruta por ssh cosa que no funciona muy bien ya que se hacen entre 12-16 peticiones por minuto.Por lo menos cuando yo lo intente ahora parece que va un poco mas rapido ![[Pasted image 20250508150218.png]]
![[Pasted image 20250508150225.png]]
[22][ssh] host: 10.0.2.7   login: bananaman   password: celtic

![[Pasted image 20250508150327.png]]
user.txt:482c811da5d5b4bc6d497ffa98491e38
![[Pasted image 20250508150426.png]]
root.txt21232f297a57a5a743894a0e4a801fc3

Maquina muy facil