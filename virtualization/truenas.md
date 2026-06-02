# TrueNAS

TrueNAS runs as a virtual machine inside Proxmox on the HP MicroServer Gen10. It provides centralized NAS services for the homelab using ZFS for storage management.

---

## Why TrueNAS

TrueNAS was chosen as the NAS operating system for the following reasons:

- Built on ZFS — a proven, enterprise-grade filesystem with data integrity guarantees
- Web-based management interface for easy administration
- Built-in support for SMB, NFS, and iSCSI shares
- Snapshot and replication support for data protection
- Active open-source community with strong documentation

Running TrueNAS as a VM under Proxmox (with disk passthrough) provides the best of both worlds: hypervisor flexibility and ZFS's direct hardware access requirements.

---

## VM Configuration

The TrueNAS VM is configured in Proxmox with disk passthrough to give ZFS direct access to the physical drives.

| Setting | Value |
|---------|-------|
| **Hypervisor** | Proxmox VE (HP MicroServer Gen10) |
| **Disk Access** | Physical disk passthrough (by-id) |
| **Network** | Bridged — VLAN 20 (Homelab / Servers) |

> **Note:** Disk passthrough is essential for ZFS. Virtualizing disks through a hypervisor layer prevents ZFS from accurately reading S.M.A.R.T. data, monitoring disk health, and managing its write cache safely.

---

## ZFS Pool Layout — Mirror vdevs (RAID 10 Equivalent)

TrueNAS uses ZFS for all storage. The pool is configured using **mirrored vdevs**, which is ZFS's equivalent of RAID 10.

### What is RAID 10 in ZFS terms?

In ZFS, RAID 10 is not a single construct but is achieved by combining multiple **mirror vdevs** into a single pool:

- Each **mirror vdev** = two drives mirrored (equivalent to RAID 1)
- Striping across multiple mirror vdevs = reads/writes distributed across mirrors (equivalent to RAID 0 across the mirrors)
- The result: **RAID 10 performance and redundancy**

```
Pool: tank
├── mirror-0
│   ├── Drive 1
│   └── Drive 2
└── mirror-1
    ├── Drive 3
    └── Drive 4
```

### Why Mirror vdevs (RAID 10)?

| Benefit | Details |
|---------|---------|
| **Redundancy** | Each mirror can lose one drive and keep running |
| **Performance** | Reads are distributed across both mirrors; writes go to both drives in each mirror |
| **Recovery speed** | Resilver (rebuild) time is faster than RAID-Z equivalents on large drives |
| **Simplicity** | Straightforward fault tolerance — lose any single drive, pool stays online |

### Trade-offs

- **50% usable capacity** — half of raw disk space is used for redundancy (same as RAID 10)
- Losing both drives in a single mirror vdev results in pool loss — drives should be from different vendors/batches to reduce simultaneous failure risk

---

## Shares and Services

TrueNAS is used to serve storage to other homelab systems:

- **SMB** — Windows file shares for DC01/DC02 and general network storage
- **NFS** — Linux/Proxmox storage (optional ISO/backup storage)

---

## Data Protection

| Feature | Status |
|---------|--------|
| ZFS checksums | Enabled (always on in ZFS) |
| Periodic scrubs | Scheduled — detects and corrects silent data corruption |
| Snapshots | Configured for key datasets |

ZFS scrubs are scheduled regularly to verify data integrity across all drives in the pool.

---

## Lessons Learned

- Disk passthrough setup in Proxmox requires using persistent `/dev/disk/by-id/` paths to avoid drive remapping after reboots
- Mirror vdevs offer faster resilver times compared to RAID-Z on large drives, making them a better fit for this use case
- ZFS ARC (Adaptive Replacement Cache) memory usage should be accounted for when sizing the TrueNAS VM RAM allocation
- TrueNAS should be the **only** system writing to the physical disks — never mount ZFS drives directly on the Proxmox host while TrueNAS is running
