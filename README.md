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


## 🚀 Active Services & Applications

| Service | Purpose | Access URL |
| :--- | :--- | :--- |
| **Pi-hole** | Network-wide ad blocking & local DNS management |
| **Nextcloud** | Private cloud storage, file sharing, and productivity |
| **Jellyfin** | Free, open-source media streaming and organization server |
| **Tailscale** | Secure mesh VPN for remote access to my local network | Managed via Tailscale Admin Console |
| **Dispatcharr** | Media queue orchestration and request dispatch management |
| **Joplin** | Open-source note-taking and to-do application with synchronization | 

## 🛠️ Maintenance & Useful Commands

### Quick Docker Cleanup
Run this via SSH or the Portainer console to clean up unused images and free up space on the laptop storage:
```bash
docker system prune -a --volumes
```

### Backing Up Configuration Data
Ensure persistent volume data (like Nextcloud databases or Pi-hole lists) is backed up before running host updates:

```
