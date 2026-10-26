
# LAB 7 – WEB PROTECTION VỚI pfSense + SQUID + E2GUARDIAN + CLAMAV

- Họ tên: Phan Thị Ái My
- MSSV: 1150080106
- Lớp: 11CNPM2

## 1. Môi trường thực hành

- **Phần mềm ảo hóa:** VMware Workstation
- **Firewall:** pfSense CE 2.7.2
- **Web Proxy:** Ubuntu Server 24.04 LTS
- **Web Server:** Ubuntu Server + Nginx
- **Domain Controller:** Windows Server 2022
- **Máy Client:** Windows

## 2. Danh sách máy ảo và địa chỉ IP

| Máy ảo | Địa chỉ IP | Vai trò |
|---|---|---|
| pfSense WAN | 192.168.250.128/24 | Kết nối Internet |
| pfSense MGMT | 192.168.56.254/24 | Quản trị pfSense |
| pfSense LAN | 10.6.0.1/24 | Gateway LAN |
| pfSense DMZ | 172.16.6.1/24 | Gateway DMZ |
| DC01 | 10.6.0.10/24 | Active Directory, DNS |
| WEB-PROXY | 10.6.0.30/24 | Squid, E2Guardian, ClamAV |
| DMZ-WEB | 172.16.6.10/24 | Nginx, website mô phỏng |
| Client-A | 10.6.0.21/24 | Máy kiểm thử |
| Client-B | 10.6.0.22/24 | Máy kiểm thử |

## 3. Cấu hình mạng VMware

| VMnet | Subnet | Chức năng |
|---|---|---|
| VMnet8 | 192.168.250.0/24 | WAN – NAT |
| VMnet2 | 192.168.56.0/24 | MGMT |
| VMnet3 | 10.6.0.0/24 | LAN |
| VMnet4 | 172.16.6.0/24 | DMZ |

## 4. Gateway và DNS

- **LAN Gateway:** 10.6.0.1
- **DMZ Gateway:** 172.16.6.1
- **DNS Server:** 10.6.0.10
- **AD Domain:** lab6.test
- **LAB 7 DNS Zone:** lab7.test

## 5. Dịch vụ sử dụng

| Dịch vụ | Địa chỉ |
|---|---|
| pfSense WebGUI | https://192.168.56.254 |
| E2Guardian | 10.6.0.30:8080 |
| Squid | 127.0.0.1:3128 |
| DNS | 10.6.0.10:53 |
| Nginx | 172.16.6.10:80 |

## 6. Danh sách website LAB 7

Tất cả website trỏ về `172.16.6.10`.

- normal.lab7.test
- china.lab7.test
- mideast.lab7.test
- jobs.lab7.test
- weapons.lab7.test
- politics.lab7.test
- religion.lab7.test
- education.lab7.test
- entertainment.lab7.test
- games.lab7.test
- streaming.lab7.test
- blockedgame.lab7.test
- download.lab7.test

## 7. Tài nguyên sử dụng

- LAB7_Resources
- Script tạo 13 website Nginx
- Script DNS LAB 7
- Squid Proxy
- E2Guardian Web Filter
- ClamAV Antivirus
- File kiểm thử: 10MB.bin, 100MB.bin, 110MB.bin
- File kiểm thử antivirus: eicar.com.txt

## 8. Mô hình kết nối

Client-A / Client-B → E2Guardian (10.6.0.30:8080) → Squid (127.0.0.1:3128) → pfSense → Internet / DMZ-WEB
