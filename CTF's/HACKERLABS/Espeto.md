  ![[Pasted image 20250513101857.png]]

![[Pasted image 20250513102126.png]]
Maquina windows con active directory vemos el puerto 80 con una web que tiene una version vulnerable

![[Pasted image 20250513101939.png]]


![[Pasted image 20250513102054.png]]
tenemos un modulo de metasploit con el cual conseguir una sesion de meterpreter.

![[Pasted image 20250513102251.png]]
![[Pasted image 20250513102350.png]]
no podemos hacer un getsystem porque no tenemos permisos suficientes asique tocara buscar otra forma de intentar escalar privilegios 
![[Pasted image 20250513102520.png]]

vamos a usar:
post/multi/recon/local_exploit_suggester
para ver a lo que puede ser vulnerable el sistema.
 
![[Pasted image 20250513171533.png]]v

 1   exploit/windows/local/bypassuac_comhijack                      Yes                      The target appears to be vulnerable.                                                                             
 2   exploit/windows/local/bypassuac_eventvwr                       Yes                      The target appears to be vulnerable.                                                                             
 3   exploit/windows/local/bypassuac_sluihijack                     Yes                      The target appears to be vulnerable.                                                                             
 4   exploit/windows/local/cve_2020_0787_bits_arbitrary_file_move   Yes                      The service is running, but could not be validated. Vulnerable Windows 8.1/Windows Server 2012 R2 build detected!
 5   exploit/windows/local/ms16_032_secondary_logon_handle_privesc  Yes                      The service is running, but could not be validated.                                                              
 6   exploit/windows/local/ms16_075_reflection                      Yes                      The target appears to be vulnerable.                                                                             
 7   exploit/windows/local/ms16_075_reflection_juicy                Yes                      The target appears to be vulnerable.                                                                             
 8   exploit/windows/local/tokenmagic                               Yes                      The target appears to be vulnerable.   
 
 a ir probando a ver a cual puede ser vulnerable 
 
 ![[Pasted image 20250513171733.png]]
 tenemos root
 
 
 hacker:60dbda530c41c753a4472c0f28450f38
 root:5dac6bd83c0b871998e9a34c0931f454
 
 no encuentro la flag del user lo dejo para manana pero he sido autority