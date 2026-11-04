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

Rather than exposing services directly to the public internet, the lab is architected around a Zero-Trust network layer and a local wildcard DNS infrastructure. Local subdomains are securely routed within the `homelab.internal` zone.

| Service | Primary Purpose | Networking & DNS Configuration | Deployment Stack |
| :--- | :--- | :--- | :--- |
| **Pi-hole** | Network-wide ad blocking & Local DNS authoritative server | DNS on port `53` (TCP/UDP); web admin published on port `8888`. Configured custom `dnsmasq` wildcard routing (`*.homelab.internal`) to map subdomains locally. | Docker (Portainer Container) |
| **Tailscale** | Encrypted mesh VPN for secure remote administration | WireGuard-based overlay network. Allows seamless remote SSH/web access to local machine node (`<host-IP>`) without opening WAN ports. | Native Host Daemon |
| **Nextcloud** | Private cloud storage, sync, & data virtualization | Routed locally via custom internal reverse proxy mapping for secure file transactions. | Docker (Persistent Volumes) |
| **Jellyfin** | Open-source media streaming & asset orchestration | Local loopback asset streaming, segmented within internal network layer. | Docker (Portainer managed) |
| **Dispatcharr** | IPTV playlist, EPG, and VOD management with a stream proxy | Web UI container on the local network. | Docker (Portainer managed) |
| **Joplin** | Decentralized notebook syncing & database storage | Configured with internal synchronization pathways to maintain local data control. | Docker (Portainer managed) |

## 📐 Design Decisions & Software Lifecycle

A critical part of managing a production or lab environment is evaluating software viability, maintaining documentation accuracy, and decommissioning deprecated components.

### 🗑️ Tech Stack Decommissioning: osTicket
* **Decision:** Intentionally removed the `osticket-database` and frontend web container infrastructure from the active deployment stack.
* **Engineering Rationale:** While osTicket provided an excellent initial baseline for understanding database containerization and local port assignments, the platform lacks alignment with modern enterprise cloud ecosystems. To maximize professional skill relevance, infrastructure resources were repurposed away from legacy self-hosted ticketing solutions to focus heavily on modern web-based enterprise helpdesk platforms and simulation suites.


## 🛠️ Operational Runbooks & Standard Operating Procedures (SOPs)

To keep this single-node host stable and free up storage, I run the following procedures over SSH.

### SOP-01: Storage Cleanup & Container Pruning
**Objective:** Reclaim storage by removing unused Docker data while keeping application volumes.
* **Procedure:** Run this command manually to remove stopped containers, unused networks, images not used by a container, and build cache:
  ```bash
  sudo docker system prune -a
  ```
* **Before running:** Confirm the SOP-02 backups are current, and use `docker ps -a` to check that every container you want to keep is running. `--volumes` is left out on purpose, because it can delete anonymous volumes that hold application data.

### SOP-02: Data Persistence & Configuration Backups
**Objective:** Maintain configuration baselines for critical localized application databases (e.g., Nextcloud sync paths, Pi-hole blocklists, and Joplin storage data).
* **Procedure:** Before any upstream host upgrades or kernel modifications on the underlying Fedora platform, all application persistent volume spaces are isolated, staged, and snapshotted.
* **Target Configurations:** 
  * `/etc/dnsmasq.d/` (Custom local wildcard DNS configuration)
  * Local Portainer volume mappings for user database stores
