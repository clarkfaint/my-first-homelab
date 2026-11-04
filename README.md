# My First Homelab

Documentation of a simple homelab I built on an old laptop, covering its hardware, network setup, and Docker containers.

## 💻 Hardware Inventory

*   **Host Machine:** HP Laptop (Model: 15-dy1023dx)
*   **Role:** Docker host and application server
*   **Operating System:** Fedora Workstation 44


## 🐳 Container Management

I deploy and monitor the containers through **Portainer**.


## 🚀 Active Services

No services are exposed directly to the public internet. I reach them remotely through Tailscale, and local wildcard DNS resolves their names under `homelab.internal`.

| Service | Purpose | Networking & DNS | Deployment |
| :--- | :--- | :--- | :--- |
| **Pi-hole** | Network-wide ad blocking and local DNS | DNS on port `53` (TCP/UDP); web admin published on port `8888`. A custom `dnsmasq` wildcard maps `*.homelab.internal` locally. | Docker (Portainer) |
| **Tailscale** | Encrypted mesh VPN for remote administration | WireGuard-based. Gives me remote SSH and web access to the host (`<host-IP>`) without opening WAN ports. | Installed on the host |
| **Nextcloud** | Private cloud storage and file sync | Reached through a local reverse proxy. | Docker (persistent volumes) |
| **Jellyfin** | Open-source media streaming | Streams on the local network. | Docker (Portainer) |
| **Dispatcharr** | IPTV playlist, EPG, and VOD management with a stream proxy | Web UI container on the local network. | Docker (Portainer) |
| **Joplin** | Note syncing and storage | Syncs internally so my notes stay under my control. | Docker (Portainer) |

## 📐 Design Decisions

Running a lab includes deciding what's worth keeping, keeping the documentation accurate, and removing what no longer fits.

### 🗑️ Tech Stack Decommissioning: osTicket
* **Decision:** I removed the `osticket-database` and web frontend containers.
* **Why:** osTicket was a good way to learn database containers and port assignments, but it doesn't match the cloud-based helpdesk tools employers use. I moved that time and those resources to web-based helpdesk platforms and simulations.


## 🛠️ Operational Runbooks & Standard Operating Procedures (SOPs)

To keep this single-node host stable and free up storage, I run the following procedures over SSH.

### SOP-01: Storage Cleanup & Container Pruning
**Objective:** Reclaim storage by removing unused Docker data while keeping application volumes.
* **Procedure:** Run this command manually to remove stopped containers, unused networks, images not used by a container, and build cache:
  ```bash
  sudo docker system prune -a
  ```
* **Before running:** Confirm the SOP-02 backups are current, and use `docker ps -a` to check that every container you want to keep is running. `--volumes` is left out on purpose, because it can delete anonymous volumes that hold application data.

### SOP-02: Data & Configuration Backups
**Objective:** Keep copies of important application data and configuration, such as Nextcloud sync paths, Pi-hole blocklists, and Joplin data.
* **Procedure:** Before upgrading the Fedora host or changing its kernel, back up each application's persistent data.
* **What to back up:**
  * `/etc/dnsmasq.d/` (custom local wildcard DNS configuration)
  * Portainer-managed volumes that hold application data
