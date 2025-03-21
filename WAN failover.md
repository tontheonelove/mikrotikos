## MikroTik router for WAN failover

### Step-by-Step Guide to WAN Failover

#### 1. Set Up Two WAN Interfaces
- Connect both WAN connections to two separate interfaces (e.g., ether1 for WAN1 and ether2 for WAN2).
#### 2. Set Up Routing for Both WAN Connections
- You will need to create default routes for both WAN connections.
- Go to IP > Routes and add two default routes:
    - Primary Route (WAN1): Set the distance (e.g., 1) for the primary route.
    - Secondary Route (WAN2): Set the distance (e.g., 2) for the secondary route.

```````
/ip route add gateway=192.168.1.1 distance=1
/ip route add gateway=192.168.2.1 distance=2
```````

- WAN1 (Distance 1) will be used as the primary route, while WAN2 (Distance 2) will be used as a backup if WAN1 fails.

#### 3. Set Up Netwatch to Monitor Primary WAN
- You need to create a script that checks if the primary WAN is up. Netwatch can ping an external IP (e.g., Google's DNS 8.8.8.8) to check the status.
- Go to Tools > Netwatch and create a new entry:

    - Host: 8.8.8.8 (or any reliable external IP)
    - Interval: 00:00:30 (adjust the interval as needed)
    - Timeout: 00:00:10
    - Up Script: (This script will enable the primary route)
 
      ``````
      /ip route enable [find dst-address=0.0.0.0/0 gateway=192.168.1.1]
      /ip route disable [find dst-address=0.0.0.0/0 gateway=192.168.2.1]
      ``````

    - Down Script: (This script will disable the primary route and enable the secondary route)

      ``````
      /ip route disable [find dst-address=0.0.0.0/0 gateway=192.168.1.1]
      /ip route enable [find dst-address=0.0.0.0/0 gateway=192.168.2.1]
      ``````

### 4. Optional: Adjust Health Check for Secondary WAN

#### Test the Failover:
- Disconnect the primary WAN cable (WAN1).
- Check if the internet connection switches to the secondary WAN (WAN2).
- Reconnect the primary WAN and check if it switches back.
