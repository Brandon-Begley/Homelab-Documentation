# Homelab Documentation

Personal homelab documentation covering hardware, networking, virtualization, storage, Docker, and Active Directory.

---

## Infrastructure Overview

### Proxmox Nodes

| Node | Role | Key Services |
|------|------|-------------|
| [Hogwarts](virtualization/hogwarts.md) | Core homelab services | Plex, Home Assistant, Immich, Homepage, Tailscale, Homelable, Pi-hole, DC01/DC02 |
| [Death-Star](virtualization/death-star.md) | Monitoring, automation, dev & gaming | Mealie, AMP, Uptime Kuma, Prometheus+Grafana, Coder, n8n |
| [MoServer](hardware/hp-microserver-gen10.md) | NAS / Storage | TrueNAS VM (ZFS RAID 10) |

### Network

| VLAN | Name | Subnet | Purpose |
|------|------|--------|---------|
| 10 | Management | 10.0.10.0/24 | Network infrastructure (UniFi, switches, APs) |
| 20 | Homelab / Servers | 10.0.20.0/24 | All three Proxmox nodes and their VMs/containers |
| 30 | IoT Devices | 10.0.30.0/24 | Smart home and IoT devices |
| 40 | LAN / Trusted | 10.0.40.0/24 | User endpoints — desktops, laptops |

All VLANs are routed and firewalled through a **UniFi Dream Machine Pro**.

---

## Sections

| Section | Description |
|---------|-------------|
| [Hardware](hardware/README.md) | Physical server inventory |
| [Networking](networking/README.md) | VLANs, UniFi, firewall rules |
| [Virtualization](virtualization/README.md) | Proxmox nodes, VMs, and containers |
| [Docker](docker/README.md) | Containerized services and Dockge |
| [Dashboards](dashboards/homepage/README.md) | Homepage dashboard configuration |

---

## Services

### Hogwarts — Core Services

| Service | Purpose |
|---------|---------|
| [Plex](virtualization/hogwarts.md#plex) | Media server |
| [Home Assistant](virtualization/hogwarts.md#home-assistant) | Home automation hub |
| [Immich](virtualization/hogwarts.md#immich) | Self-hosted photo and video backup |
| [Homepage](virtualization/hogwarts.md#homepage) | Homelab dashboard |
| [Tailscale](virtualization/hogwarts.md#tailscale) | Mesh VPN for remote access |
| [Homelable](virtualization/hogwarts.md#homelable) | Interactive infrastructure network visualizer |
| [Pi-hole](virtualization/hogwarts.md#pi-hole) | DNS ad blocking and local DNS records |
| DC01 / DC02 | Active Directory and DNS (Windows Server VMs) |

### Death-Star — Monitoring, Automation & Dev

| Service | Purpose |
|---------|---------|
| [Mealie](virtualization/death-star.md#mealie) | Self-hosted recipe manager and meal planner |
| [AMP](virtualization/death-star.md#amp-game-server-hosting) | Game server management (Minecraft, etc.) |
| [Uptime Kuma](virtualization/death-star.md#uptime-kuma) | Service uptime and health monitoring |
| [Prometheus + Grafana](virtualization/death-star.md#prometheus--grafana) | Metrics collection and visualization dashboards |
| [Coder](virtualization/death-star.md#coder) | Self-hosted cloud development environments |
| [n8n](virtualization/death-star.md#n8n) | Workflow automation |

### MoServer — Storage

| Service | Purpose |
|---------|---------|
| [TrueNAS](virtualization/truenas.md) | Centralized NAS via ZFS mirrored vdev pool (RAID 10 equivalent) |

---

## Storage

TrueNAS runs as a VM on MoServer (HP ProLiant MicroServer Gen10) under Proxmox, with physical drives passed through directly for ZFS management. The pool uses **mirrored vdevs** — ZFS's equivalent of RAID 10 — providing both redundancy and read/write performance across two drive pairs.

See [TrueNAS documentation](virtualization/truenas.md) for pool layout, shares, and ZFS details.

---

## Active Directory

Two Windows Server VMs hosted on Hogwarts provide domain services:

- **DC01** — Primary Domain Controller
- **DC02** — Secondary Domain Controller

Services provided: Active Directory, DNS, authentication infrastructure.

See [Windows Servers documentation](virtualization/windows-servers.md) for details.
