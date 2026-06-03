# Death-Star — Proxmox Node

Death-Star is the secondary Proxmox node in the homelab, dedicated to development tooling, game server hosting, monitoring infrastructure, and automation services.

---

## Node Overview

| Property | Value |
|----------|-------|
| **Hostname** | Death-Star |
| **Hypervisor** | Proxmox VE |
| **Network VLAN** | VLAN 20 (Homelab / Servers) |
| **Management Access** | Web UI over HTTPS |
| **Primary Role** | Monitoring, automation, dev tools, game servers |

---

## Hosted Services

| Service | Type | Purpose |
|---------|------|---------|
| [Mealie](#mealie) | LXC / VM | Recipe manager and meal planner |
| [AMP](#amp-game-server-hosting) | LXC / VM | Game server management and hosting |
| [Uptime Kuma](#uptime-kuma) | LXC | Service uptime monitoring |
| [Prometheus + Grafana](#prometheus--grafana) | LXC / VM | Metrics collection and dashboards |
| [Coder](#coder) | LXC / VM | Cloud development environments |
| [n8n](#n8n) | LXC / VM | Workflow automation |

---

## Service Details

### Mealie

[Mealie](https://mealie.io) is a self-hosted recipe manager and meal planner. It supports importing recipes from URLs, organizing a personal recipe library, and generating shopping lists.

---

### AMP (Game Server Hosting)

[AMP (Application Management Panel)](https://cubecoders.com/AMP) by CubeCoders provides a web-based interface for managing and hosting game servers. It handles server lifecycle management, player slot configuration, backups, and console access for multiple game server instances.

Supported use cases:
- Spin up and manage game servers (Minecraft, etc.)
- Web UI for starting, stopping, and monitoring game server instances
- Scheduled backups and automated restarts

---

### Uptime Kuma

[Uptime Kuma](https://uptime.kuma.pet) monitors the availability of homelab services and sends alerts when a service goes down.

Key features used:
- HTTP/HTTPS, ping, and port monitoring for all homelab services
- Status page for a consolidated health overview
- Notification alerts for downtime events

---

### Prometheus + Grafana

Prometheus collects time-series metrics from homelab hosts and services. Grafana provides visualization dashboards for those metrics.

Key integrations:
- Node Exporter on Proxmox hosts for CPU, memory, disk, and network metrics
- Dashboards for tracking resource utilization trends across the homelab
- Alerting rules for resource threshold breaches

---

### Coder

[Coder](https://coder.com) provides self-hosted cloud development environments (CDEs). It allows spinning up fully configured development workspaces accessible via browser or local IDE, removing the need to configure local dev environments.

---

### n8n

[n8n](https://n8n.io) is a self-hosted workflow automation tool — similar to Zapier but open-source and running locally. It connects services and automates tasks via a visual node-based editor.

Example workflows:
- Triggering automations based on homelab events
- Connecting Home Assistant with other services
- Scheduled data processing tasks

---

## Networking

- Death-Star management traffic on VLAN 20
- VMs and containers inherit VLAN placement via Linux bridges
- Prometheus scrapes metrics from targets across VLAN 20
- Uptime Kuma monitors services across all Proxmox nodes
