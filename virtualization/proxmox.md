# Proxmox Virtualization Environment

Proxmox VE is the core virtualization platform across the homelab, deployed on three physical nodes: **Hogwarts**, **Death-Star**, and **MoServer**.

The goal of this setup is to gain hands-on experience with enterprise-style virtualization while supporting real services used daily.

---

## Why Proxmox

Proxmox was chosen for the following reasons:
- Native support for both VMs and LXC containers
- Web-based management interface
- Strong networking and VLAN capabilities
- Snapshot and backup support
- Widely used in homelab and small enterprise environments

---

## Cluster Overview

| Node | Hostname | Primary Role |
|------|----------|-------------|
| [Hogwarts](hogwarts.md) | `hogwarts` | Core homelab services (Plex, Home Assistant, Immich, Pi-hole, etc.) |
| [Death-Star](death-star.md) | `death-star` | Monitoring, automation, dev tools, game servers |
| [MoServer](../hardware/hp-microserver-gen10.md) | `moserver` | NAS / Storage — TrueNAS VM with ZFS RAID 10 |

All nodes reside on **VLAN 20 (Homelab / Servers)** and are managed via their respective Proxmox web UIs over HTTPS.

---

## Node Details

### Hogwarts

The primary node hosting most core self-hosted services. See [Hogwarts documentation](hogwarts.md) for the full service list.

Key services: Plex, Home Assistant, Immich, Homepage, Tailscale, HomeLabel, Pi-hole, DC01/DC02 (Active Directory)

---

### Death-Star

The secondary node dedicated to monitoring infrastructure, automation, development tooling, and game server hosting. See [Death-Star documentation](death-star.md) for the full service list.

Key services: Mealie, AMP, Uptime Kuma, Prometheus + Grafana, Coder, n8n

---

### MoServer

The HP ProLiant MicroServer Gen10 running Proxmox with a single TrueNAS VM. Physical drives are passed through directly to TrueNAS for ZFS management. See the [hardware doc](../hardware/hp-microserver-gen10.md) and [TrueNAS doc](truenas.md) for details.

Key service: TrueNAS (ZFS mirrored vdev pool — RAID 10 equivalent)

---

## Virtualization Design Principles

### VMs vs LXC Containers

| Use Case | Technology | Reason |
|----------|-----------|--------|
| Windows workloads (AD/DNS) | VM | Requires full OS isolation |
| Direct hardware access (TrueNAS) | VM + disk passthrough | ZFS needs direct disk control |
| Lightweight Linux services | LXC container | Lower overhead, faster startup |

### Windows Server VMs (Hogwarts)

Two Windows Server VMs provide Active Directory and DNS:

- **DC01** – Primary Domain Controller
- **DC02** – Secondary Domain Controller

See [Windows Servers](windows-servers.md) for full documentation.

### TrueNAS VM (MoServer)

TrueNAS runs as a VM on MoServer with physical disk passthrough. It provides centralized NAS storage using a ZFS mirrored vdev pool (RAID 10 equivalent). See [TrueNAS documentation](truenas.md) for details.

---

## Networking Integration

Proxmox is integrated into the VLAN-based network design across all nodes:

- Management traffic on VLAN 20
- VMs and containers inherit VLAN placement via Linux bridges
- Pi-hole on Hogwarts handles internal DNS for all services
- Access to Proxmox UIs restricted to trusted networks
- Tailscale on Hogwarts enables remote access without port forwarding

---

## Backups and Stability

Basic backup and snapshot practices are used across all nodes to:
- Protect critical VMs
- Safely test configuration changes
- Recover from misconfigurations

Snapshots were particularly useful during:
- Windows Server upgrades
- Domain controller changes
- Service migrations

---

## Lessons Learned

Working across three Proxmox nodes provided practical experience with:
- Virtualization fundamentals and resource planning
- LXC container vs VM trade-offs
- Disk passthrough for ZFS-based storage
- Multi-node service distribution and isolation
- Network integration with VLANs and Linux bridges
- Running production-like services in a lab environment
