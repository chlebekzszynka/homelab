# 🏠 Homelab Infrastructure

## The Beginning

My homelab journey started with a Raspberry Pi 5 (8GB RAM - the 16GB version wasn't available yet) that I initially bought as a desktop experiment. It quickly evolved into a server, starting with Pi-hole for network-wide ad blocking. 

The real game-changer for this little powerhouse was discovering [CasaOS](https://github.com/IceWhaleTech/CasaOS). I'm surprised it's not more popular in the homelab community - it provides a gentle introduction to Docker, making container deployment as simple as installing apps on a smartphone. Sure, not every container works perfectly out-of-the-box without manual configuration tweaks, but the learning curve is gentle enough to enjoy self-hosting from day one. 

Currently running 23 containers (everything from Jellyfin to Uptime Kuma and Sonarr) at ~60% RAM utilization, which means it's time to add another Pi 5 - this time with 16GB RAM.

While most homelabs focus on typical self-hosted services, what makes mine unique is the extensive HAM radio and SDR infrastructure - from HackRF One with PortaPack to QRP transceivers and a collection of specialized SDR receivers. This radiocommunication ecosystem will be thoroughly documented in upcoming sections.

The next milestone was building a proper NAS with [FriendlyElec CM3588 NAS Kit](https://www.friendlyelec.com/index.php?route=product/product&product_id=294) running OpenMediaVault, but that's a story for [JOURNEY.md](./JOURNEY.md) - keeping this README focused on technical details and quick references.



Note: This repository is AI-generated and is currently undergoing verification. It may temporarily contain misleading information.

## Technical documentation for multi-server homelab environment.

## Hardware Infrastructure

![Network diagram](./assets/homelab.drawio.svg?=v2)

### 🖥️ Physical Servers

| Device | Model | CPU | RAM | Storage | Network | Role |
|--------|-------|-----|-----|---------|---------|------|
| **Primary Server** | Raspberry Pi | ARM | 8GB | 459GB | 1GbE | Service orchestration, containers |
| **Cold Storage** | Firebat AM02 | Intel N100 (4C/4T) | 16GB DDR4 | 13.7TB (6-drive array) | 2.5GbE | Unraid, archival storage |
| **Fast Storage** | FriendlyElec CM3588 | RK3588 (8C) | 16GB LPDDR4x | 7.27TB ZFS RAID-Z2 | 2.5GbE | OpenMediaVault, NVMe storage |
| **Compute Node** | LattePanda Sigma | Intel i5-1340P (12C/16T) | 16GB LPDDR5 | 953GB/1T | 2.5GbE | AI/ML workloads, [Dragon OS](https://cemaxecuter.com/) SDR hub/ dual boot|
| **GPU Acceleration** | AMD RX 7600 XT | RDNA 3 | 16GB VRAM | - | PCIe 4.0 | LLM inference, image generation |
| **Kubernetes Lab** | Raspberry Pi 4 | ARM | 2GB | SD Card | 1GbE | Kubernetes |
| **Virtualization** | Soyo M4Air | N95 | 16GB DDR4 | 512 GB | 2.5GbE | Proxmox VE |

### Edge Devices

| Device | Quantity | Purpose |
|--------|----------|---------|
| Orange Pi Zero 3 | 2 | ARM compute nodes |
| Orange Pi R2S | 1 | Network appliance |
| Raspberry Pi Zero 2W | 1 | IoT gateway |
| Milk-V Mars | 1 | RISC-V development |


### Network Infrastructure

| Component | Model | Function |
|-----------|-------|----------|
| Router | MikroTik RB5009UG+S+IN | Gateway, firewall, WAN Load Balancing and Failover |
| Managed Switch | Xikestor SKS3200M-8GPY1XF | 8x 2.5G, 1x SFP, 60 GBbps |
| Enterprise Switch | Cisco Catalyst 2960-S | L2 switching |
| Mesh WiFi | TP-Link Deco (M5x2, P9x3, X50-PoE) | Wireless coverage - AP mode|
| UPS | PowerWalker 1500VA | Power protection |


### 📡 Radio Infrastructure

What truly sets this homelab apart from typical setups is the extensive radio and SDR infrastructure. 
Beyond standard IT services, this lab serves as a complete radio experimentation platform - from 
HAM radio operations and SDR research to wireless security analysis.

| Device / Platform                                                    | Type                    | Primary Use Case                                                                                      | Frequency Range   |
| :------------------------------------------------------------------- | :---------------------- | :---------------------------------------------------------------------------------------------------- | :---------------- |
| [HackRF One](https://greatscottgadgets.com/hackrf/one/) + PortaPack | SDR Transceiver         | Versatile, go-to transceiver for field analysis and signal transmission, supercharged by the PortaPack for standalone operation. | 1 MHz - 6 GHz     |
| [SDRplay RSP1A](https://www.sdrplay.com/rsp1a/) | SDR Receiver            | High-fidelity wideband receiver (14-bit ADC) for serious spectrum analysis and monitoring.            | 1 kHz - 2 GHz     |
| [Nooelec NESDR SMArt v5](https://www.nooelec.com/store/sdr/sdr-receivers/smart/nesdr-smart-sdr.html) | SDR Receiver            | Reliable, general-purpose SDR workhorse with a stable TCXO, perfect for daily experiments.            | 25 MHz - 1.75 GHz |
| [Nooelec NESDR SMArTee v2](https://www.nooelec.com/store/sdr/sdr-receivers/smart/nesdr-smartee.html) | SDR Receiver            | SDR receiver featuring an integrated Bias-Tee to power remote amplifiers (LNAs) directly over coax.  | 25 MHz - 1.75 GHz |
| [RTL-SDR (ADS-B)](https://www.rtl-sdr.com/buy-rtl-sdr-dvb-t-dongles/) | SDR Receiver            | Dedicated receiver with a built-in LNA and filter, optimized for tracking aircraft via ADS-B signals.   | 1090 MHz          |
| [Ubertooth One](https://greatscottgadgets.com/ubertoothone/) | Bluetooth Analyzer      | The de facto standard for sniffing and analyzing classic Bluetooth (BR/EDR) traffic.                  | 2.4 GHz           |
| [uSDX](https://www.dl2man.de/) (Clone) | HAM QRP Transceiver     | Ultra-portable, multi-band QRP transceiver for digital mode operations (JS8Call, FT8) from anywhere. | 3.5-30 MHz (HF)   |
| [QRP Labs QDX-M](https://qrp-labs.com/qdx.html) | HAM QRP Transceiver     | Specialized 5W digital-mode-only transceiver, engineered for high-efficiency QRP operations.          | 11 m    |
| [CRT SS 7900 V](https://www.crtfrance.com/en/mobiles-transceivers/993-crt-ss-7900-v-turbo.html) | HAM/CB Transceiver      | Higher-power (up to 60W SSB) transceiver serving as a base station for 10m/11m band operations.          | 28-29.7 MHz       |
| Microcontroller Arsenal - examples | Wi-Fi/BLE Pentesting    | A toolkit of microcontrollers running specialized firmware for hands-on Wi-Fi and BLE security research.  | 2.4 GHz           |
| ↳ [ESP32 Marauder](https://github.com/justcallmekoko/ESP32Marauder) | Wi-Fi/BLE Pentesting    | Portable Wi-Fi and Bluetooth auditing toolkit.                                                        | 2.4 GHz           |
| ↳ [ESP8266 Deauther](https://github.com/SpacehuhnTech/esp8266_deauther) | Wi-Fi Pentesting        | The classic tool for Wi-Fi deauthentication attacks and network analysis.                             | 2.4 GHz           |
| ↳ [nRF52840 Dongle](https://www.nordicsemi.com/Products/Development-hardware/nrf52840-dongle) | BLE Pentesting          | Versatile USB dongle for advanced analysis and interaction with Bluetooth Low Energy (BLE) devices.     | 2.4 GHz           |

## Services Catalog

### Core Infrastructure

| Service | Repository | Port | Platform | Description |
|---------|------------|------|----------|-------------|
| CasaOS | [IceWhaleTech/CasaOS](https://github.com/IceWhaleTech/CasaOS) | 81 | Docker | Container management dashboard |
| OpenMediaVault | [openmediavault](https://github.com/openmediavault/openmediavault) | 80 | Bare metal | NAS operating system |
| Unraid | [Commercial](https://unraid.net) | 80 | Bare metal | Storage array management |
| Proxmox VE | [proxmox](https://www.proxmox.com) | 8006 | Bare metal | Virtualization platform |
| LLM Server | [OLLAMA](https://github.com/ollama/ollama) | 11434 | Bare metal | Local large language models|

### Monitoring & Observability

| Service | Repository | Port | Platform | Description |
|---------|------------|------|----------|-------------|
| Uptime Kuma | [louislam/uptime-kuma](https://github.com/louislam/uptime-kuma) | 3001 | Docker | Service monitoring |
| Netdata | [netdata/netdata](https://github.com/netdata/netdata) | 19999 | Docker | Real-time metrics |
| Beszel | [henrygd/beszel](https://github.com/henrygd/beszel) | 8090 | Docker | System monitoring |
| Glances | [nicolargo/glances](https://github.com/nicolargo/glances) | 61208 | Docker | Cross-platform monitoring |
| MySpeed | [gersta/MySpeed](https://github.com/gersta/MySpeed) | 5216 | Docker | Internet speed tracking |
| OpenSpeedTest | [openspeedtest/Speed-Test](https://github.com/openspeedtest/Speed-Test) | 3004 | Docker | LAN speed testing |

### Network Services

| Service | Repository | Port | Platform | Description |
|---------|------------|------|----------|-------------|
| Pi-hole | [pi-hole/pi-hole](https://github.com/pi-hole/pi-hole) | 80/53 | Bare metal | DNS sinkhole |
| Pi-hole (Proxmox) | [pi-hole/pi-hole](https://github.com/pi-hole/pi-hole) | 80/53 | LXC | Secondary DNS |
| NetBox | [netbox-community/netbox](https://github.com/netbox-community/netbox) | 8000 | Docker | IPAM and DCIM |
| WatchYourLAN | [aceberg/WatchYourLAN](https://github.com/aceberg/WatchYourLAN) | - | Docker | Network device discovery |
| Tailscale | [tailscale/tailscale](https://github.com/tailscale/tailscale) | - | System | Mesh VPN |
| Cloudflared | [cloudflare/cloudflared](https://github.com/cloudflare/cloudflared/) | - | System and Docekr | Cloudflare Tunnel |

### Media Management

| Service | Repository | Port | Platform | Description |
|---------|------------|------|----------|-------------|
| Jellyfin | [jellyfin/jellyfin](https://github.com/jellyfin/jellyfin) | 8096 | Docker | Media server |
| Immich | [immich-app/immich](https://github.com/immich-app/immich) | 2283 | Docker | Photo management |
| Sonarr | [Sonarr/Sonarr](https://github.com/Sonarr/Sonarr) | 8989 | Docker | TV series management |
| Radarr | [Radarr/Radarr](https://github.com/Radarr/Radarr) | 38759 | Docker | Movie management |
| Prowlarr | [Prowlarr/Prowlarr](https://github.com/Prowlarr/Prowlarr) | 9696 | Docker | Indexer manager |
| Jackett | [Jackett/Jackett](https://github.com/Jackett/Jackett) | 9117 | Docker | Torrent proxy |
| Autobrr | [autobrr/autobrr](https://github.com/autobrr/autobrr) | 7474 | Docker | Release automation |
| FlareSolverr | [FlareSolverr/FlareSolverr](https://github.com/FlareSolverr/FlareSolverr) | 8191 | Docker | Cloudflare bypass |

### Download Management

| Service | Repository | Port | Platform | Description |
|---------|------------|------|----------|-------------|
| qBittorrent | [qbittorrent/qBittorrent](https://github.com/qbittorrent/qBittorrent) | 8181 | Docker | BitTorrent client |

### AI/ML Services

| Service | Repository | Port | Platform | Description |
|---------|------------|------|----------|-------------|
| Ollama | [ollama/ollama](https://github.com/ollama/ollama) | 11434 | Bare metal | LLM runtime |
| Open WebUI | [open-webui/open-webui](https://github.com/open-webui/open-webui) | 8081 | Docker | LLM web interface |
| Anse | [anse-app/anse](https://github.com/anse-app/anse) | 8014 | Docker | AI chat interface |

### Productivity

| Service | Repository | Port | Platform | Description |
|---------|------------|------|----------|-------------|
| Trilium | [zadam/trilium](https://github.com/zadam/trilium) | 8089 | Docker | Note-taking |
| Wallabag | [wallabag/wallabag](https://github.com/wallabag/wallabag) | 25661 | Docker | Read-it-later |
| Papra | [papra-hq/papra](https://github.com/papra-hq/papra) | 1221 | Docker | Document management platform |

### Automation & IoT

| Service | Repository | Port | Platform | Description |
|---------|------------|------|----------|-------------|
| Node-RED | [node-red/node-red](https://github.com/node-red/node-red) | 1880 | Bare Metal | Flow-based programming |
| Home Assistant | [home-assistant/core](https://github.com/home-assistant/core) | 8123 | LXC container | Home automation |
| Mosquitto | [eclipse/mosquitto](https://github.com/eclipse/mosquitto) | 1883 | Bare metal | MQTT broker |

### System Services

| Service | Repository | Port | Platform | Description |
|---------|------------|------|----------|-------------|
| Syslog-ng | [syslog-ng/syslog-ng](https://github.com/syslog-ng/syslog-ng) | 6514 | Docker | Log aggregation |
| NUT | [networkupstools/nut](https://github.com/networkupstools/nut) | - | System | UPS management |

### Proxmox Virtual Machines & Containers

| VM/CT | Type | CPU | RAM | Storage | IP | Purpose |
|-------|------|-----|-----|---------|----|---------| 
| 100 | VM | - | - | 46.2GB | - | adguard |
| 101 | VM | - | - | 28.2GB | - | pihole |
| 105 | CT | 2 CPUs | - | 0.6GB | - | qemu |
| 102-104 | - | - | - | - | - | Available slots |

## Storage Architecture

### Storage Tiers

| Tier | Technology | Capacity | Use Case |
|------|------------|----------|----------|
| Fast | ZFS RAID-Z2 (4x NVMe) | 7.27TB | Active data, databases |
| Cold | Unraid (6-drive array) | 13.7TB | Media, archives |


### Storage Distribution

- **Unraid Array**: 5 data drives + 1 parity (4x Toshiba N300 4TB, 2x Toshiba P300 3TB)
- **ZFS Pool**: 4x KIOXIA EXCERIA PRO NVMe SSDs with ~2:1 compression
- **Network Shares**: SMB, NFS, iSCSI

## Network Architecture

### VLANs

| VLAN | Network | Purpose |
|------|---------|---------|
| 1 | Default | Main homelab network |
| 591 | IoT | Device isolation in Deco Wifi |
| 99 | Management | Infrastructure management |

### Network Services

- **DNS**: Adguard (test/primary) Pi-hole (primary), Pi-hole on Proxmox (secondary)
- **DHCP**: MikroTik router
- **VPN**: Tailscale mesh (8 devices)
- **Zero Trust** Claudflare Tunnel
- **Firewall**: MikroTik, planned Sophos UTM

## Monitoring Stack

### Metrics Collection
- Netdata (real-time)
- Glances (system resources)
- Syslog


### Service Monitoring
- Netdata (performance, monitoring)
- Uptime Kuma (availability)
- Beszel (system health)
- MySpeed (internet performance)

### Log Management
- Syslog-ng (aggregation)
- 30-day retention
- 
### Resource Limits

| Service Type | CPU Limit | Memory Limit |
|--------------|-----------|--------------|
| Media | 4 cores | 4GB |
| Monitoring | 2 cores | 2GB |
| Productivity | 1 core | 1GB |
| Network | 1 core | 512MB |

## Backup Strategy

| Data Type | Frequency | Destination | Retention |
|-----------|-----------|-------------|-----------|
| Configuration | Daily | Multiple | 30 days |
| Databases | Daily | ZFS snapshots | 7 days |
| Media | Weekly | Cold storage | Indefinite |
| System | Weekly | External | 4 weeks |

## Power Management

- **UPS**: PowerWalker 1500VA
- **Runtime**: ~20-30 minutes at current load
- **Monitoring**: NUT with automated shutdown servers
- **Protected**: All critical infrastructure


## Access Methods

- **Local**: Direct IP access
- **Remote**: Tailscale VPN only
- **Management**: SSH, IPMI where available
- **Claudflare Tunnel/Zero Trust**
  
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
- Services are accessed via VPN/Zero Trust only

## 🔒 Repository Status

This repository is READ-ONLY.
- No pull requests accepted
- No external contributions
- Documentation purposes only

For questions, open an issue.

---

⭐ **If this project inspired you, leave a star!**



---

*Infrastructure as of Aug 2025*