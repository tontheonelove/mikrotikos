Example of settings (recommended):
📌 Method 1: EoIP over SSTP (secure and use the same Gateway)
Site A: Set VPN Server (e.g. SSTP)

Set PPP > Profiles and Secrets

Set SSTP Server: Enable, enter Certificate, use the open Port (443)

Site B: Connect SSTP to Site A

Set PPP Client: Use SSTP Protocol, Username/Password from step 1

Get Tunnel Interface, e.g. sstp-out1

Create EoIP Tunnel on both Sites


Site A:

``
Interface > Add > EoIP Tunnel

Remote Address: IP WAN Site B

Tunnel ID: 1

Bind to Interface: sstp-in1
``


Site B:

``
Interface > Add > EoIP Tunnel

Remote Address: IP WAN Site A

Tunnel ID: 1

Bind to Interface: sstp-out1
``


Bridge EoIP to LAN of both Sites

Bridge > Add > Put ether2, eoip-tunnel1 together

Site B will get IP/DHCP as if connected to LAN of Site A



🚨 Note:
Make sure Site B has internet that can connect VPN to Site A

VPN should have encryption (SSTP / IPsec) for security

EoIP is not recommended to cross NAT directly, should be used with SSTP or other VPN


