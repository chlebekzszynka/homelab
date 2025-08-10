# 🏠 Homelab Infrastructure

> My homelab documentation - infrastructure, services, and automation

## 📋 Overview

This homelab consists of various components - some run permanently, others are started on-demand. The main functional element is a CasaOS server, but the infrastructure also includes other devices and services.

## 🖥️ Infrastructure

## 🖥️ Infrastructure

### Multi-Server Setup
- **Main Server**: Raspberry Pi with CasaOS (172.16.16.15)
- **Cold Storage**: Firebat AM02 N100 (172.16.16.140) - 13.7TB Unraid array  
- **Fast Storage**: CM3588 NAS Kit with ZFS (172.16.16.14) - 7.27TB ZFS RAID-Z2
- **AI Compute**: LattePanda Sigma (172.16.16.11) - Intel 13th gen + AMD RX 7600 XT
- **Home Automation**: Raspberry Pi 4 with Home Assistant (IP TBD)
- **Micro Servers**: 2x Orange Pi Zero 3 + Orange Pi R2S + Raspberry Pi Zero 2W + Milk-V Mars
- **Network Security**: Sophos Firewall (planned integration)
- **Network Gateway**: MikroTik RB5009UG+S+IN Router
- **DNS Server**: Pi-hole (integrated with main server)

### Core Hardware Details

#### 🏠 Main Server (CasaOS)
- **Model**: Raspberry Pi with CasaOS (172.16.16.15)
- **CPU**: ARM-based processor (7% avg usage)
- **RAM**: 7GB total (52% usage ~ 3.6GB used)
- **Storage**: ~365GB used / 459GB total (~80% utilization)
- **Role**: Services orchestration, monitoring, automation

#### 🗄️ Cold Storage (Firebat AM02 N100) - **VERIFIED**
- **Platform**: Firebat AM02 Mini PC with Intel Alder Lake-N N100 processor 
- **CPU**: **Intel N100 quad-core CONFIRMED** (4C/4T, no hyperthreading)
  - **Base/Boost**: 700MHz - 3.4GHz (41% current scaling)
  - **Architecture**: x86-64 Gracemont cores (Intel 7 process, 10nm)
  - **TDP**: 6W ultra-low power design
  - **Features**: VMX, AVX2, AVX_VNNI, AES-NI, SHA-NI, HWP
- **RAM**: **15GB DDR4** (2.2GB used, 13GB available, 15% utilization)
- **Platform**: Professional Mini PC chassis optimized for 24/7 operation
- **Array Configuration**: **6-drive mixed array (5 data + 1 parity)**
  - **Data Disks**: 4x Toshiba N300 4TB + 2x Toshiba P300 3TB = 18TB
  - **Parity Disk**: 1x Toshiba N300 4TB (largest drive size)
  - **Usable Capacity**: 13.7TB (efficient parity overhead)
  - **Current Usage**: Disk1 (heavy) + Disk2 (medium) + 2 empty drives
- **Data Distribution**: Movies, Documents, Photos, App data
- **Expansion**: 2 empty drives ready for growth  
- **Health**: All drives 26-27°C, 0 errors, active sync checks
- **Uptime**: 7 weeks, 1 day, 21 hours
- **Role**: Long-term archival, media library, backup target, expandable cold storage

#### ⚡ Fast Storage (CM3588 NAS Kit) - **VERIFIED**
- **Platform**: FriendlyElec CM3588 NAS Kit with Rockchip RK3588 SoC
- **CPU**: RK3588 octa-core **CONFIRMED**
  - **Performance**: 4x Cortex-A76 @ up to 2.35GHz (43% scaling)
  - **Efficiency**: 4x Cortex-A55 @ up to 1.8GHz (100% scaling)
