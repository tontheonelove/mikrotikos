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




# mikrotikos7

# firewall mangle

/ip firewall mangle

add action=mark-connection chain=prerouting connection-mark=no-mark connection-state=new \

    in-interface=ether1 new-connection-mark=ISP1_conn passthrough=yes
    
add action=mark-connection chain=prerouting connection-mark=no-mark connection-state=new \

    in-interface=pppoe-out1 new-connection-mark=ISP2_conn passthrough=yes
    
add action=mark-routing chain=output connection-mark=ISP1_conn new-routing-mark=ISP1 \

    passthrough=yes
    
add action=mark-routing chain=output connection-mark=ISP2_conn new-routing-mark=ISP2 \

    passthrough=yes
    
add action=mark-connection chain=prerouting connection-mark=no-mark connection-state=new \

    dst-address-type=!local in-interface=ether5 new-connection-mark=ISP1_conn \
    
    passthrough=yes per-connection-classifier=both-addresses-and-ports:2/0
    
add action=mark-connection chain=prerouting connection-mark=no-mark connection-state=new \

    dst-address-type=!local in-interface=ether5 new-connection-mark=ISP2_conn \
    
    passthrough=yes per-connection-classifier=both-addresses-and-ports:2/1
    
add action=mark-routing chain=prerouting connection-mark=ISP1_conn in-interface=ether5 \

    new-routing-mark=ISP1 passthrough=yes
    
add action=mark-routing chain=prerouting connection-mark=ISP2_conn in-interface=ether5 \

    new-routing-mark=ISP2 passthrough=yes
    


# ip route

/ip route
add check-gateway=ping comment=Default disabled=no distance=1 dst-address=0.0.0.0/0 \
    gateway=xxx.xxx.xxx.xxx pref-src="" routing-table=main scope=30 suppress-hw-offload=no \
    target-scope=10
add check-gateway=ping comment=Default disabled=no distance=2 dst-address=0.0.0.0/0 \
    gateway=pppoe-out1 pref-src="" routing-table=main scope=30 suppress-hw-offload=no \
    target-scope=10
add disabled=no distance=1 dst-address=0.0.0.0/0 gateway=xxx.xxx.xxx.xxx pref-src="" \
    routing-table=ISP1 scope=30 suppress-hw-offload=no target-scope=10
add disabled=no distance=1 dst-address=0.0.0.0/0 gateway=pppoe-out1 pref-src="" \
    routing-table=ISP2 scope=30 suppress-hw-offload=no target-scope=10


# routing tables

/routing table
add disabled=no fib name=ISP1
add disabled=no fib name=ISP2



