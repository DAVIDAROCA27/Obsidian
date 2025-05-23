nmap -p- -vvv -sS -sC -sV -sU --min-rate=5000 -T4 -A --script=vuln -O -Pn 192.168.1.0/24

nmap -sC -sV -sC -vvv -p- -A -T4 -Pn -n --disable-arp-pin 10.129.20.187 -oN scan.txt

escaneo UDP puede tardar bastante -->  nmap -sU -T4 --max-retries 2 --min-rate 500 -p- <IP> 

nmap --script smb-os-discovery 10.129.228. --> Nombre de la maquina

Alternativa más silenciosa -->
nmap -sS -sU -p- -T2 -O --script=vuln --data-length=50 --scan-delay=5s 192.168.1.0/24

nmap -p- -vvv -sS -sC -sV -sU --min-rate=5000 -T4 -Pn 192.168.1.0/24


- **Escaneo de Rango de Red:**
    
    - `nmap 10.129.2.0/24 -sn -oA tnet` → Detecta hosts activos en la red sin escanear puertos.
    - **Limitación:** Si hay firewalls, algunos hosts pueden no responder.
- **Escaneo desde Lista de IPs:**
    
    - `nmap -sn -oA tnet -iL hosts.lst` → Usa una lista predefinida de IPs.
- **Escaneo de IPs Específicas:**
    
    - `nmap -sn -oA tnet 10.129.2.18-20` → Especificar IPs individuales o en rango.
- **Escaneo de un Solo Host:**
    
    - `nmap 10.129.2.18 -sn -oA host` → Verifica si el host está activo antes de escanear puertos.
    - `-PE` → Fuerza el uso de ICMP Echo Request (ping).
- **Opciones Avanzadas:**
    
    - `--packet-trace` → Muestra los paquetes enviados y recibidos.
    - `--reason` → Explica por qué Nmap considera un host activo.
    - `--disable-arp-ping` → Evita usar ARP para forzar escaneo ICMP.


## Scanning Options

| **Nmap Option**      | **Description**                                                        |
| -------------------- | ---------------------------------------------------------------------- |
| `10.10.10.0/24`      | Target network range.                                                  |
| `-sn`                | Disables port scanning.                                                |
| `-Pn`                | Disables ICMP Echo Requests                                            |
| `-n`                 | Disables DNS Resolution.                                               |
| `-PE`                | Performs the ping scan by using ICMP Echo Requests against the target. |
| `--packet-trace`     | Shows all packets sent and received.                                   |
| `--reason`           | Displays the reason for a specific result.                             |
| `--disable-arp-ping` | Disables ARP Ping Requests.                                            |
| `--top-ports=<num>`  | Scans the specified top ports that have been defined as most frequent. |
| `-p-`                | Scan all ports.                                                        |
| `-p22-110`           | Scan all ports between 22 and 110.                                     |
| `-p22,25`            | Scans only the specified ports 22 and 25.                              |
| `-F`                 | Scans top 100 ports.                                                   |
| `-sS`                | Performs an TCP SYN-Scan.                                              |
| `-sA`                | Performs an TCP ACK-Scan.                                              |
| `-sU`                | Performs an UDP Scan.                                                  |
| `-sV`                | Scans the discovered services for their versions.                      |
| `-sC`                | Perform a Script Scan with scripts that are categorized as "default".  |
| `--script <script>`  | Performs a Script Scan by using the specified scripts.                 |
| `-O`                 | Performs an OS Detection Scan to determine the OS of the target.       |
| `-A`                 | Performs OS Detection, Service Detection, and traceroute scans.        |
| `-D RND:5`           | Sets the number of random Decoys that will be used to scan the target. |
| `-e`                 | Specifies the network interface that is used for the scan.             |
| `-S 10.10.10.200`    | Specifies the source IP address for the scan.                          |
| `-g`                 | Specifies the source port for the scan.                                |
| `--dns-server <ns>`  | DNS resolution is performed by using a specified name server.          |
| -f                   | Send tpc fragmented packets                                            |
# 📌 **Nmap Cheatsheet**

## 🎯 **Escaneo de Puertos**

|Opción|Descripción|Ejemplo|
|---|---|---|
|`-p`|Escanear puertos específicos|`nmap -p 22,80,443 192.168.1.100`|
|`-p-`|Escanear todos los puertos (1-65535)|`nmap -p- 192.168.1.100`|
|`-F`|Escaneo rápido (puertos comunes)|`nmap -F 192.168.1.100`|
|`-r`|Escaneo en orden secuencial|`nmap -p 1-1024 -r 192.168.1.100`|

## 🏷️ **Detección de Servicios y Versiones**

