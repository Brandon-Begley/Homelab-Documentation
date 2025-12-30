# Proxmox Virtualization Environment

This document describes how Proxmox VE is used as the core virtualization platform in my homelab.

Proxmox serves as the foundation for hosting:
- Virtual machines
- Linux containers (LXC)
- Self-hosted services (including Plex-related workloads)

The goal of this setup is to gain hands-on experience with enterprise-style virtualization while supporting real services used daily.

---

## Why Proxmox

Proxmox was chosen for the following reasons:
- Native support for both VMs and containers
- Web-based management interface
- Strong networking capabilities
- Snapshot and backup support
- Widely used in homelab and small enterprise environments

Using Proxmox has provided practical experience beyond basic virtualization concepts.

---

## Host Overview

- **Hypervisor:** Proxmox VE
- **Network VLAN:** VLAN 20 (Homelab / Servers)
- **Management Access:** Web UI over HTTPS
- **Primary Roles:** VM hosting, container hosting, service isolation

The Proxmox host is treated as critical infrastructure and is isolated from end-user devices.

---

## Virtual Machine Design

Virtual machines are used for workloads that benefit from:
- Full OS isolation
- Windows-based services
- Domain services

### Windows Server VMs

Proxmox hosts two Windows Server virtual machines:
- **DC01** – Primary Domain Controller
- **DC02** – Secondary Domain Controller

These VMs provide:
- Active Directory
- DNS services
- Redundancy for authentication infrastructure

Running domain controllers in Proxmox provided experience with:
- VM networking
- DNS dependency handling
- Domain controller promotion and demotion
- Troubleshooting replication and role assignment

---

## Linux Containers (LXC)

Linux containers are used for lightweight services where full VM isolation is unnecessary.

Advantages of using LXC:
- Lower resource overhead
- Faster startup times
- Simpler management for single-purpose services

Containers are preferred for:
- Media services
- Supporting infrastructure
- Utility services

---

## Plex and Media Services

Plex and related services are hosted within the Proxmox environment.

Key considerations included:
- Storage mapping
- Permissions and ownership
- Performance tuning
- Service separation

Through hosting Plex in Proxmox, I gained hands-on experience with:
- Mount points and storage passthrough
- UID/GID permission alignment
- Separating media services from core infrastructure
- Troubleshooting container permissions and filesystem access

This portion of the lab represented a significant amount of real-world troubleshooting and learning.

---

## Networking Integration

Proxmox is integrated into the VLAN-based network design.

- Proxmox management traffic resides on VLAN 20
- VMs and containers inherit VLAN placement via bridged networking
- Access is restricted to trusted networks

This reinforced understanding of:
- Linux bridges
- VLAN tagging
- Layer 2 vs Layer 3 responsibilities

---

## Backups and Stability

Basic backup and snapshot practices are used to:
- Protect critical VMs
- Safely test configuration changes
- Recover from misconfigurations

Snapshots were particularly useful during:
- Windows Server upgrades
- Domain controller changes
- Service migrations

---

## Lessons Learned

Working extensively with Proxmox provided practical experience with:
- Virtualization fundamentals
- Resource allocation and performance considerations
- Storage and permission management
- Troubleshooting complex, multi-layered issues
- Running production-like services in a lab environment

Proxmox has become the backbone of the homelab and a key learning platform.
