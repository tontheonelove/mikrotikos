# WireGuard + EoIP (Ethernet over IP)


🎯 Goal

- Connect two sites via WireGuard VPN

- Use EoIP to create a bridge between site A and B

- Support broadcast and cross-site DHCP

💡 Prerequisites

- MikroTik RouterOS v7.x or higher (because WireGuard is only supported in v7)

- Public IP or Dynamic DNS on both sides

- Each router must have access to the Internet.

🛠️ Setup steps

🔐 1. Setup  WireGuard (Site A and Site B)

➤ Site A (Main)
```
/interface/wireguard/add name=wg0 listen-port=51820 private-key="A_PRIVATE_KEY"

/ip/address/add address=10.10.10.1/24 interface=wg0

/interface/wireguard/peers/add interface=wg0 public-key="B_PUBLIC_KEY" endpoint-address=5.6.7.8 endpoint-port=51820 allowed-address=10.10.10.2/32 persistent-keepalive=25
```
