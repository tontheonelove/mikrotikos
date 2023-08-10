# mikrotikos7-bgp

# Filter BGP

| Command | 	Purpose |
| --- | --- |
| if (dst==193.0.0.0/24) {accept} | advertise this network  |
| if (dst-len == 24) { reject; } | Reject Subnet /24 |
| if (bgp-as-path 23932) {set distance 100} | Set distance bgp |
| if (bgp-as-path 23932) {set bgp-local-pref 450} | set local preference for multiple bgp |
