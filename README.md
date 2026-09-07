# my-first-homelab
Documentation of a simple homelab created on an old laptop. 
# My Homelab Documentation

Welcome to my homelab repository. This project serves as a central hub for documenting the network architecture, hardware deployment, and active Docker containers running in my home environment.

## 💻 Hardware Inventory

*   **Host Machine:** HP Laptop (Model: 15-dy1023dx)
*   **Role:** Primary Docker Node / Application Server
*   **Operating System:** Fedora Workstation 44


## 🐳 Container Management

All localized containers are deployed and monitored visually using **Portainer**. 


## 🚀 Active Services & Architectural Matrix

Rather than exposing services directly to the public internet, the lab is architected around a Zero-Trust network layer and a localized Split-Horizon DNS infrastructure. Local subdomains are securely routed within the `homelab.internal` zone.

| Service | Primary Purpose | Networking & DNS Configuration | Deployment Stack |
| :--- | :--- | :--- | :--- |
| **Pi-hole** | Network-wide ad blocking & Local DNS authoritative server | Listening on Port `8888`. Configured custom `dnsmasq` wildcard routing (`*.homelab.internal`) to map subdomains locally. | Docker (Portainer Container) |
| **Tailscale** | Encrypted mesh VPN for secure remote administration | WireGuard-based overlay network. Allows seamless remote SSH/web access to local machine node (`192.168.68.65`) without opening WAN ports. | Native Host Daemon |
| **Nextcloud** | Private cloud storage, sync, & data virtualization | Routed locally via custom internal reverse proxy mapping for secure file transactions. | Docker (Persistent Volumes) |
| **Jellyfin** | Open-source media streaming & asset orchestration | Local loopback asset streaming, segmented within internal network layer. | Docker (Portainer managed) |
| **Dispatcharr** | Media queue pipeline & automation orchestration | Web UI container hooked into localized application API networks. | Docker (Portainer managed) |
| **Joplin** | Decentralized notebook syncing & database storage | Configured with internal synchronization pathways to maintain local data control. | Docker (Portainer managed) |


## 🛠️ Maintenance & Useful Commands

### Quick Docker Cleanup
Run this via SSH or the Portainer console to clean up unused images and free up space on the laptop storage:
```bash
docker system prune -a --volumes
```

### Backing Up Configuration Data
Ensure persistent volume data (like Nextcloud databases or Pi-hole lists) is backed up before running host updates:

```
