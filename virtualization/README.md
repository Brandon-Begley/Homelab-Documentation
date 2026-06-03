# Virtualization

This section documents the virtualization layer of the homelab. Proxmox VE runs on three nodes, each with a distinct role.

## Nodes

| Node | Role | Key Services |
|------|------|-------------|
| [Hogwarts](hogwarts.md) | Core homelab services | Plex, Home Assistant, Immich, Homepage, Tailscale, Homelable, Pi-hole |
| [Death-Star](death-star.md) | Monitoring, automation, dev, gaming | Mealie, AMP, Uptime Kuma, Prometheus+Grafana, Coder, n8n |
| [MoServer](../hardware/hp-microserver-gen10.md) | NAS / Storage | TrueNAS VM (ZFS RAID 10) |

## Documents

- [Proxmox VE Overview](proxmox.md) — cluster overview, design principles, networking
- [Hogwarts](hogwarts.md) — core services node
- [Death-Star](death-star.md) — monitoring, automation, and dev tools node
- [TrueNAS](truenas.md) — ZFS-based NAS VM on MoServer
- [Windows Servers](windows-servers.md) — Active Directory domain controllers (hosted on Hogwarts)