- **GPU**: ARM Mali-G610 MP4 + 6 TOPS NPU
- **Video**: 8K@60fps H.265/VP9 decoder, 8K@30fps H.264 decoder, 4K@60fps AV1 decoder
- **RAM**: **15GB LPDDR4x** (4.3GB used, 11GB available for ZFS ARC)
- **Storage**: **7.27TB ZFS RAID-Z2** (2.82TB actual data with ~2:1 compression)
- **Configuration**: 4x M.2 Key-M 2280 NVMe PCIe 3.0 x1 slots with 4x KIOXIA EXCERIA PRO SSDs
- **Network**: **2.5Gbps Ethernet with Jumbo Frames (MTU 9194)** 🚀
- **Boot**: Separate 57.6GB eMMC for system reliability
- **Thermal**: Excellent cooling (48-49°C across 7 thermal zones)
- **Interfaces**: 2x HDMI OUT, 1x HDMI IN, 2x USB3.0, 1x USB3.0 Type-C with DP
- **OS**: OpenMediaVault on Debian Linux with Docker support
- **Role**: High-speed active storage, VM storage, 4K/8K media processing

#### 🧠 AI Compute Server (LattePanda Sigma) - **VERIFIED**
- **Platform**: LattePanda Sigma x86 Single Board Computer (172.16.16.11/123 + Tailscale)
- **CPU**: **Intel Core i5-1340P (13th Gen Raptor Lake) CONFIRMED**
  - **Configuration**: 12 cores / 16 threads (P+E hybrid architecture)
  - **Base Clock**: 1.9GHz (boost up to 4.6GHz)
  - **TDP**: 44W maintained (efficient cooling design)
- **GPU**: **AMD Radeon RX 7600 XT RDNA 3** 🎮
  - **VRAM**: **16GB GDDR6** (288 GB/s bandwidth) 🚀
  - **Memory Speed**: 2250 MHz effective
  - **Architecture**: Latest RDNA 3 for AI acceleration
  - **Interface**: PCIe 4.0 x16
  - **Driver**: 32.0.21025.1024 (current)
- **RAM**: **16GB Samsung LPDDR5-6400** (8x 2GB modules, ~51.2GB/s bandwidth)
- **Storage**: 953GB main drive (164GB available for AI models)
- **AI Software**: Ollama 0.11.4 installed (ready for LLM deployment)
- **Network**: Multi-interface setup
  - **Homelab VLAN**: 172.16.16.123
  - **Secondary LAN**: 172.16.16.11  
  - **Tailscale VPN**: [Private mesh network]
- **Virtualization**: Hyper-V enabled, WSL2 ready
- **AI Capabilities**: 
  - **LLM Hosting**: **20B-34B parameter models** (Llama 34B, CodeLlama 34B, Mixtral 8x7B)
  - **GPU Acceleration**: AMD ROCm/DirectML with **16GB VRAM**
  - **Image Generation**: Multiple Stable Diffusion models simultaneously
  - **Video AI**: 4K+ processing, upscaling, generation
  - **Multi-modal AI**: Text, image, video, and code generation
  - **Total AI Memory**: **32GB** (16GB VRAM + 16GB RAM)
- **Role**: Local LLM hosting, AI inference, machine learning workloads, GPU-accelerated computing

#### 🏠 Home Automation Hub
- **Model**: Raspberry Pi 4 with Home Assistant (IP TBD)
- **Role**: Smart home control, IoT device management
- **Milk-V Mars**: RISC-V development board (4GB) - open architecture computing
- **Orange Pi Zero 3**: 2 units (ARM-based micro servers)
- **Orange Pi R2S**: 1 unit (potential router/firewall)
- **Raspberry Pi Zero 2W**: 1 unit (lightweight services)
- **Role**: Distributed computing, RISC-V cluster, IoT gateways, architecture comparison

#### 🌐 Network & Security Infrastructure
- **Router**: MikroTik RB5009UG+S+IN
- **Firewall**: Sophos Firewall (planned integration)
- **Features**: Enterprise-grade routing, VLAN support, advanced security
- **UPS**: PowerWalker 1500VA (100% charged, online)