|                       |                                 |                                                |
| --------------------- | ------------------------------- | ---------------------------------------------- |
| Opción                | Descripción                     | Ejemplo                                        |
| `-sV`                 | Detectar versiones de servicios | `nmap -sV 192.168.1.100`                       |
| `--version-intensity` | Nivel de profundidad (0-9)      | `nmap -sV --version-intensity 5 192.168.1.100` |
| `--allports`          | No ignorar puertos filtrados    | `nmap -sV --allports 192.168.1.100`            |

## 🖥️ **Detección de Sistemas Operativos**

|                  |                                    |                                        |
| ---------------- | ---------------------------------- | -------------------------------------- |
| Opción           | Descripción                        | Ejemplo                                |
| `-O`             | Detección de sistema operativo     | `nmap -O 192.168.1.100`                |
| `--osscan-guess` | Adivinar OS si no hay datos claros | `nmap -O --osscan-guess 192.168.1.100` |

## 🕵️ **Escaneos Silenciosos (Evasión de IDS/IPS)**

|                    |                                   |                                           |
| ------------------ | --------------------------------- | ----------------------------------------- |
| Opción             | Descripción                       | Ejemplo                                   |
| `-sS`              | Escaneo TCP SYN (stealth)         | `nmap -sS 192.168.1.100`                  |
| `-sN`              | Escaneo TCP NULL                  | `nmap -sN 192.168.1.100`                  |
| `-sF`              | Escaneo TCP FIN                   | `nmap -sF 192.168.1.100`                  |
| `-sX`              | Escaneo TCP Xmas                  | `nmap -sX 192.168.1.100`                  |
| `--data-length 50` | Agregar datos al paquete          | `nmap --data-length 50 -sS 192.168.1.100` |
| `--scan-delay 5s`  | Introducir retraso entre paquetes | `nmap --scan-delay 5s -sS 192.168.1.100`  |

## 📡 **Escaneos UDP**

|   |   |   |
|---|---|---|
|Opción|Descripción|Ejemplo|
|`-sU`|Escaneo de puertos UDP|`nmap -sU 192.168.1.100`|
|`-sSU`|Escaneo combinado TCP SYN + UDP|`nmap -sSU 192.168.1.100`|
|`-p`|Especificar puertos UDP|`nmap -sU -p 161 192.168.1.100`|

## 🔥 **Escaneo de Vulnerabilidades (NSE Scripts)**

|                   |                             |                                      |
| ----------------- | --------------------------- | ------------------------------------ |
| Opción            | Descripción                 | Ejemplo                              |
| `--script=vuln`   | Escaneo de vulnerabilidades | `nmap --script=vuln 192.168.1.100`   |
| `--script=smb*`   | Scripts SMB                 | `nmap --script=smb* 192.168.1.100`   |
| `--script=http-*` | Scripts HTTP                | `nmap --script=http-* 192.168.1.100` |
## Scrip Engine

| **Categoría**      | **Descripción**                                                                                                                         |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------- |
| auth               | Determination of authentication credentials.                                                                                            |
| broadcast          | Scripts, which are used for host discovery by broadcasting and the discovered hosts, can be automatically added to the remaining scans. |
| brute              | Executes scripts that try to log in to the respective service by brute-forcing with credentials.                                        |
| default            | Default scripts executed by using the -sC option.                                                                                       |
| discovery          | Evaluation of accessible services.                                                                                                      |
| dos                | These scripts are used to check services for denial of service vulnerabilities and are used less as it harms the services.              |
| exploit            | This category of scripts tries to exploit known vulnerabilities for the scanned port.                                                   |
| external           | Scripts that use external services for further processing.                                                                              |
| fuzzer             | This uses scripts to identify vulnerabilities and unexpected packet handling by sending different fields, which can take much time.     |
| intrusive          | Intrusive scripts that could negatively affect the target system.                                                                       |
| malware            | Checks if some malware infects the target system.                                                                                       |
| safe               | Defensive scripts that do not perform intrusive and destructive access.                                                                 |
| version            | Extension for service detection.                                                                                                        |
| vuln               | Identification of specific vulnerabilities.                                                                                             |
| --script=http-enum | Usa scripts NSE para enumerar archivos y directorios comunes en servidores web.                                                         |
#### Specific Scripts Category

```shell-session
[!bash!]$ sudo nmap <target> --script <category>
```

#### Defined Scripts

```shell-session
[!bash!]$ sudo nmap <target> --script <script-name>,<script-name>,...
```
## 📂 **Opciones de Salida**

|   |   |
|---|---|
|Opción|Descripción|
|`-oA filename`|Almacena en todos los formatos|
|`-oN filename`|Formato normal|
|`-oG filename`|Formato grepable|
|`-oX filename`|Formato XML|

