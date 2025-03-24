# Mikrotik IPsec site-to-site VPN between two routers

### Scenario
- Router 1 (Office 1):
  - WAN IP: 192.168.70.2 (replace with your public IP)
  - LAN: 10.10.11.0/24
- Router 2 (Office 2):
  - WAN IP: 192.168.80.2 (replace with your public IP)
  - LAN: 10.10.12.0/24
- Goal: Connect the two LANs securely via an IPsec tunnel.

- Prerequisites
  - Both routers have internet access and public IP addresses (or are properly port-forwarded if behind NAT).
  - Local networks (LANs) on both routers are different subnets.
  - Mikrotik RouterOS is updated (this guide works with recent versions as of March 23, 2025).
 

### Configuration Steps
#### Step 1: Basic Router Setup
- Ensure both routers have their WAN and LAN interfaces configured with appropriate IP addresses and NAT masquerade rules for internet access. For example:

#### Router 1:
````````
/ip address
add address=192.168.70.2/30 interface=ether1  # WAN
add address=10.10.11.1/24 interface=ether2   # LAN

/ip route
add gateway=192.168.70.1  # WAN gateway

/ip firewall nat
add chain=srcnat out-interface=ether1 action=masquerade

````````

#### Router 2:
````````
/ip address
add address=192.168.80.2/30 interface=ether1  # WAN
add address=10.10.12.1/24 interface=ether2   # LAN

/ip route
add gateway=192.168.80.1  # WAN gateway

/ip firewall nat
add chain=srcnat out-interface=ether1 action=masquerade
````````

#### Step 2: Configure IPsec Peer
- Define the IPsec peer settings on both routers, specifying the remote router’s WAN IP and a pre-shared key (PSK) for authentication.

#### Router 1:
````````
/ip ipsec peer
add address=192.168.80.2/32 auth-method=pre-shared-key secret="YourStrongSecretKey" exchange-mode=ike2
````````

#### Router 2:
````````
/ip ipsec peer
add address=192.168.70.2/32 auth-method=pre-shared-key secret="YourStrongSecretKey" exchange-mode=ike2
````````

- Use the same secret on both sides (e.g., "YourStrongSecretKey").
- exchange-mode=ike2 is recommended for better security, but main works too if compatibility is an issue.

### Step 3: Configure IPsec Policy
- Set up the IPsec policy to define which traffic to encrypt (the LAN-to-LAN traffic) and enable tunneling.

#### Router 1:
````````
/ip ipsec policy
add src-address=10.10.11.0/24 dst-address=10.10.12.0/24 tunnel=yes action=encrypt proposal=default sa-src-address=192.168.70.2 sa-dst-address=192.168.80.2
````````

#### Router 2:
````````
/ip ipsec policy
add src-address=10.10.12.0/24 dst-address=10.10.11.0/24 tunnel=yes action=encrypt proposal=default sa-src-address=192.168.80.2 sa-dst-address=192.168.70.2
````````

- src-address is the local LAN subnet.
- dst-address is the remote LAN subnet.
- sa-src-address and sa-dst-address are the WAN IPs of the local and remote routers, respectively.

### Step 4: Bypass NAT for VPN Traffic

- To prevent NAT from interfering with the VPN traffic, add a NAT bypass rule on both routers.

#### Router 1:
````````
/ip firewall nat
add chain=srcnat src-address=10.10.11.0/24 dst-address=10.10.12.0/24 action=accept place-before=0
````````

#### Router 2:
````````
/ip firewall nat
add chain=srcnat src-address=10.10.12.0/24 dst-address=10.10.11.0/24 action=accept place-before=0
````````

- Place this rule above the masquerade rule to ensure VPN traffic isn’t NAT’d.

### Step 5: Adjust Firewall (if needed)
- By default, Mikrotik allows IPsec traffic (UDP ports 500 and 4500, and ESP protocol). If your firewall is strict, add these rules:

#### Both Routers:
````````
/ip firewall filter
add chain=input protocol=udp dst-port=500 action=accept
add chain=input protocol=udp dst-port=4500 action=accept
add chain=input protocol=ipsec-esp action=accept
````````

### Step 6: Verify the Tunnel

- 1 Check if the IPsec tunnel is established:
````````
/ip ipsec remote-peers print
````````
- Look for a peer with "established" status.

- 2 Check security associations:
````````
/ip ipsec installed-sa print
````````
- You should see active SAs (security associations).

- 3 Test connectivity by pinging from one LAN to the other:
  - From Router 1: ping 10.10.12.1 src-address=10.10.11.1
  - From Router 2: ping 10.10.11.1 src-address=10.10.12.1
 

### Optional: Customize IPsec Settings

- You can adjust encryption settings for better security or compatibility:

#### Both Routers:
````````
/ip ipsec proposal
set [find default=yes] auth-algorithms=sha256 enc-algorithms=aes-256-cbc pfs-group=modp2048
````````

- sha256 and aes-256-cbc provide strong security.
- pfs-group=modp2048 enables Perfect Forward Secrecy (PFS).

#### Troubleshooting
- Tunnel not establishing: Verify WAN IPs are correct and reachable (ping the remote WAN IP). Check the PSK matches on both sides.
- No traffic passing: Ensure NAT bypass rules are in place and firewall isn’t blocking traffic. Check routing with /ip route print.
- Logs: Use /log print to see IPsec-related errors.

##### This setup creates a secure IPsec site-to-site VPN between two Mikrotik routers, allowing the LANs to communicate as if they were on the same network
