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

## Key Infrastructure

- **Primary Proxmox node** — Windows Server DCs (AD/DNS), media services, containers
- **HP MicroServer Gen10** — Proxmox + TrueNAS VM with ZFS mirror vdev pool (RAID 10) for centralized NAS storage
