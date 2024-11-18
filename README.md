# mikrotikos7-bgp

# Let’s Encrypt Certificate on MikroTik RouterOS 7

1. Enabled Cloud DDNS

2. open terminal  and run this command 👉 "certificate/enable-ssl-certificate dns-name=xxx.sn.mynetname.net"

wait till msg.  “progess: [success] ssl certificate updated”

3. Enable Service “www-ssl (https)” and Disable Service “www (http)”

Try Open by browsers  👉 URL “https://xxx.sn.mynetname.net”

SSL Expire 90 Day renew it if available


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



# Block Domain with Dns server  Add list (available on fw 7.15+)

add .txt file with pattern   and upload to file im your routeros

0.0.0.0  yourdomain1

0.0.0.0  yourdomain2

0.0.0.0  yourdomain3

<img src= sc.png/>


# Static DNS for block domain os7 & os6

/ip firewall nat

add action=redirect chain=dstnat dst-port=53 protocol=tcp to-ports=53

add action=redirect chain=dstnat dst-port=53 protocol=udp to-ports=53

/ip dns static

add address=2.2.2.2 name=yourdomain1.com

add address=2.2.2.2 name=yourdomain2.com

add address=2.2.2.2 name=yourdomain3.com


# send logging to syslog server os7 & os6
-------------------------------------
<img src=log.png/>
-------------------------------------
<img src=log2.png/>
-------------------------------------

# Netflow to Netflow server os7 & os6

<img src=netflow.png/>
-------------------------------------

# Netwatch for check network status os7 & os6

![image](https://github.com/user-attachments/assets/260ad835-fca1-48a4-8433-69faba94d5e6)






