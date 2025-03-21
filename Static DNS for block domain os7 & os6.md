## Static DNS for block domain os7 & os6
```
/ip firewall nat
add action=redirect chain=dstnat dst-port=53 protocol=tcp to-ports=53
add action=redirect chain=dstnat dst-port=53 protocol=udp to-ports=53
/ip dns static
add address=2.2.2.2 name=yourdomain1.com
add address=2.2.2.2 name=yourdomain2.com
add address=2.2.2.2 name=yourdomain3.com
```
