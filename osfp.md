## OSPF (Open Shortest Path First) connection between MikroTik 2

### Example

- Router 1 (R1):
  - อินเทอร์เฟซที่เชื่อมต่อกับ R2: ether1, IP: 192.168.12.1/30
  - เครือข่าย LAN: 192.168.1.0/24
- Router 2 (R2):
  - อินเทอร์เฟซที่เชื่อมต่อกับ R1: ether1, IP: 192.168.12.2/30
  - เครือข่าย LAN: 192.168.2.0/24
- OSPF Area: backbone (Area ID: 0.0.0.0)

### Setting steps
## 1. Set the IP Address on the interface
#### Router 1 (R1):
 
````
/ip address
add address=192.168.12.1/30 interface=ether1
add address=192.168.1.1/24 interface=ether2
````

#### Router 2 (R2):
````
/ip address
add address=192.168.12.2/30 interface=ether1
add address=192.168.2.1/24 interface=ether2
````

## 2. ตั้งค่า OSPF Instance
- กำหนด Router ID เพื่อให้ OSPF ระบุตัวตนของเราเตอร์ได้ (แนะนำให้ใช้ IP ที่ไม่เปลี่ยนแปลง เช่น Loopback หรือ IP หลัก)

#### Router 1 (R1):
````
/routing ospf instance
set [find default=yes] router-id=192.168.1.1
````

#### Router 2 (R2):
````
/routing ospf instance
set [find default=yes] router-id=192.168.2.1
````

## 3. Add OSPF Area
- In this case, we use the default Backbone Area (Area ID: 0.0.0.0)

#### Router 1 (R1) and Router 2 (R2):
````
/routing ospf area
add name=backbone area-id=0.0.0.0
````

## 4. Add the networks you want OSPF to work on
- Add the networks connecting R1 and R2, including the LANs you want to share the route with

#### Router 1 (R1):
````
/routing ospf network
add network=192.168.12.0/30 area=backbone
add network=192.168.1.0/24 area=backbone
````

#### Router 2 (R2):

````
/routing ospf network
add network=192.168.12.0/30 area=backbone
add network=192.168.2.0/24 area=backbone
````

## 5. Check OSPF connectivity
- Once the setup is complete, you can check the OSPF Neighbor status as follows:

#### Router 1 (R1) and Router 2 (R2):
````
/routing ospf neighbor print
````
- If the status is "Full", then OSPF is working properly and both routers are successfully connected.


## 6. Check the Routing Table
- Check if the route from the other router is added to the routing table:

#### Router 1 (R1):
````
/ip route print
````
- You should see the route 192.168.2.0/24 obtained from OSPF (with the letter "o" in front).

#### Router 2 (R2):
````
/ip route print
````
- You should see the route 192.168.1.0/24 obtained from OSPF.

#### Additional Notes
- Network Type: If the connection is Point-to-Point (e.g. directly via LAN cable), you can set network-type=point-to-point on the OSPF interface to reduce the DR/BDR selection steps:
````
/routing ospf interface
add interface=ether1 network-type=point-to-point
````

- Redistribution: หากต้องการให้เราเตอร์กระจายเส้นทางอื่นๆ (เช่น Static หรือ Connected) เข้าไปใน OSPF ให้ตั้งค่าใน OSPF Instance:
````
/routing ospf instance
set [find default=yes] redistribute-connected=as-type-1
````


#### Example Result
- If everything works correctly:

  - R1 will see R2 as a neighbor and have a route to 192.168.2.0/24
  - R2 will see R1 as a neighbor and have a route to 192.168.1.0/24
