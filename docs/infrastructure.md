# 🏗️ Infrastructure Overview

## 🌐 Network Architecture

### Internet Connectivity
- **Primary Link**: Fiber optic connection #1
- **Secondary Link**: Fiber optic connection #2 (redundancy)
- **Backup Link**: Mobile router (4G/5G failover)
- **Configuration**: Multi-WAN with automatic failover

### Core Network Equipment

#### 🔧 Router
- **Model**: MikroTik RB5009UG+S+IN
- **Role**: Main gateway, VLAN routing, firewall
- **Features**: 
  - Dual WAN load balancing/failover
  - Advanced VLAN management
  - Enterprise-grade routing
  - QoS and traffic shaping

#### 🔀 Switches

**Primary Homelab Switch**
- **Model**: TP-Link SKS3200M-8GPY1XF
- **Type**: Managed PoE+ switch
- **Ports**: 8x Gigabit PoE+ ports + 1x SFP
- **Role**: Main homelab infrastructure
- **Connected Devices**:
  - Raspberry Pi CasaOS server
  - Firebat AM02 N100 (Unraid)
  - CM3588 NAS Kit
  - LattePanda Sigma
  - Deco X50-PoE AP

**Secondary Switch**
- **Model**: Cisco Catalyst 2960-S
- **Type**: Enterprise managed switch
- **Role**: Extended connectivity, VLAN support
- **Features**: Layer 2 switching, VLAN tagging, QoS

#### 📡 Wireless Infrastructure

**TP-Link Deco Mesh System (AP Mode)**
- **Deco M5**: 2 units (AC1300 dual-band)
- **Deco P9**: 3 units (AC1200 + Powerline)
- **Deco X50-PoE**: 1 unit (Wi-Fi 6, PoE powered)
- **Configuration**: AP mode with controller-based management
- **Coverage**: Whole-home mesh network
- **VLANs**: 
  - Main network (untagged)
  - IoT VLAN (isolated)

### Network Layout Diagram

```
┌──────────────────────────────────────────────────────────────────┐
│                         INTERNET                                   │
│     Fiber #1 ←──→ Fiber #2 ←──→ Mobile Router (Backup)           │
└─────────┬──────────────┬──────────────────┬──────────────────────┘
          │              │                  │
          └──────────────┴──────────────────┘
                         │
                    ┌────▼────┐
                    │ MikroTik │ Gateway: 172.16.16.1
                    │ RB5009   │ (Multi-WAN, VLANs, Firewall)
                    └────┬────┘
                         │ Trunk (VLANs: Main, IoT, Management)
                    ┌────▼────────────────┐
                    │ TP-Link SKS3200M    │ PoE+ Managed Switch
                    │ 8GPY1XF (PoE+)      │
                    └──┬──┬──┬──┬──┬──┬──┘
                       │  │  │  │  │  │
     ┌─────────────────┘  │  │  │  │  └───────────────┐
     │                    │  │  │  │                  │
┌────▼────┐         ┌────▼──▼──▼──▼────┐       ┌────▼────┐
│ Pi       │         │ Cisco Catalyst   │       │ Deco    │
│ CasaOS   │         │ 2960-S           │       │ X50-PoE │
│ .15      │         └──┬──┬──┬──┬──┬──┘       │ (Wi-Fi) │
└─────────┘             │  │  │  │  │          └─────────┘
                        │  │  │  │  │
┌─────────┐      ┌─────▼┐ │  │  │  └────┐     ┌─────────┐
│ Unraid  │      │Deco  │ │  │  │       │     │ Orange  │
│ N100    │      │M5 #1 │ │  │  │   ┌───▼──┐  │ Pi      │
│ .140    │      └──────┘ │  │  │   │Deco  │  │ Cluster │
└─────────┘                │  │  │   │P9 #3 │  └─────────┘
                    ┌──────▼┐ │  │   └──────┘
┌─────────┐         │Deco   │ │  │             ┌─────────┐
│ CM3588  │         │M5 #2  │ │  └────┐        │ Home    │
│ NAS Kit │         └───────┘ │       │        │Assistant│
│ .14     │                   │   ┌───▼──┐     │ Pi 4    │
└─────────┘            ┌──────▼┐  │Deco  │     └─────────┘
                       │Deco   │  │P9 #2 │
┌─────────┐            │P9 #1  │  └──────┘     ┌─────────┐
│LattePanda│           └───────┘                │ Sophos  │
│ Sigma   │                                     │Firewall │
│ .11     │                                     │(planned)│
└─────────┘                                     └─────────┘
```

