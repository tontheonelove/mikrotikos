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

➤ Site B (Remote)
```
/interface/wireguard/add name=wg0 listen-port=51820 private-key="B_PRIVATE_KEY"
/ip/address/add address=10.10.10.2/24 interface=wg0

/interface/wireguard/peers/add interface=wg0 public-key="A_PUBLIC_KEY" endpoint-address=1.2.3.4 endpoint-port=51820 allowed-address=10.10.10.1/32 persistent-keepalive=25
```

🔄 2. Configure EoIP Tunnel via WireGuard

➤ Site A

```
/interface/eoip/add name=eoip-tunnel remote-address=10.10.10.2 tunnel-id=100
```

➤ Site B

```
/interface/eoip/add name=eoip-tunnel remote-address=10.10.10.1 tunnel-id=100
```

🌉 3. Set up Bridge to connect EoIP to LAN.

➤ Site A

```
/interface/bridge/add name=bridge1
/interface/bridge/port/add bridge=bridge1 interface=eoip-tunnel
/interface/bridge/port/add bridge=bridge1 interface=LAN_INTERFACE
```
➤ Site B

```
/interface/bridge/add name=bridge1
/interface/bridge/port/add bridge=bridge1 interface=eoip-tunnel
/interface/bridge/port/add bridge=bridge1 interface=LAN_INTERFACE
```

📶 4. (Optional) Enable cross-site DHCP forwarding

- If you want Site A to distribute DHCP to Site B:

- Do not set up a DHCP server at Site B

- Let the client Site B receive the IP from Site A's DHCP via the bridge




