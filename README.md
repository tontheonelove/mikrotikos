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






