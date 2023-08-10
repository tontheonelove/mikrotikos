# mikrotikos7-bgp

# Filter BGP

| Command | 	Purpose |
| --- | --- |
| if (dst==193.0.0.0/24) {accept} | advertise this network  |
| if (dst-len == 24) { reject; } | reject Subnet /24 |
| if (bgp-as-path 23932) {set distance 100} | set distance bgp |
| if (bgp-as-path 23932) {set bgp-local-pref 450} | set local preference for multiple bgp |
| if (protocol bgp) {set distance 25} | set distance all bgp |
| if (bgp-as-path 23932) {reject;} | reject as-path-route |
