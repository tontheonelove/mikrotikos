## VRRP (Virtual Router Redundancy Protocol) on MikroTik routers for high availability

### Assumptions:
- Router 1 (Primary Router): 192.168.1.1
- Router 2 (Backup Router): 192.168.1.2
- Virtual IP Address (VRRP IP): 192.168.1.100
- Interface: ether1

### 1 Configure the Primary Router (Router 1)
- On Router 1:
- Go to IP > VRRP.
- Click on + to add a new VRRP instance.
- Set the following parameters:
- Interface: Select the interface to which the VRRP will be applied (e.g., ether1).
- VRRP ID: Set the ID for the VRRP group (usually 1 or 2).
- Priority: Set a higher priority (e.g., 100) to make this the master router.
- Virtual Address: Set the virtual IP address (e.g., 192.168.1.100).
- Preemption: Enable this option if you want the master router to take over if it comes back online.
- Click OK.

#### Example configuration for Router 1:

```
/ip address
add address=192.168.1.1/24 interface=ether1 network=192.168.1.0

/ip vrpp
add interface=ether1 vrid=1 priority=100 virtual-address=192.168.1.100
```

### 2 Configure the Backup Router (Router 2)
- On Router 2:
- Go to IP > VRRP.
- Click on + to add a new VRRP instance.
- Set the following parameters:
- Interface: Select the same interface (e.g., ether1).
- VRRP ID: Use the same VRRP ID as on Router 1 (e.g., 1).
- Priority: Set a lower priority (e.g., 50) to make this the backup router.
- Virtual Address: Set the same virtual IP address (e.g., 192.168.1.100).
- Preemption: Enable this option if you want the primary router to reclaim control when it returns online.
- Click OK.
- 
#### Example configuration for Router 2:

``````
/ip address
add address=192.168.1.2/24 interface=ether1 network=192.168.1.0

/ip vrpp
add interface=ether1 vrid=1 priority=50 virtual-address=192.168.1.100

``````


### 3 Testing and Verification
- Once both routers are configured:

- Router 1 will be the master, as it has a higher priority.
- Router 2 will be the backup, taking over the virtual IP address 192.168.1.100 if Router 1 goes down.
- To check the status of VRRP on MikroTik, you can run the following command:

``````
/interface vrrp print
``````
- This will show you the VRRP status and which router is currently the master.
