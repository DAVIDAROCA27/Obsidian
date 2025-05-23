![[Pasted image 20250508152520.png]]la pagina web vemos que es la web de apache por defecto 
![[Pasted image 20250508152631.png]]
vemos que tiene un wordpress corriendo
el wordpress funciona un poco mal vemos en el codigo fuente que esta intenando todo el rato coger la informacion de  una ip que corre en local sin exito ya que se queda todo el rato cargando ![[Pasted image 20250508152742.png]]
tiene estos plugins los dos vulnerables

ahora ya es ir probando scripts
este por ejemplo no funciona bien apunta a una url que simplemente no es correcta ![[Pasted image 20250508153607.png]]
despues de estar probando bastantes payloads y muchos exploits de github encontramos un exploit con msfconsole que funciona genial 
![[Pasted image 20250508160433.png]]
nos traemos linpeas ya que somos www-data y no tenemos muchos privilegios ![[Pasted image 20250508161249.png]]
tenemos esto ![[Pasted image 20250508161452.png]]

para poder entrar a la base de datos del wordpress

![[Pasted image 20250508171240.png]]
encontramos un archivo que pone muyconfidencial.zip

![[Pasted image 20250508171405.png]]
![[Pasted image 20250508171700.png]]![[Pasted image 20250508171710.png]]
![[Pasted image 20250508171726.png]]

tenemos ahora dabase.kdbx y kepass.dpm