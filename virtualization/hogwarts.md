# Hogwarts — Proxmox Node

Hogwarts is the primary Proxmox node in the homelab. It hosts the majority of core self-hosted services used daily, running a mix of VMs and LXC containers.

---

## Node Overview

| Property | Value |
|----------|-------|
| **Hostname** | Hogwarts |
| **Hypervisor** | Proxmox VE |
| **Network VLAN** | VLAN 20 (Homelab / Servers) |
| **Management Access** | Web UI over HTTPS |
| **Primary Role** | Core homelab services |

---

## Hosted Services

| Service | Type | Purpose |
|---------|------|---------|
| [Plex](#plex) | LXC / VM | Media server |
| [Home Assistant](#home-assistant) | LXC / VM | Home automation hub |
| [Immich](#immich) | LXC / VM | Self-hosted photo backup and management |
| [Homepage](#homepage) | LXC | Homelab dashboard |
| [Tailscale](#tailscale) | LXC | VPN / secure remote access |
| [HomeLabel](#homelabel) | LXC / VM | Home inventory / labeling |
| [Pi-hole](#pi-hole) | LXC | DNS-level ad blocking |

---

## Service Details

### Plex

Plex Media Server provides centralized media streaming for the homelab. Storage for media libraries is sourced from the MoServer TrueNAS NAS over the network.

Key considerations:
- Mount points and storage passthrough from NAS
- UID/GID permission alignment between Proxmox container and TrueNAS shares
- Performance tuning for transcoding
- Service isolation from core infrastructure

---

### Home Assistant

Home Assistant serves as the home automation hub, managing smart home devices, automations, and integrations across the house.

---

### Immich

Immich provides self-hosted photo and video backup — a privacy-respecting alternative to Google Photos. It handles automatic mobile backups and provides a browsable photo library.

---

### Homepage

[Homepage](https://gethomepage.dev) is used as the homelab dashboard, providing a single-pane-of-glass view of all running services, their status, and quick navigation links.

See the [Homepage dashboard documentation](../dashboards/homepage/README.md) for configuration details.

---

### Tailscale

Tailscale provides a secure mesh VPN for remote access to homelab services. It enables connectivity to the homelab from outside the local network without exposing ports to the internet.

Key benefits:
- Zero-config WireGuard-based VPN
- Access to internal services (Proxmox UI, Home Assistant, etc.) remotely
- No public port forwarding required

---

### HomeLabel

HomeLabel is used for home inventory and labeling — tracking physical items, their locations, and generating labels for organized storage.

---

### Pi-hole

Pi-hole acts as the homelab's DNS server and network-wide ad blocker. All DNS queries from the local network are routed through Pi-hole for filtering.

Key roles:
- DNS-level ad and tracker blocking
- Local DNS records for homelab services (internal hostnames)
- Query logging and analytics

---

## Windows Server VMs

Hogwarts also hosts the Active Directory domain controllers:

- **DC01** – Primary Domain Controller
- **DC02** – Secondary Domain Controller

These provide Active Directory and DNS services for the homelab. See [Windows Servers](windows-servers.md) for full documentation.

---

## Networking

- Proxmox management traffic on VLAN 20
- VMs and containers inherit VLAN placement via Linux bridges
- Pi-hole handles internal DNS resolution for all services
- Tailscale provides remote access without port forwarding
