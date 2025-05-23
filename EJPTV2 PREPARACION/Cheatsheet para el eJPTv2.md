   https://rinku.tech/experiencia-ejptv2/
https://mirasoyroot.com/guia-definitiva-para-aprobar-el-ejpt-estrategias-herramientas-recursos-y-trucos-para-el-exito-en-pentesting/
pin sweep one liner
## 

Herramientas core para el examen

Hay que saber utilizar muy bien las siguientes herramientas para poder aprobar el examen.

- fping
    
- Nmap
    
- WPScan
    
- CrackMapExec (esta es super importante)
    
- Metasploit
    
- Searchsploit
    
- Hydra
    
- xfreerdp

### Descubrimiento de hosts

```
fping -a -g {Rango de IP} 
```

- -a: Este argumento indica que muestre solamente las ips que están activas en la red.
- -g: Este argumento especifica que vamos a hacer el ping a un rango de ip.

```
nmap -sn {Rango de IP}
```

- -sn Este argumento indica a nmap que realice un escaneo de ping al rango de ip indicado.

```
netdiscover -r {Rango de IP}
```

- -r Este argumento indica que se va escanear un rango de ip.

### IP Routing (Acceder a otra IP a la que no tengamos accesibilidad)

```
Route
#Para ver las redes a las que tenemos acceso

ip route add "IP descubierta" via "gateway"
```

### Nmap

- Detección de sistema operativo

```
nmap -Pn -O "IP"
```

- -Pn: Indica que ignore la detección de host. Es útil cuando el host está filtrando o bloqueando el trafico ICMP.
- -O: Indica que muestre el sistema operativo.

#### Escaneo rápido de Nmap

```
nmap -sC -sV "IP"
```

- -sC: Indica que utilice unos scripts básicos de reconocimiento contra la IP.
- -sV: Indica que muestre la versión que está funcionando en los puertos encontrados.

#### Escaneo completo de Nmap

```
nmap -sCV -p- "IP" -oN escaneo_completo.txt
```

- -sCV: Es un conjunto del comando -sC y -sV.
- -p-: Indica que haga un escaneo a los 65535.
- -oN: Indica que el resultado que muestre Nmap por consola lo almacene en el archivo escaneo_completo.txt

#### Comprobación de vulnerabilidades con Nmap

```
nmap --script=servicio-* -p"puerto del servicio" "IP"
Ej: nmap --script=ftp-* -p21 "IP"
```

pivvoting con metasploit --> https://www.zonasystem.com/2020/01/pivoting-con-metasploit-route-portfwd-y-portproxy.html

http://tutorialspoint.com/metasploit/metasploit_pivoting.htm


# Brute-Forcing any service

[](https://github.com/PakCyberbot/eJPTv2-Notes/blob/main/service-enumeration.md#brute-forcing-any-service)

It is recommended to use the **HYDRA** tool for brute-forcing.

```shell
hydra -l <username> -P <passlist> <IP> <serviceName>
```

WinRM (5985/5986) service brute-forcing doesn’t supported by hydra. Use **CRACKMAPEXEC** instead.

```shell
crackmapexec winrm <IP> -d <Domain Name> -u usernames.txt -p passwords.txt
```

Brute-forcing can be done through Nmap & MsfConsole

Nmap has the naming-convention for brute-forcing scripts: **-brute** for example: smb-brute, ssh-brute e.t.c.

MsfConsole has the naming-convention for brute-forcing modules: **_login** for example: smb_login, ssh_login e.t.c.