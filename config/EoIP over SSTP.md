📌 1: EoIP over SSTP (ปลอดภัย และใช้ Gateway เดิมได้)
Site A: ตั้ง VPN Server (เช่น SSTP)

ตั้ง PPP > Profiles และ Secrets

ตั้ง SSTP Server: Enable, ใส่ Certificate, ใช้ Port ที่เปิดไว้ (443)

Site B: เชื่อม SSTP ไป Site A

ตั้ง PPP Client: ใช้ SSTP Protocol, Username/Password จากข้อ 1

ให้ได้ Tunnel Interface เช่น sstp-out1

สร้าง EoIP Tunnel บนทั้ง 2 Site

Site A:

`Interface > Add > EoIP Tunnel
Remote Address: IP WAN ของ Site B (หรือ VPN IP)
Tunnel ID: 1
Bind to Interface: sstp-in1`

Site B:

`
Interface > Add > EoIP Tunnel
Remote Address: IP WAN ของ Site A
Tunnel ID: 1 (ต้องตรงกัน)
Bind to Interface: sstp-out1
`

Bridge EoIP เข้ากับ LAN ของทั้งสอง Site

Bridge > Add > ใส่ ether2, eoip-tunnel1 เข้าด้วยกัน

Site B จะได้ IP/DHCP เหมือนต่ออยู่กับ LAN ของ Site A

