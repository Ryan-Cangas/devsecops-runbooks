# 🛡️ FortiGate Firewall Integration with Wazuh SIEM

## 📖 [View My Detailed Implementation Notes Diary](/FortiGate%20Firewall%20Integration%20in%20Wazuh.md)

[![Wazuh Version](https://img.shields.io/badge/Wazuh-v4.x-blue.svg)](https://wazuh.com/)
[![FortiOS Version](https://img.shields.io/badge/FortiOS-v7.6+-orange.svg)](https://www.fortinet.com/)
[![Platform](https://img.shields.io/badge/Environment-Proxmox%20%7C%20Docker-green.svg)](#)

Welcome to my **FortiGate NGFW & Wazuh SIEM Integration** project! This repo contains the configuration assets, architectural layouts, and pipeline implementations used to integrate enterprise-tier network virtualization with centralized log analysis and automated threat detection.

By feeding real-time network telemetry from a virtualized **FortiGate Next-Generation Firewall (NGFW)** into a containerized **Wazuh SIEM/SOC cluster**, this architecture captures, decodes, and indexes active security events across the entire local network.

---

## 🎯 Project Overview & Intent

The objective of this project is to demonstrate how to safely route connectionless UDP syslog streams out of an isolated hypervisor layer and securely deliver them into a containerized SIEM engine without breaking network address translation (NAT) or dropping active packets.

### Key Solutions Solved in this Deployment:

- **Real-Time Visibility:** Transforms raw, obfuscated firewall events into structured, human-readable security alarms.
- **Unified SOC View:** Monitors network-edge telemetry (IDS/IPS, firewall blocks, UTM events) right alongside host-level endpoint data.
- **Architecture Proof-of-Concept:** Handles unprivileged host-to-container port mapping, safely bypassing native Linux restrictions on root network bindings (<1023) via custom NAT proxies.

---

## 🏗️ Technical Pipeline & Packet Journey

Below is the high-level data pathway as an event moves from the firewall edge into your analytics index pattern:

```text
  [ FortiGate KVM Appliance ]
         (192.168.1.xxx)
                │
                │  1. Generates UTM Security Event Log
                │  2. Fires connectionless UDP packet to Host Edge
                ▼
   [ Proxmox Linux Bridge (vmbr0) ]
                │
                │  3. Standard Layer 2 Frame Switching
                ▼
   [ Ubuntu Host VM (Jumpbox Edge) ] ── (Listens on vacant Host Port 5140)
                │
                │  4. Catching traffic outside privileged port space
                ▼
    [ Docker NAT Proxy Daemon ]
                │
                │  5. Remaps destination socket: Host:5140 ──> Container:514
                ▼
   [ Wazuh Manager Core Daemon ] ──── (allowed-ips wildcard: 0.0.0.0/0)
                │
                │  6. Validates, decodes with "fortigate-firewall-v6" rules
                ▼
     [ Indexed Security Alerts ]
```

---

## 📂 Repository Breakdown

- **`FortiGate Firewall Integration in Wazuh.md`** - The primary engineering notebook documenting initial provisioning steps, CLI configuration scripts, and pipeline troubleshooting.
- **`/Images/`** - Topology maps, physical system architecture layouts, and dashboard analytical outputs verifying alert ingestion.

---

## 🚀 Key Implementation Highlights

- **NAT Ingress Architecture:** Proxies IANA-registered well-known ports (Syslog UDP 514) down into Docker application spaces using vacant host-side unprivileged ports (5140).
- **Wildcard IP Allocation:** Solves Docker's Source NAT (SNAT) masking behavior within the engine configurations (`ossec.conf`), allowing the container to cleanly ingest logs from external local subnets.
- **Automated KVM Provisioning:** Built entirely on customized Proxmox storage logic scripts (`qm importdisk`), prioritizing speed and storage performance by utilizing low-overhead `virtio-scsi` controllers.
- **UTM Telemetry Auditing:** Features end-to-end event validation using integrated FortiOS diagnostic log tests to simulate real threat matrices (Viruses, Web Filtering, IPS hits).

---

## 📊 Telemetry Verification

Once the pipeline is active, all ingested firewall data automatically feeds through the native `fortigate-firewall-v6` decoder rule mapping. Inbound log strings are parsed, categorized, and indexed within the **Wazuh Security Events Dashboard** for live analytical cross-referencing and incident investigation.
