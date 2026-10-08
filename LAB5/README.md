# LAB5: TRIỂN KHAI VÀ CẤU HÌNH FIREWALL PFSENSE


* **Họ và tên:** Phan Thị Ái My
* **MSSV:** 1150080106
* **Lớp:** 11CNPM2



---

## CÔNG CỤ VÀ MÔI TRƯỜNG THỰC HIỆN
* **Phần mềm ảo hóa:** VMware Workstation Pro.
* **Hệ điều hành tường lửa:** pfSense 2.7.2-RELEASE (FreeBSD).
* **Hệ điều hành máy trạm / máy chủ kiểm thử:**
  * Windows Server 2022 (Domain Controller - LAN).
  *  Ubuntu 64-bit (Client - LAN).
  * Windows Server (DMZ-Web).
* **Công cụ dòng lệnh kiểm thử:** `ping`, `nslookup`, `curl`, `ipconfig` / `ip a`, `pfctl`, `netsh`.
* **Trình duyệt quản trị Web:** Trình duyệt web trên máy tính vật lý truy cập qua WebConfigurator (`https://10.0.0.1`).

---

## SƠ ĐỒ PHÂN BỔ ĐỊA CHỈ IP (IP ADDRESSING SCHEME)

| Tên thiết bị / Phân vùng | Card mạng ảo (VMware) | Interface pfSense | Địa chỉ IP / Subnet Mask | Default Gateway | Chức năng |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **pfSense (WAN)** | NAT | `em0` | `192.168.80.132/24` (DHCP) | `192.168.80.2` | Kết nối ra Internet bên ngoài |
| **pfSense (LAN)** | Custom (`VMnet1` - Host-only) | `em1` | `10.0.0.1/8` | — | Gateway quản trị và phân vùng nội bộ |
| **pfSense (DMZ)** | LAN Segment (`DMZ-Net`) | `em2` | `172.16.0.1/16` | — | Gateway phân vùng máy chủ công cộng |
| **Host PC (Máy thật)** | `VMware Network Adapter VMnet1` | — | `10.0.0.100/8` | `10.0.0.1` | Máy trạm quản trị pfSense WebGUI |
| **Windows Server 2022** | Custom (`VMnet1` - Host-only) | — | `10.0.0.2/8` | `10.0.0.1` | Máy chủ Domain Controller trong LAN |
| **Ubuntu Client** | Custom (`VMnet1` - Host-only) | — | `10.0.0.3/8` | `10.0.0.1` | Máy trạm Linux kiểm thử trong LAN |
| **DMZ-Web** | LAN Segment (`DMZ-Net`) | — | `172.16.0.2/16` | `172.16.0.1` | Máy chủ dịch vụ Web đặt tại vùng DMZ |

---

## NỘI DUNG VÀ KẾT QUẢ THỰC HIỆN CHÍNH
* **Tình huống 1:** Kiểm soát dịch vụ mạng LAN — Chặn ICMP (ping) ra ngoài Internet, chỉ cho phép phân giải tên miền (DNS port 53) và duyệt Web (HTTP/HTTPS port 80, 443).
* **Tình huống 2:** Hạn chế truy cập Internet — Vô hiệu hóa truy cập Web từ LAN ra ngoài, duy trì kết nối mạng nội bộ và dịch vụ quản trị pfSense.
* **Tình huống 3:** Cô lập DMZ với LAN — Thiết lập rule trên vùng DMZ cho phép truy cập Internet bình thường nhưng chặn toàn bộ các kết nối từ DMZ tới mạng nội bộ LAN (`10.0.0.0/8`).
