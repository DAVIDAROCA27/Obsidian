Puertos abiertos ![[Pasted image 20250512140523.png]]
La web predeterminada de apache ![[Pasted image 20250512140538.png]]
**![[Pasted image 20250512140508.png]]**
![[Pasted image 20250512140553.png]]
![[Pasted image 20250512140604.png]]
Solamente tenemos esto y un puerto ssh  no sabemos usuarios demomento tmp solamente que la contrasena puede ser seguramente vulnerable 
![[Pasted image 20250512140735.png]]
tenemos acceso por ssh ![[Pasted image 20250512140823.png]]


info:bdf8c3a56b3c61670c093a8bff406f6e

![[Pasted image 20250512140855.png]]

![[Pasted image 20250512141012.png]]
![[Pasted image 20250512151801.png]]
hay varias formas ahora de rootear la maquina crackeando el archivo /etc/passwd y /etc/shadow en conjunto o cogiendo el id_rsa del root y crackeandolo con john que es la opcionq que yo he utilizado

```
LFILE=/etc/shadow
sudo base64 "$LFILE" | base64 --decode
```
![[Pasted image 20250512151901.png]]
![[Pasted image 20250512143616.png]]

ya podemos acceder por ssh y tendriamos root y la flag 


root:7af5b2ba4805dbee057ea57dd5b1d089