### VLAN Configuration

| VLAN ID | Name | Network | Purpose | Tagged Ports |
|---------|------|---------|---------|--------------|
| 1 | Default | 172.16.16.0/24 | Main homelab network | All ports (untagged) |
| 10 | IoT | 172.16.10.0/24 | IoT devices isolation | Deco APs, specific ports |
| 99 | Management | 172.16.99.0/24 | Switch/AP management | Management ports |

### Network Segments Detail

```
Main Network (VLAN 1): 172.16.16.0/24
├── Gateway:        172.16.16.1    (MikroTik Router)
├── Servers:
│   ├── 172.16.16.11              (LattePanda Sigma - AI)
│   ├── 172.16.16.14              (CM3588 NAS - Fast Storage)
│   ├── 172.16.16.15              (Raspberry Pi - CasaOS)
│   └── 172.16.16.140             (Firebat N100 - Unraid)
├── Infrastructure:
│   ├── 172.16.16.2               (SKS3200M Switch)
│   ├── 172.16.16.3               (Catalyst 2960-S)
│   └── 172.16.16.4-9             (Deco APs)
└── DHCP Pool:      172.16.16.100-254

IoT Network (VLAN 10): 172.16.10.0/24
├── Gateway:        172.16.10.1    (MikroTik Router)
├── Isolated from main network
├── Internet access only
└── DHCP Pool:      172.16.10.100-254

Management (VLAN 99): 172.16.99.0/24
├── Gateway:        172.16.99.1    (MikroTik Router)
├── Switch mgmt:    172.16.99.2-10
└── Limited access (admin only)

Tailscale Mesh: 100.x.x.x/8
├── Exit nodes:     Pi + Unraid
├── Remote access to all services
└── 8 devices total
```

## 🖥️ Physical Hardware

### Main Servers

#### 🏠 Main Server (CasaOS)
- **Model**: Raspberry Pi with CasaOS
- **IP Address**: 172.16.16.15
- **CPU**: ARM-based processor (7% avg usage)
- **RAM**: 7GB total (52% usage ~ 3.6GB used)
- **Storage**: ~365GB used / 459GB total (~80% utilization)
- **Connection**: SKS3200M switch (Gigabit)
- **Role**: Services orchestration, monitoring, automation

#### 🗄️ Cold Storage Server (Unraid)
- **Model**: Firebat AM02 Mini PC
- **IP Address**: 172.16.16.140
- **CPU**: Intel N100 quad-core (Alder Lake-N)
- **RAM**: 15GB DDR4 (15% utilization)
- **Storage**: 13.7TB usable (6-drive array)
- **Connection**: SKS3200M switch (Gigabit)
- **Role**: Long-term archival, media library

#### ⚡ Fast Storage Server (OpenMediaVault)
- **Model**: FriendlyElec CM3588 NAS Kit
- **IP Address**: 172.16.16.14
- **CPU**: Rockchip RK3588 octa-core
- **RAM**: 15GB LPDDR4x
- **Storage**: 7.27TB ZFS RAID-Z2 (4x NVMe)
- **Network**: 2.5Gbps Ethernet with Jumbo Frames
- **Connection**: SKS3200M switch (2.5GbE)
- **Role**: High-speed active storage, VM storage