### Network Layout
```
Main LAN:     172.16.16.0/24     (Homelab network segment)
├── 172.16.16.11               (LattePanda Sigma - AI Compute Server)
├── 172.16.16.14               (CM3588 NAS Kit - Fast Storage)
├── 172.16.16.15               (Raspberry Pi CasaOS - Service Hub)
├── 172.16.16.140              (Firebat AM02 N100 - Cold Storage)
├── [IP TBD]                   (Raspberry Pi 4 - Home Assistant)
├── [IPs TBD]                  (Orange Pi Zero 3 x2 - Micro servers)
├── [IP TBD]                   (Orange Pi R2S - Network appliance)
├── [IP TBD]                   (Raspberry Pi Zero 2W - IoT gateway)
├── [IP TBD]                   (Sophos Firewall - Security)
└── Gateway: MikroTik RB5009UG+S+IN

Tailscale:    [Private mesh]        (8-device mesh VPN)
├── [Pi exit node]                (Raspberry Pi - exit node)
├── [Tower exit node]             (Unraid Tower - exit node)
└── 6 other devices               (Windows, Android clients)

IPv6:         [Private IPv6 range]  (Tailscale IPv6)
```

## 🚀 Services

<!-- AUTO-GENERATED-SERVICES-START -->
## 🐳 Container Services (Docker)
| Service | Description | Port | Status | URL | Category |
|---------|-------------|------|--------|-----|----------|
| CasaOS Dashboard | Main management interface | 80 | ✅ | http://172.16.16.15 | 🏠 Core |
| Pi-hole | Network-wide ad blocking & DNS | 80 | ✅ | https://172.16.16.15/admin/ | 🌐 Network |
| Uptime Kuma | Service uptime monitoring | 3001 | ✅ | http://172.16.16.15:3001 | 📊 Monitoring |
| Wallabag | Read-it-later service | 25661 | ✅ | http://172.16.16.15:25661 | 📚 Productivity |
| NetBox | Infrastructure documentation | 8000 | ✅ | http://172.16.16.15:8000 | 🌐 Network |
| Beszel | System monitoring | 8090 | ✅ | http://172.16.16.15:8090 | 📊 Monitoring |
| WatchYourLAN | Network device monitoring | - | ✅ | Internal | 🌐 Network |
| Netdata | Real-time performance monitoring | 19999 | ✅ | http://172.16.16.15:19999 | 📊 Monitoring |
| Syslog-ng | Log aggregation | 6514 | ✅ | Internal | 🔧 System |
| qBittorrent | Torrent client | 8181 | ✅ | http://172.16.16.15:8181 | ⬇️ Downloads |
| Sonarr | TV series management | 8989 | ✅ | http://172.16.16.15:8989 | 🎬 Media |
| FlareSolverr | Captcha solver | 8191 | ✅ | http://172.16.16.15:8191 | 🔧 Utility |
| Jackett | Torrent indexer | 9117 | ✅ | http://172.16.16.15:9117 | ⬇️ Downloads |
| Prowlarr | Indexer manager | 9696 | ✅ | http://172.16.16.15:9696 | ⬇️ Downloads |
| Radarr | Movie management | 38759 | ✅ | http://172.16.16.15:38759 | 🎬 Media |
| Autobrr | Release automation | 7474 | ✅ | http://172.16.16.15:7474 | ⬇️ Downloads |
| Trilium | Note-taking application | 8089 | ✅ | http://172.16.16.15:8089 | 📚 Productivity |
| OpenSpeedTest | Network speed testing | 3004 | ✅ | http://172.16.16.15:3004 | 🌐 Network |
| Anse | AI chat interface | 8014 | ✅ | http://172.16.16.15:8014 | 🤖 AI |
| Glances | System monitoring | 61208 | ✅ | http://172.16.16.15:61208 | 📊 Monitoring |
| Open WebUI | AI interface | 8081 | ✅ | http://172.16.16.15:8081 | 🤖 AI |
| MySpeed | Internet speed monitor | 5216 | ✅ | http://172.16.16.15:5216 | 📊 Monitoring |

## 🖥️ System Services (Native)
| Service | Description | Version | Status | URL | Category |
|---------|-------------|---------|--------|-----|----------|
| Tailscale | Mesh VPN network | 1.86.2 | ✅ | [Private mesh] | 🌐 Network |
| NUT Server | UPS monitoring server | 2.8.0 | ✅ | Internal | 🔌 Power |
| NUT Driver | PowerWalker UPS driver | 2.8.0 | ✅ | Internal | 🔌 Power |
| NUT Monitor | UPS shutdown controller | 2.8.0 | ✅ | Internal | 🔌 Power |
| Node-RED | Visual automation flows | - | ✅ | http://172.16.16.15:1880 | 🏠 Automation |
| Mosquitto | MQTT message broker | - | ✅ | tcp://172.16.16.15:1883 | 🏠 IoT |

