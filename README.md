# RaspberryPi-HomeServer
Raspberry Pi Home Server Deployment, Preparation, Install and Secure Docker, WireGuard PiVPN Setup, Deploy Dockerized Services - Pi-hole DNS, Prometheus + Grafana Dashboard Monitoring RPI, Home Assistant, NGINX Reverse Proxy with SSL, Firewall Configuration (UFW), Router Port Forwarding

## 📡 System Architecture 
<img src="ArchitectureDiagram.png" width="500"> <img src="OverallArchitecture.png" width="250">

## 📄 Services and Exposure
| Service              | Docker?| Port(s) Exposed Internally                       | Port(s) Exposed Publicly| Protocol| Notes |
|----------------------|------- |--------------------------------------------------|-------------------------|---------|-------|
| WireGuard VPN (PiVPN)| Host   | 51820 (UDP)                                      | 51820 (UDP) (forwarded) | UDP     | VPN access only|
| NGINX Reverse Proxy  | Docker | 443 (mapped)                                     | 443 (TCP) (forwarded)   | TCP     | SSL termination|
| contact_client App   | Docker | 8080                                             | None                    | TCP     | Behind NGINX| 
| urlrepo App          | Docker | 8085                                             | None                    | TCP     | Behind NGINX|
| Portainer            | Docker | 8000, 9443                                       | None                    | TCP     | LAN/VPN only|
| Pi-hole              | Docker | 53 (TCP/UDP), 67 (UDP), 8081 (HTTP), 8443 (HTTPS)| None                    | TCP, UDP| LAN DNS Adblocker|
| Redis                | Docker | 6379                                             | None                    | TCP     | LAN-only|
| Grafana              | Docker | 3000                                             | None                    | TCP     | LAN dashboard|
| node_exporter        | Docker | 9100                                             | None                    | TCP     | Metrics|
| Prometheus           | Docker | 9090                                             | None                    | TCP     | Metrics|
| Watchtower           | Docker | 8080                                             | None                    | TCP     | Container auto-update|
| Home Assistant       | Docker | 8123                                             | None                    | TCP     | Smart home hub|
| RPi Connect          | Host   | 44353, 5367, 6780                                | None                    | TCP     | LAN/VPN only |

## 🖥 Raspberry Pi Preparation

```bash
ssh pi@<Raspberry Pi IP>
...
cat /etc/os-release

PRETTY_NAME="Raspbian GNU/Linux 12 (bookworm)"
NAME="Raspbian GNU/Linux"
VERSION_ID="12"
VERSION="12 (bookworm)"
VERSION_CODENAME=bookworm
ID=raspbian
ID_LIKE=debian
HOME_URL="http://www.raspbian.org/"
SUPPORT_URL="http://www.raspbian.org/RaspbianForums"
BUG_REPORT_URL="http://www.raspbian.org/RaspbianBugs"

uname -a
Linux raspberrypi 6.12.22-v8+ #1872 SMP PREEMPT Tue Apr 15 15:46:58 BST 2025 aarch64 GNU/Linux
```

 - ✅ OS Details
 - ✅ Static IP address configured in Router (DHCP reservation)
 - ✅ SSH hardened: PasswordAuthentication: No. PublicKeyAuthentication: Yes. Only key-based SSH access

## 🛡️ WireGuard VPN Setup (via PiVPN)

```bash
curl -L https://install.pivpn.io | bash
```
During setup: Select WireGuard, choose default ports (51820/UDP)

 🔍 After Install Useful Commands:

```bash
pivpn add             # Add clients
pivpn list            # View connected clients
pivpn -qr             # Export client QR code
pivpn -c              # View VPN status
pivpn -bk             # Backup PiVPN
pivpn -d              # Debug


sudo systemctl start wg-quick@wg0
sudo systemctl enable wg-quick@wg0
```
Router Port Forwarding: Forward 51820/UDP to your Raspberry Pi internal IP.
