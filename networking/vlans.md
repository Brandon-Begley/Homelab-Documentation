# VLAN Design

# VLAN Design

This document describes the VLAN segmentation used in my homelab.

The primary goals of implementing VLANs were:
- Separation of trust levels
- Improved security and isolation
- Clear organization of infrastructure
- Gaining hands-on experience with enterprise-style network design

Rather than using a flat network, the homelab is segmented to better reflect real-world environments.

---

## VLAN Overview

| VLAN ID | Name                | Subnet        | Gateway     | Purpose |
|-------:|---------------------|---------------|-------------|---------|
| 10     | Management          | 10.0.10.0/24  | 10.0.10.1   | Network infrastructure and management devices |
| 20     | Homelab / Servers   | 10.0.20.0/24  | 10.0.20.1   | Servers, virtualization, and core services |
| 30     | IoT Devices         | 10.0.30.0/24  | 10.0.30.1   | Smart home and IoT devices |
| 40     | LAN / Trusted       | 10.0.40.0/24  | 10.0.40.1   | User endpoints and trusted clients |

All VLANs are routed through the UniFi Dream Machine Pro, which acts as the default gateway and firewall.

---

## VLAN 10 – Management

**Purpose:**  
The Management VLAN is reserved for network infrastructure and management planes.

**Examples of devices:**
- UniFi Dream Machine Pro
- Switches
- Wireless access points

**Design considerations:**
- Isolated from user devices
- Limited access from other VLANs
- Used only for administrative access

This VLAN exists to reduce the attack surface of critical network infrastructure.

---

## VLAN 20 – Homelab / Servers

**Purpose:**  
This VLAN hosts the core services and lab systems.

**Examples of devices and services:**
- Proxmox VE host
- Docker host (Dockge)
- Plex and related services
- Home Assistant
- Windows Server domain controllers (DC01 / DC02)
- Game servers

**Design considerations:**
- Accessible from the LAN / Trusted VLAN
- Restricted access to Management VLAN
- Acts as the backbone of the homelab

This VLAN is intentionally separated from user devices to limit lateral movement in the event of compromise.

---

## VLAN 30 – IoT Devices

**Purpose:**  
The IoT VLAN is used for smart home and internet-connected devices.

**Examples of devices:**
- Smart plugs
- Smart TVs
- Home Assistant integrations

**Design considerations:**
- Internet access allowed
- Restricted access to other VLANs
- No direct access to Management VLAN

This VLAN reflects a common real-world security practice where IoT devices are treated as untrusted.

---

## VLAN 40 – LAN / Trusted

**Purpose:**  
This VLAN is used for trusted user endpoints.

**Examples of devices:**
- Desktop PCs
- Laptops
- Printers
- NAS access

**Design considerations:**
- Allowed access to Homelab / Servers VLAN
- No direct access to Management VLAN
- Represents normal user activity

This VLAN provides a clean separation between users and backend services.

---

## Inter-VLAN Traffic (High-Level)

- **LAN / Trusted → Homelab / Servers:** Allowed
- **Homelab / Servers → Management:** Restricted
- **IoT → Other VLANs:** Denied (except required services)
- **Management → All VLANs:** Administrative access only

Specific firewall rules are documented separately.

---

## Lessons Learned

Implementing VLAN segmentation provided hands-on experience with:
- Layer 3 routing concepts
- Trust boundaries
- Firewall rule design
- Real-world network organization

This design is intentionally simple but scalable, allowing future expansion without major restructuring.
