# Virtualization

This section documents the virtualization layer of the homelab.

Technologies covered:
- Proxmox VE
- Virtual Machines
- Resource allocation
- High-level design decisions

## Hosts

| Host | Hardware | Role |
|------|----------|------|
| Primary | (primary Proxmox node) | VMs, containers, services |
| MicroServer | [HP MicroServer Gen10](../hardware/hp-microserver-gen10.md) | NAS / Storage |

## Documents

- [Proxmox VE](proxmox.md) — core virtualization platform
- [TrueNAS](truenas.md) — ZFS-based NAS VM on the MicroServer Gen10
- [Windows Servers](windows-servers.md) — Active Directory domain controllers
