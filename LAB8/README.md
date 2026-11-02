# LAB 8 – LOCAL DNS ATTACK

- Họ tên: Phan Thị Ái My
- MSSV: 1150080106
- Lớp: 11CNPM2

## 1. Môi trường thực hành

- **Phần mềm ảo hóa:** VMware Workstation
- **Firewall / Router:** pfSense
- **DNS Server:** Ubuntu Server + BIND9
- **Victim:** Ubuntu
- **Attacker:** Kali Linux + Python/Scapy
- **Web mô phỏng:** HTTP Server / Nginx
- **Phân tích gói tin:** Wireshark

## 2. Sơ đồ địa chỉ IP

| Thiết bị | Địa chỉ IP | Vai trò |
|---|---|---|
| pfSense – DNSLAB | 10.9.0.1/24 | Gateway mạng DNSLAB |
| DNS-SERVER | 10.9.0.10/24 | DNS Resolver BIND9 |
| VICTIM | 10.9.0.100/24 | Máy thực hiện truy vấn DNS |
| ATTACKER-KALI | 10.9.0.200/24 | Máy thử nghiệm DNS Spoofing và Web mô phỏng |
| pfSense – DMZ | 172.16.6.1/24 | Gateway mạng DMZ |
| DMZ-WEB | 172.16.6.10/24 | Web Server hợp lệ |
| APP-TEST-AUTH-DNS | 172.16.6.20/24 | Máy mô phỏng Authoritative DNS có độ trễ |

## 3. Cấu hình mạng ảo

- **VMnet5 – DNSLAB:** 10.9.0.0/24
- **VMnet4 – DMZ:** 172.16.6.0/24

## 4. Thông tin tên miền kiểm thử

- **Tên miền kiểm thử:** www.external.lab9.test
- **Địa chỉ IP phân giải hợp lệ:** 172.16.6.10
- **Địa chỉ IP giả trong thí nghiệm:** 10.9.0.200
