## Filter BGP

| Command | 	Purpose |
| --- | --- |
| if (dst==193.0.0.0/24) {accept} | advertise this network  |
| if (dst-len == 24) { reject; } | reject Subnet /24 |
| if (bgp-as-path 99999) {set distance 100} | set distance bgp |
| if (bgp-as-path 99999) {set bgp-local-pref 200} | control outbound traffic |
| if (protocol bgp) {set distance 100; accept} | set distance  bgp route |
| if (bgp-as-path 99999) {reject;} | reject as-path-route |
| if (dst==0.0.0.0/23) {set bgp-path-prepend 3; accept} | control Inbound Traffic
| routing/bgp/advertisements/print where nexthop=XXX.XXX.XXX.XXX | check-route |
