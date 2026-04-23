# Future Projects

## Overview
This is a list of projects and ideas that should be pursued in the future once the network is build out.

## To-Do List (unordered)
- [ ] Use chrony to set up a time server on Sol to be the network NTP
- [ ] Use a pi or switch to be an IP helper and try doing DHCP on UDP through port 67 instead of handling all un-leased traffic directly
- [ ] Use network boot options in Kea to allow for network booting of a pi
- [ ] Transition kea from a memfile to a postgresql db for lease management
- [ ] Automate git runner pi with separate pipeline
- [ ] Automate Unifi pi with separate pipeline
- [ ] Automate DMZ Proxy Pi
- [ ] Validation script ensuring that deployment functions properly
- [ ] Create state management and tracking router deployment hashes
- [ ] Install Merlin on existing router and use it on the lab network
- [ ] Kids VLAN with DNS-based content filtering and DoH blocking
- [ ] Internal CA with certificate management for all internal services
- [ ] WireGuard VPN self-hosted (you mentioned Linode or similar)
- [ ] Tailscale for remote access
- [ ] Grafana/Prometheus/Loki observability stack
- [ ] Honeypot on DMZ VLAN
- [ ] Vulnerability scanning from Lab VLAN against other VLANs
- [ ] DNS over TLS on Unbound for upstream queries
- [ ] VLAN breakout testing - deliberately attempt to break segmentation
- [ ] Squid or DNS sinkhole for ad blocking (you mentioned this early on)
- [ ] DPI via security tooling at 2Gbps
- [ ] RPZ blocklist automation from Pi-hole sources
- [ ] DNSSEC validation on Unbound
- [ ] Rotate MongoDB credentials on Eris - exposed in system.properties output
- [ ] Backup strategy for Sol configs and lease database
- [ ] Alerting rules in Grafana for network events
- [ ] mDNS reflector for IoT VLAN Bonjour support
- [ ] Proxy config on DMZ Pi for public facing services
- [ ] TPM measured boot - seal disk encryption key to PCR values
- [ ] Quarterly security review runbook implementation
- [ ] Backup and disaster recovery testing - run full bootstrap from scratch to verify runbook
- [ ] Enable and configure IPv6 across all VLANs
- [ ] Migrate Route 53 dynamic DNS script from Pluto to Ceres
- [ ] Update script to handle both IPv4 and IPv6 public addresses
- [ ] Build custom network dashboard for non-technical household visibility
    - Short term: Cockpit for basic server status
    - Long term: Grafana dashboards with network-specific views
    - Requirements: read only, device friendly names, WAN status, 
      basic bandwidth, no ability to change settings
- [ ] Internal service proxy via HAProxy on Ceres
    - Friendly URLs for all internal services
    - No port numbers visible to end users
    - SSL termination at HAProxy
    - Requires internal CA or Lets Encrypt DNS challenge (see CA project)
- [ ] Internal Kea lease management API in Go
    - Query active leases by VLAN, MAC, hostname
    - Release leases manually
    - View lease history and renewals
    - REST API with authentication
    - Grafana integration for visualization
    - Requires: Postgres lease backend migration first
--Route 53 script considerations
- [ ] TPM 2.0 SPI module for Ceres
- [ ] Encrypt AWS credentials at rest using TPM-backed key
- [ ] Docker entrypoint decrypt pattern for sensitive configs
- [ ] Separate IAM credential per service on Ceres
    - Route53 updater gets its own credential
    - Any future services get their own credential
    - No shared credentials between services
- [ ] AppArmor profiles for Docker containers on Ceres
- [ ] Read-only container filesystems where possible

## Completed List (ordered by date desc)