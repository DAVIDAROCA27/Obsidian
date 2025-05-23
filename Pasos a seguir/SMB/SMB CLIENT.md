smbclient -N -L \\\\{TARGET_IP}\\
-N : No password
-L : This option allows you to look at what services are available on a server

enum4linux -a <IP_DEL_OBJETIVO>

crackmapexec smb <IP_DEL_OBJETIVO> --shares

ejemplo para listar shares con session 
smbclient -L \\10.129.202.136 -U john



