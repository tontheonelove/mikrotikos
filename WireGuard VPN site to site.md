## WireGuard VPN site to site

### Prerequisites:
- MikroTik RouterOS 7.x or higher.
- WireGuard installed and enabled on both routers.
- Knowledge of the IP addressing scheme and interfaces on both routers.


#### Steps for Router 1 (Server):
- Install WireGuard on Router 1: Ensure you have RouterOS 7.x or higher. If not, update the firmware.

- Create WireGuard Interface: Go to the Interfaces menu and create a new WireGuard interface.

``````
/interface wireguard add name=wg0 listen-port=51820 private-key="private_key_of_router1"
``````
- Replace private_key_of_router1 with the actual private key of Router 1. You can generate this using the command:
``````
/interface wireguard generate-key-pair
``````
- Assign IP Address to WireGuard Interface: Assign an IP address to the WireGuard interface. This IP will be used for communication with Router 2.
``````
/ip address add address=10.10.10.1/24 interface=wg0
``````
- Create WireGuard Peer for Router 2: Set up the peer for Router 2 (client).

``````
/interface wireguard peers add public-key="public_key_of_router2" allowed-address=10.10.10.2/32 endpoint-address=<Router2_Public_IP> endpoint-port=51820 interface=wg0
``````
- Replace public_key_of_router2 with the public key of Router 2.
- Replace <Router2_Public_IP> with the public IP address of Router 2.
- Configure Firewall: Set up the firewall rules to allow WireGuard traffic (UDP port 51820).

``````
/ip firewall filter add chain=input protocol=udp dst-port=51820 action=accept
``````
- Set up Routing (optional): If you want Router 1 to route traffic to other networks via Router 2, you need to configure routing.

``````
/ip route add dst-address=10.10.10.2/32 gateway=wg0
``````

#### Steps for Router 2 (Client):
- Install WireGuard on Router 2: Make sure RouterOS 7.x or higher is installed on Router 2.

- Create WireGuard Interface: Create a WireGuard interface on Router 2.

``````
/interface wireguard add name=wg0 private-key="private_key_of_router2"
``````
- Replace private_key_of_router2 with the actual private key of Router 2.
- Assign IP Address to WireGuard Interface: Assign an IP address to the WireGuard interface on Router 2.

``````
/ip address add address=10.10.10.2/24 interface=wg0
``````
- Create WireGuard Peer for Router 1: Add Router 1's public key as the peer on Router 2.

``````
/interface wireguard peers add public-key="public_key_of_router1" allowed-address=10.10.10.1/32 endpoint-address=<Router1_Public_IP> endpoint-port=51820 interface=wg0
``````
- Replace public_key_of_router1 with Router 1’s public key.
- Replace <Router1_Public_IP> with the public IP address of Router 1.
- Configure Firewall: Set up the firewall rules to allow WireGuard traffic (UDP port 51820).

``````
/ip firewall filter add chain=input protocol=udp dst-port=51820 action=accept
``````
- Set up Routing (optional): If you want Router 2 to route traffic to other networks via Router 1, configure routing.

``````
/ip route add dst-address=<desired_network>/24 gateway=wg0
``````
#### Testing the Connection:
- From Router 2, ping Router 1’s WireGuard IP (10.10.10.1). If the connection is successful, the VPN is set up correctly.
- Similarly, you can ping Router 2 from Router 1.