## 🗄️ Storage Infrastructure
| Server | Role | IP | Storage | Status | Access |
|--------|------|----|---------| -------|--------|
| Unraid Tower | Cold storage array | 172.16.16.140 | **13.7TB usable** (6-drive: 5 data + 1 parity, 2 empty) | ✅ | Tailscale: [VPN access] |
| OpenMediaVault | Fast ZFS storage | 172.16.16.14 | **7.27TB ZFS RAID-Z2** (2.82TB actual + compression) | ✅ | Local network |
| Raspberry Pi | Service storage | 172.16.16.15 | 459GB | ✅ | Docker volumes |

## 🏠 IoT & Automation Infrastructure
| Device | Role | IP | Platform | Status | Purpose |
|--------|------|----|---------| -------|---------|
| LattePanda Sigma | AI compute server | 172.16.16.11 | Intel i5-1340P + AMD RX 7600 XT | ✅ | Ollama LLM hosting |
| Raspberry Pi 4 | Home Assistant hub | TBD | Home Assistant OS | 🔄 | Smart home control |
| Milk-V Mars | RISC-V cluster node | TBD | 4GB RAM | 🔄 | Open architecture computing |
| Orange Pi Zero 3 #1 | ARM micro server | TBD | Linux | 🔄 | Cluster worker node |
| Orange Pi Zero 3 #2 | ARM micro server | TBD | Linux | 🔄 | Hybrid ARM/RISC-V workloads |
| Orange Pi R2S | Network appliance | TBD | OpenWrt/Linux | 🔄 | Router/firewall/gateway |
| Raspberry Pi Zero 2W | IoT gateway | TBD | Linux | 🔄 | Lightweight IoT services |

## 🌐 Network Infrastructure  
| Component | Model/Type | IP/Access | Role | Status |
|-----------|------------|-----------|------|--------|
| Router | MikroTik RB5009UG+S+IN | Gateway | Advanced routing, VLANs | ✅ |
| Firewall | Sophos Firewall | TBD | Enterprise security, UTM | 🔄 |
| DNS | Pi-hole | https://172.16.16.15/admin/ | Ad blocking, DNS filtering | ✅ |
| VPN | Tailscale | Mesh network | Secure remote access | ✅ |


