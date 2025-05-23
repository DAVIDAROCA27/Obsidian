#### **Ping**

📌 Envía paquetes ICMP para verificar si un host está activo.

bash

CopiarEditar

`ping -c 4 192.168.1.1`

#### 2️⃣ **Arp-scan**

📌 Escanea la red local usando ARP.

bash

CopiarEditar

`arp-scan -l`

#### 3️⃣ **Nmap (ICMP y ARP Discovery)**

📌 Descubre hosts en una red con diferentes técnicas.

bash

CopiarEditar

`nmap -sn 192.168.1.0/24  # Ping scan nmap -PR 192.168.1.0/24  # ARP scan`

#### 4️⃣ **Netdiscover**

📌 Escaneo ARP en redes locales (ideal para WiFi).

bash

CopiarEditar

`netdiscover -r 192.168.1.0/24`

#### 5️⃣ **Fping**

📌 Envía múltiples pings rápidamente.

bash

CopiarEditar

`fping -g 192.168.1.0/24`

#### 6️⃣ **Hping3**

📌 Envía paquetes personalizados (ICMP, TCP, UDP).

bash

CopiarEditar

`hping3 -1 192.168.1.1  # ICMP Ping hping3 -S 192.168.1.1 -p 80  # TCP SYN Ping`

#### 7️⃣ **Masscan**

📌 Escaneo de red ultrarrápido.

bash

CopiarEditar

`masscan 192.168.1.0/24 -p80`

#### 8️⃣ **Traceroute**

📌 Muestra la ruta que sigue un paquete.

bash

CopiarEditar

`traceroute 192.168.1.1`

#### 9️⃣ **MTR**

📌 Similar a traceroute, pero en tiempo real.

bash

CopiarEditar

`mtr 192.168.1.1`

#### 🔟 **Nbtscan**

📌 Escanea hosts en redes Windows (NetBIOS).

bash

CopiarEditar

`nbtscan 192.168.1.0/24`