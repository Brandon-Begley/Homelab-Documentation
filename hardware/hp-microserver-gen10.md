# HP ProLiant MicroServer Gen10

The HP ProLiant MicroServer Gen10 serves as a dedicated NAS/storage server in the homelab, running Proxmox VE with a TrueNAS virtual machine for centralized storage.

---

## Hardware Overview

| Component | Details |
|-----------|---------|
| **Hostname** | MoServer |
| **Model** | HP ProLiant MicroServer Gen10 |
| **Form Factor** | Tower (ultra-compact) |
| **Drive Bays** | 4x non-hot-plug LFF (3.5") |
| **Hypervisor** | Proxmox VE |
| **Primary Role** | NAS / Centralized Storage |

---

## Role in Homelab

The MicroServer Gen10 is dedicated to storage duties:

- Hosts a TrueNAS virtual machine for centralized NAS services
- Provides shared storage accessible by other hosts and services
- Offloads storage workloads from the primary Proxmox node

---

## Proxmox Configuration

Proxmox VE is installed bare-metal on the MicroServer Gen10 to enable:

- Running TrueNAS as a VM with direct disk passthrough
- Hardware-level isolation between the hypervisor and storage OS
- Future flexibility to add additional VMs or containers if needed

Disk passthrough is used so TrueNAS has direct, unmediated access to the physical drives. This is the recommended approach for ZFS-based storage systems, as ZFS requires direct control of the underlying disks for proper health monitoring, caching, and error correction.

---

## Storage Design

The physical drives are passed through directly to the TrueNAS VM. See [TrueNAS documentation](../virtualization/truenas.md) for pool layout and ZFS configuration details.

---

## Lessons Learned

- Proxmox disk passthrough requires passing the full disk device (e.g., `/dev/disk/by-id/`) rather than a partition, ensuring ZFS can manage disk identity correctly
- The Gen10's compact form factor makes it ideal as a dedicated NAS appliance
- Separating storage duties onto dedicated hardware reduces contention on the primary virtualization host
