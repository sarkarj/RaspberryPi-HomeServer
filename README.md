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
