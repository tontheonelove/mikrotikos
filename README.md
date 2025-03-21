# MikrotikOS Configuration

<img src=https://logonoid.com/images/mikrotik-logo.png/>

## List (update 21- 3 - 25)

- [Load Balance for os6..](Mikrotik%20os6%20Loadbalance.txt)
- [Load Balance for os7..](Mikrotik%20os7%20Loadbalance.txt)
- [Block Syn Flood](Mirkotik%20Block%20Syn%20Flood.md)
- [ROS7  Block range ASN  ](ROS7%20Block%20range%20ASN%20example%20%20ROS7%20Block%20range%20ASN.md)
- [Let’s Encrypt Certificate on MikroTik RouterOS 7](Let’s%20Encrypt%20Certificate%20on%20MikroTik%20RouterOS%207.txt)
- [Bgp Filtering](Filter%20BGP.md)



## Block Domain with Dns server  Add list (available on fw 7.15+)
```
add .txt file with pattern   and upload to file im your routeros
0.0.0.0  yourdomain1
0.0.0.0  yourdomain2
0.0.0.0  yourdomain3
```
<img src= sc.png/>


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

## send logging to syslog server os7 & os6
-------------------------------------
<img src=log.png/>
-------------------------------------
<img src=log2.png/>
-------------------------------------

## Netflow to Netflow server os7 & os6

<img src=netflow.png/>
-------------------------------------

## Netwatch for check network status os7 & os6

![image](https://github.com/user-attachments/assets/260ad835-fca1-48a4-8433-69faba94d5e6)






