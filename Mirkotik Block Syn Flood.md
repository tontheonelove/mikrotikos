## Mirkotik Block Syn Flood

`````````

SYN Flood
/ip/settings/set tcp-syncookies=yes
SYN-ACK Flood
/ip/firewall/filter add action=return chain=detect-ddos dst-limit=32,32,src-and-dst-addresses/10s protocol=tcp tcp-flags=syn,ack

`````````
