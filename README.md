# Router Lab Documentation

This repository captures the design, architecture, and operational decisions for a home network built to strengthen network-layer expertise through practical implementation.

## Why this exists?
As a principal software engineer I've worked with Kubernetes in a production environment, designed APIs, and managed complex databases. However, through this experience I realized my understanding of network layer of those systems was largely surface level. I recognized my knowledge gap in this area and I am endevoring to close it with this project.

## Project scope
### Hardware
| Device | Role | Why |
|--------|------|-----|
| Protectli VP6670 - 32GB RAM; 1TB NVMe Storage | Router | Supports up to 2Gbps throughput and provides a robust platform for routing, firewalling, and advanced networking services. |
| Ubiquiti Switch Pro Max 16 | Primary switch | Managed PoE switch with SFP+ support for resilient high-bandwidth internal transport. |
| Ubiquiti Switch Flex 2.5G | Access switch | Managed switch with SFP+ uplink support that fits the existing lab layout. |
| Ubiquiti Access Point U7 Pro XG | Access point | Modern Wi-Fi bands with PoE for a clean and reliable deployment. |
| Raspberry Pi 4B 8GB | Git Actions Runner | Allows greater control over the runner, ensures all IaC packages are build oon prm

### VLANs
| ID | Name | Purpose | Trust level |
|----|------|---------|-------------|
| 10 | Home | Personal devices | High |
| 20 | IoT | Smart home devices | Low |
| 30 | Guest | Visitor devices | None |
| 40 | DMZ | Public-facing reverse proxy | Controlled |
| 50 | Services | Internal servers | Medium |
| 60 | Lab | Experimental, breakable | Isolated |
| 100 | Infra | Networking equipment | Administrative |
| 110 | Management | IaC runners & config management | Administrative |
| 999 | Bootstrap | New unprovisioned equipment | Quarantine |

## Documentation
### Architecture Decisions
- Core Network Services Design
- IaC Pipeline Design
- DNS Blocklist Automation
- VLAN Trust Model

### Operational Incidents
- Incidents

## Current Status
- 04/15/2026 - Installed OS on Protectli router

## Naming Convention
The devices on this network are named after celestial bodies in our solar system chosen losely to reflect their role and position in the network heirarchy.

| Device | Body | Rationale |
|--------|------|-----------|
| Router | Sol | The sun, central to all devices on this network |
| DMZ Pi | Ceres | Sits on the DMZ VLAN - the most dangerous area of the network analogous to the astroid belt |
| Infra Pi | Eris | Far on the out skirts of the system as is access to infrastructure directly |
| Git Runner Pi | Dysnomia | Eri's moon and even further removed from the network, mark its boundry |
| Bootstrap VLAN | Oort Cloud | This marks entry into the network and it is the boundry of the solar system |
| LLM Machine | Neptune | Outer planet and isolated |
| Gaming Server | Pluto | Favorite planet in our system |