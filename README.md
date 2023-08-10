# mikrotikos7-bgp

# Filter BGP

| Command | 	Purpose |
| --- | --- |
| if (dst==193.0.0.0/24) {accept} | advertise this network  |
| if (dst-len == 24) { reject; } | Reject Subnet |
| interface fastethernet/number | Enters interface configuration mode for the specified fast ethernet interface |
| reload | An exec mode command that reboots a Cisco switch or router |