## 🚀 **Optimización y Rendimiento**

|   |   |
|---|---|
|Opción|Descripción|
|`--max-retries <num>`|Número de reintentos|
|`--stats-every=5s`|Mostrar estado cada 5s|
|`-v/-vv`|Salida detallada|
|`--initial-rtt-timeout 50ms`|Configurar RTT inicial|
|`--max-rtt-timeout 100ms`|Configurar RTT máximo|
|`--min-rate 300`|Paquetes simultáneos|
|`-T <0-5>`|Nivel de velocidad (4 es rápido y estable)|
#### Default Scan

  Performance

```shell-session
pepedavidphone@htb[/htb]$ sudo nmap 10.129.2.0/24 -F

<SNIP>
Nmap done: 256 IP addresses (10 hosts up) scanned in 39.44 seconds
```

#### Optimized RTT

  Performance

```shell-session
pepedavidphone@htb[/htb]$ sudo nmap 10.129.2.0/24 -F --initial-rtt-timeout 50ms --max-rtt-timeout 100ms

<SNIP>
Nmap done: 256 IP addresses (8 hosts up) scanned in 12.29 seconds
```
---
#### Default Scan

  Performance

```shell-session
pepedavidphone@htb[/htb]$ sudo nmap 10.129.2.0/24 -F -oN tnet.default

<SNIP>
Nmap done: 256 IP addresses (10 hosts up) scanned in 29.83 seconds
```

#### Optimized Scan

  Performance

```shell-session
pepedavidphone@htb[/htb]$ sudo nmap 10.129.2.0/24 -F -oN tnet.minrate300 --min-rate 300

<SNIP>
Nmap done: 256 IP addresses (10 hosts up) scanned in 8.67 seconds
```

## Decoys FIREWALL

### IPv6 Attacks

Hacer ataques usando IPV6 puede funcionar ya que el el firewall aveces puede no filtrar por este

#### Scan by Using Decoys

```shell-session
[!bash!]$ sudo nmap 10.129.2.28 -p 80 -sS -Pn -n --disable-arp-ping --packet-trace -D RND:5

|`-D RND:5`|Generates five random IP addresses that indicates the source IP the connection comes from.|
```
#### SYN-Scan From DNS Port


```shell-session
[!bash!]$ sudo nmap 10.129.2.28 -p50000 -sS -Pn -n --disable-arp-ping --packet-trace --source-port 53
```
cosas a tener en cuenta aqui el objetivo tb puede ser vulnerable si enviamos paquetes desde los puertos :
==88==-kerberos
==20==-ftp
==67==-DHCP
EJEMPLO: 
 `**nmap -sS -v -v -Pn 172.25.0.14**`

`**Starting Nmap ( https://nmap.org )**`
`**Nmap scan report for 172.25.0.14**`
`**Not shown: 1658 filtered ports**`
`**PORT   STATE  SERVICE**`
`**88/tcp closed kerberos-sec**`   

`**Nmap done: 1 IP address (1 host up) scanned in 7.02 seconds**`

`nmap -sS -v -v -Pn -g 88 172.25.0.14**`

`**Starting Nmap ( https://nmap.org )**`
`**Nmap scan report for 172.25.0.14**`
`**Not shown: 1653 filtered ports**`
`**PORT     STATE SERVICE**`
`**135/tcp  open  msrpc**`
`**139/tcp  open  netbios-ssn**`
`**445/tcp  open  microsoft-ds**`
`**1025/tcp open  NFS-or-IIS**`
`**1027/tcp open  IIS**`
`**1433/tcp open  ms-sql-s**`

`**Nmap done: 1 IP address (1 host up) scanned in 0.37 seconds**`
`**nmap --dns-server <ns1>,<ns2> <objetivo>**`

#### Connect To The Filtered Port

```shell-session
[!bash!]$ ncat -nv --source-port 53 10.129.2.28 50000
```
📌 **Notas:**

- `-n` → No resolución DNS.
    
- `-R` → Forzar resolución inversa.
    
- `--disable-arp-ping` → No usar ARP.
    
- `-PR` → ARP scan en LAN.
    
- `-PS` → TCP SYN ping.
    
- `-PA` → TCP ACK ping.
    
- `-PU` → UDP ping.
    
- `-PB` → TCP SYN + ICMP.
    
- `-T1` a `-T5` → Ajuste de velocidad.
  
  
Guardar archivo en xml para poder verlo html:

--> xsltproc target.xml -o target.html
Levantar server con python y la ruta --> http://0.0.0.0:8000/target.html
![[Pasted image 20250322191755.png]]


## SABER SERVIDOR DNS 
dig @10.129.2.48 -p 53 version.bind CHAOS TXT



https://nmap.org/book/firewall-subversion.html