[🌐 Network diagram ](https://htmlpreview.github.io/?https://github.com/chlebekzszynka/homelab/blob/main/docs/network-diagram.html)


### Total Infrastructure Count
- **🖥️ Servers**: 8 total (4 main + 4 micro servers)
- **🌐 Network**: 2 enterprise appliances + mesh VPN
- **💾 Storage**: 21TB+ across multiple tiers (cold + fast)
- **🧠 AI Computing**: Dedicated Intel 13th gen server for LLM
- **🏠 Smart Home**: Dedicated automation hub
- **🔬 Distributed**: 4 micro servers + RISC-V node for specialized tasks

## ⚡ UPS Status
- **Model**: PowerWalker 1500VA
- **Battery Charge**: 100%
- **Status**: Online (OL)
- **Monitoring**: Full NUT stack with web interface
<!-- AUTO-GENERATED-SERVICES-END -->

## 🔧 Tools & Automation

### CI/CD Pipeline
- **GitHub Actions** - automatic documentation updates
- **Ansible** - configuration and deployment
- **Docker Compose** - container orchestration

### Monitoring
- **Uptime monitoring** - service availability checks
- **Health checks** - automated system status tests
- **Alerts** - problem notifications

## 📁 Repository Structure

```
├── docs/              # Detailed documentation
├── scripts/           # Automation scripts
├── ansible/           # Ansible playbooks
├── docker/            # Docker Compose files
├── monitoring/        # Monitoring configurations
└── .github/workflows/ # GitHub Actions
```

## 🚀 Quick Start

1. **Clone repository:**
   ```bash
   git clone https://github.com/[username]/homelab
   cd homelab
   ```

2. **Update documentation:**
   ```bash
   ./scripts/generate-services-table.sh
   ```

3. **Deploy service:**
   ```bash
   cd docker/compose/[service]
   docker-compose up -d
   ```

## 📊 Infrastructure Stats

### Total Capacity
- **Servers**: 8 physical servers (4 main + 4 micro) + 2 network appliances
- **Storage**: **21TB+ multi-tier storage infrastructure**
  - **⚡ Fast Tier**: 7.27TB ZFS RAID-Z2 NVMe (2.82TB + 2:1 compression)
  - **🗄️ Cold Tier**: 13.7TB Unraid (6-drive: 4x NAS-grade + 2x consumer)
  - **🏠 Service Tier**: 459GB Pi storage (Docker volumes)
- **Computing**: Multi-architecture (ARM + x86 + RISC-V)
  - **🧠 AI Server**: Intel 13th gen with 16GB LPDDR5 for LLM workloads
  - **⚡ ARM Processing**: Rockchip RK3588 with 6 TOPS NPU
  - **🔬 Edge Computing**: N100 + distributed micro servers
- **Services**: 30+ total (Docker + system + storage + planned HA + AI)
- **Network**: Multi-VLAN with dual firewall + mesh VPN overlay
- **Automation**: Dedicated Home Assistant + distributed IoT gateways

### Resource Utilization  
- **Unraid Tower**: 14% RAM, <1% CPU (cold storage efficiency)
- **Raspberry Pi**: 52% RAM, 7% CPU (active service hub)
- **Storage Tiers**: Cold (13.7TB) + Fast (SSD) + Service (459GB)
- **Network**: Enterprise routing + security + mesh connectivity
- **Distributed Computing**: 4 micro servers for specialized workloads

### 🏷️ Service Distribution
- 🏠 **Core & Automation**: 4 services (CasaOS, Node-RED, MQTT, Pi-hole)
- 📊 **Monitoring Stack**: 6 services (comprehensive observability)
- 🎬 **Media Management**: 2 services + Unraid backend storage
- ⬇️ **Download Automation**: 5 services (complete *arr ecosystem)
- 📚 **Productivity**: 2 services (knowledge management) 
- 🤖 **AI Interfaces**: 2 services (LLM integration)
- 🌐 **Network Tools**: 5 services (infrastructure management)
- 🔌 **Power Management**: 3 NUT services + UPS monitoring
- 🗄️ **Storage Services**: 2 dedicated NAS systems

## 🎯 Roadmap

### Current Capabilities ✅
- [x] Multi-server infrastructure (8 servers)
- [x] 21TB+ storage capacity with redundancy
- [x] Comprehensive monitoring stack (6 tools)
- [x] Complete media automation pipeline
- [x] Enterprise networking (MikroTik + VLANs)
- [x] Mesh VPN with exit nodes
- [x] UPS backup power protection
- [x] DNS filtering and ad blocking
- [x] AI/LLM integration with 16GB VRAM

### Future Enhancements 🚀
- [ ] Automated infrastructure backups
- [ ] RISC-V cluster deployment (Milk-V Mars + K3s)
- [ ] ARM vs RISC-V performance benchmarking
- [ ] Kubernetes migration for container orchestration
- [ ] High availability clustering
- [ ] Security scanning automation
- [ ] Infrastructure as Code (Terraform/Ansible)
- [ ] Disaster recovery procedures
- [ ] Performance optimization

## 📚 Documentation

- [Infrastructure Overview](docs/infrastructure.md)
- [Services Documentation](docs/services.md)
- [Automation Guide](docs/automation.md)

## 🤝 Inspiration

This homelab was inspired by:
- [mischavandenburg/homelab](https://github.com/mischavandenburg/homelab)
- [awesome-selfhosted](https://github.com/awesome-selfhosted/awesome-selfhosted)


## ⚠️ Disclaimer

This is a personal homelab documentation for reference purposes.
- No pull requests accepted
- Configuration examples may be simplified
- Always adapt security settings to your needs

## 🔒 Security Note

- All IP addresses are from private RFC1918 ranges
- No public endpoints are exposed
- Services are accessed via VPN only

## 🔒 Repository Status

This repository is READ-ONLY.
- No pull requests accepted
- No external contributions
- Documentation purposes only

For questions, open an issue.

---

⭐ **If this project inspired you, leave a star!**