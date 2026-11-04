
# LAB 9 – Remote DNS Cache Poisoning


## Môi trường thực hành

- Phần mềm ảo hóa: VMware Workstation
- Firewall/Router: pfSense
- DNS Resolver: Ubuntu Server, BIND9
- Attacker: Kali Linux
- Victim: Ubuntu Linux
- Công cụ: Python 3, dnspython, tcpdump, Wireshark, WinSCP

## Sơ đồ mạng và địa chỉ IP

| Máy ảo | Địa chỉ IP | Mạng VMware | Vai trò |
|---|---|---|---|
| pfSense – WAN | DHCP | VMnet8 | NAT |
| pfSense – MGMT | 192.168.56.254 | VMnet2 | Quản trị |
| pfSense – LAN | 10.6.0.1 | VMnet3 | LAN-DOMAIN |
| pfSense – DMZ | 172.16.6.1 | VMnet4 | Gateway DMZ |
| pfSense – DNSLAB | 10.9.0.1 | VMnet5 | Gateway DNSLAB |
| pfSense – ATTACKNET | 10.10.0.1 | VMnet6 | Gateway ATTACKNET |
| DNS-SERVER | 10.9.0.10 | VMnet5 | DNS Resolver |
| VICTIM | 10.9.0.100 | VMnet5 | Máy kiểm thử |
| APP-TEST-AUTH-DNS | 172.16.6.20 | VMnet4 | Authoritative DNS / Relay |
| DMZ-WEB | 172.16.6.10 | VMnet4 | Website hợp lệ |
| ATTACKER-KALI | 10.10.0.200 | VMnet6 | Mô phỏng attacker |

## Nội dung thực hành

1. Cấu hình mạng DNSLAB, DMZ và ATTACKNET trên VMware.
2. Thiết lập Firewall Rules trên pfSense.
3. Kiểm tra DNS Baseline.
4. Chứng minh Kali không sniff được lưu lượng DNS upstream.
5. Mô phỏng Remote DNS Cache Poisoning.
6. Phân tích gói DNS bằng Wireshark.
7. Kiểm tra kết quả phân giải DNS trên VICTIM.
8. Khôi phục BIND9 và kiểm tra các biện pháp hardening.


