# Hardware Inventory

## Protectli VP6670

### Specs
 - i7-1255U CPU
 - 32 GB DDR5 Ram
 - 1 TB Samsung 990 Pro NVME
 - coreboot Bios
 - TPM 2.0

 ### Role
Router for the network being built.

### Choice Rationale
- Provides full control over OS, routing stack, and service composition allowing for a native linux environment to build the router from the ground up.
- i7-1255U and 32GB RAM give enough headroom for VPN, DPI, DNS filtering, and containerized services.
- open firmware stack enabling reduced vendor firmware dependencies and improved boot chain observability.
- TPM 2.0 enables hardware root of trust for artifact verification and future attestation.
- Dual SFP+ plus multiple RJ45 ports give flexible WAN/LAN/VLAN split options.

## Ubiquiti Ecosystem

All switching and wireless hardware was selected from the Ubiquiti Unifi product line for the following reasons that apply across all devices:

- Unified management via a single Unifi controller instance 
  eliminates vendor fragmentation and multiple management interfaces
- Consistent VLAN implementation across the entire stack simplifies 
  trunk configuration between the router, switches, and AP
- SMB-grade hardware used in real production deployments, not 
  consumer or prosumer equipment
- Selected for best capability-to-cost ratio within project constraints

## Ubiquiti Switch Pro Max 16

### Specs
- 16 RJ45 ports supporting 2.5 Gbps speeds
- 2 SFP+ ports supporting 10 Gbps
- 12 PoE+ Ports
- 4 PoE++ Ports
- L2/L3 level switch

### Role
Primary switch to replace current 16 port dumb switch for ethernet wired home.

### Choice Rationale
- Port count matches current switch in operation
- PoE+ ports eliminate need for AP power supply
- PoE++ ports allow for future devices with this power requirement
- Dual SFP+ ports provide 10 Gbps internal backbone capacity
- L3 capability allows future implementation of inter-VLAN routing at the switch layer

## Ubiquiti Switch Flex 2.5G

### Specs
- 8 RJ45 ports supporting up to 2.5 Gbps
- 1 RJ45 uplink supporting up to 10 Gbps and PoE+
- 1 SFP+ uplink supporting up to 10 Gbps
- L2 switch

### Role
Dedicated switch to networking area to support lab devices, Pi's, LLM machine in home lab area of the home

### Choice Rationale
- SFP+ uplink allows for 10 Gbps internal bandwidth to the home lab area
- Compact form factor fits existing home lab area
- Supports downstream dumb switches via daisy chaining for existing hardware reuse

## Ubiquiti Access Point U7 Pro XG

### Specs
- 2.4 GHz, 5 GHz, and 6 GHz bands supported
- Powered via PoE+
- WIFI 7 supported
- 10 Gbps uplink

### Role
Access point for the network

### Choice Rationale
- PoE+ simplifies placement of AP
- Wi-Fi 7 capability provides a future-proof access layer with higher throughput, lower latency, and better MU-MIMO performance for next-generation devices.
- Native support for 2.4 GHz preserves compatibility with IoT

## Raspberry Pi 4B

### Specs
- 8 GB RAM
- 128 GB Micro SD
- Gigabit Ethernet
- USB 3.0 ports

### Role
Multiple infrastructure roles in the home lab, including:
- Git Actions runner
- DMZ reverse proxy
- UniFi controller

Note, Wi-Fi radios are disabled and all 3 are hardwired to minimize attack surface

### Choice Rationale
- Affordable, widely supported hardware for non-production services
- Common platform simplifies deployment, maintenance, and spare parts
- 8 GB RAM provides enough capacity for CI jobs, lightweight proxy workloads, and controller services
- Compact, low-power form factor fits the home lab and keeps operating cost low
- Using identical hardware across multiple roles makes automation, troubleshooting, and potential replacements easier