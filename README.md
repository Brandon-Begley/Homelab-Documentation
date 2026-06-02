# Homelab Documentation

Personal homelab documentation covering hardware, networking, virtualization, storage, Docker, and Active Directory.

## Sections

| Section | Description |
|---------|-------------|
| [Hardware](hardware/README.md) | Physical servers and hardware inventory |
| [Networking](networking/README.md) | VLANs, firewall, and network design |
| [Virtualization](virtualization/README.md) | Proxmox VE, VMs, and containers |
| [Docker](docker/README.md) | Containerized services |
| [Dashboards](dashboards/homepage/README.md) | Homepage and monitoring dashboards |

## Proxmox Nodes

| Node | Role |
|------|------|
| **Hogwarts** | Core services — Plex, Home Assistant, Immich, Homepage, Tailscale, HomeLabel, Pi-hole, AD/DNS |
| **Death-Star** | Monitoring & automation — Mealie, AMP, Uptime Kuma, Prometheus+Grafana, Coder, n8n |
| **MoServer** | Storage — Proxmox + TrueNAS VM (ZFS RAID 10) |