#### 🧠 AI Compute Server
- **Model**: LattePanda Sigma
- **IP Address**: 172.16.16.11 (+ .123 secondary)
- **CPU**: Intel Core i5-1340P (13th Gen)
- **GPU**: AMD Radeon RX 7600 XT (16GB VRAM)
- **RAM**: 16GB LPDDR5-6400
- **Connection**: SKS3200M switch (Gigabit)
- **Role**: LLM hosting, AI inference, GPU computing

#### 🏠 Home Automation Hub
- **Model**: Raspberry Pi 4
- **IP Address**: TBD (DHCP reservation planned)
- **RAM**: 4GB/8GB
- **OS**: Home Assistant OS
- **Connection**: Cisco Catalyst 2960-S
- **Role**: Smart home control, automation

### Micro Servers & Edge Devices
- **Orange Pi Zero 3**: 2 units (ARM micro servers)
- **Orange Pi R2S**: 1 unit (potential router/firewall)
- **Raspberry Pi Zero 2W**: 1 unit (IoT gateway)
- **Milk-V Mars**: RISC-V board (4GB RAM)
- **Connection**: Cisco Catalyst 2960-S
- **Role**: Distributed computing, IoT gateways

## 🔌 Power & Environment

### Power Infrastructure
- **UPS Model**: PowerWalker 1500VA
- **Battery Status**: 100% charged
- **Mode**: Online (OL)
- **Runtime**: ~20-30 minutes at current load
- **Monitoring**: NUT server with web interface
- **Protected Devices**:
  - MikroTik router
  - SKS3200M switch
  - All main servers
  - Critical Deco APs

### Power Consumption Estimates
- **Router**: ~10W
- **SKS3200M Switch**: ~75W (with PoE devices)
- **Catalyst 2960-S**: ~30W
- **Servers Combined**: ~150W
- **Deco APs**: ~50W total
- **Total Estimated**: ~315W continuous

### Environmental Monitoring
- **Server Temperatures**: 26-49°C (monitored)
- **Room Temperature**: Ambient monitoring via HA
- **Cooling**: Natural convection + case fans

## 🔐 Security Architecture

### Network Security Layers
1. **Perimeter Security**
   - MikroTik firewall rules
   - Sophos Firewall (planned UTM)
   - DDoS protection
   - Port security

2. **Network Segmentation**
   - VLAN isolation (Main/IoT/Management)
   - Inter-VLAN routing control
   - Guest network isolation

3. **Access Control**
   - Tailscale mesh VPN for remote access
   - No direct port forwarding
   - Certificate-based authentication
   - 2FA where supported

4. **DNS Security**
   - Pi-hole for filtering
   - DNSSEC validation
   - Malware blocking lists

### Physical Security
- Equipment in secured location
- UPS battery backup
- Environmental monitoring

## 📊 Monitoring & Management

### Network Monitoring
- **Uptime Kuma**: Service availability
- **WatchYourLAN**: Device discovery
- **Netdata**: Real-time metrics
- **MikroTik Tools**: Traffic analysis

### Infrastructure Management
- **NetBox**: Documentation and IPAM
- **Beszel**: System monitoring
- **Glances**: Resource monitoring
- **SNMP**: Switch/router monitoring

### Log Management
- **Syslog-ng**: Centralized logging
- **Log retention**: 30 days
- **Alert routing**: Critical events

## 🔄 Redundancy & Failover

### Internet Redundancy
- **Primary**: Fiber optic link #1
- **Secondary**: Fiber optic link #2
- **Tertiary**: Mobile router (4G/5G)
- **Failover**: Automatic with health checks

### Storage Redundancy
- **Unraid**: Single parity protection
- **ZFS**: RAID-Z2 double parity
- **Backups**: Local + cloud (planned)

### Power Redundancy
- **UPS**: 20-30 minute runtime
- **Auto-shutdown**: NUT-managed graceful shutdown

---

*Last updated: December 2024*