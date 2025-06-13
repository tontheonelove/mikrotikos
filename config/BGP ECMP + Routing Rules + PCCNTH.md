# BGP ECMP + Routing Rules + PCCNTH (BGP Load Balancer) 2 ISP  80/20 %

## Firewall Mangle splits traffic 80/20 using NTH or PCC.


`````
/ip/firewall/mangle
# Mark Connection: 80% to ISP1
add chain=prerouting connection-mark=no-mark nth=5,1 action=mark-connection new-connection-mark=to-ISP1 passthrough=yes

# Mark Connection: 20% to ISP2
add chain=prerouting connection-mark=no-mark nth=5,2 action=mark-connection new-connection-mark=to-ISP2 passthrough=yes

# Mark Routing
add chain=prerouting connection-mark=to-ISP1 action=mark-routing new-routing-mark=to-ISP1 passthrough=yes
add chain=prerouting connection-mark=to-ISP2 action=mark-routing new-routing-mark=to-ISP2 passthrough=yes
``````


## Add Routing to Mark

``````
/ip/route
add dst-address=0.0.0.0/0 gateway=ISP1_GATEWAY routing-mark=to-ISP1
add dst-address=0.0.0.0/0 gateway=ISP2_GATEWAY routing-mark=to-ISP2
``````

## Result:

First 5 connections: 4 to ISP1, 1 to ISP2 → 80/20

Do at session level (connection-level load balancing)


## 📌 Note:

This method is Per-Connection Load Balance, not per-packet.

Be careful with some protocols that use many same sessions (e.g. video call, VPN, banking) → may drop if switch path.

Can be used with Netwatch or BFD to check ISP failover.
