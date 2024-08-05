# mikrotikos7-bgp

# Filter BGP

| Command | 	Purpose |
| --- | --- |
| if (dst==193.0.0.0/24) {accept} | advertise this network  |
| if (dst-len == 24) { reject; } | reject Subnet /24 |
| if (bgp-as-path 99999) {set distance 100} | set distance bgp |
| if (bgp-as-path 99999) {set bgp-local-pref 450} | set local preference for multiple bgp |
| if (protocol bgp) {set distance 100; accept} | set distance  bgp route |
| if (bgp-as-path 99999) {reject;} | reject as-path-route |
| routing/bgp/advertisements/print where nexthop=XXX.XXX.XXX.XXX | check-route |



# Block Domain with Dns server

add .txt file with pattern   and upload to file im your routeros

0.0.0.0  yourdomain1

0.0.0.0  yourdomain2

0.0.0.0  yourdomain3

<img src= sc.png/>


# Static DNS for block domain

/ip firewall nat
add action=redirect chain=dstnat dst-port=53 protocol=tcp to-ports=53
add action=redirect chain=dstnat dst-port=53 protocol=udp to-ports=53

/ip dns static
add address=2.2.2.2 name=yourdomain1.com

add address=2.2.2.2 name=yourdomain2.com

add address=2.2.2.2 name=yourdomain3.com





