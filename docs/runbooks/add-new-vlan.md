# Adding a new VLAN to the network

NOTE: This is not the completed file, jotting down notes here to eventually writing the flushed out version of this runbook

NOTE: All deployments are handled by the IaC pipeline so any restarting or reloading of services will be omitted

## Steps

These steps are in deployment order!

### Network Interfaces
1. Add the netdev file for the VLAN - ex. `30-vlan100.netdev`.
```
#/etc/systemd/network/30-vlan100.network
[NetDev]
Name=vlan100
Kind=vlan

[VLAN]
Id=100
```
2. Update the existing .network file for the interface you are adding the VLAN to, add it under `[Network]` before any other configs/settings. Remove the subnet defined in the file as well.
```
BEFORE
#/etc/systemd/network/20-lan.network
[Match]
Name=<interface>

[Network]
Address=192.168.1.1/24
LinkLocalAddressing=no
IPv6AcceptRA=no
ConfigureWithoutCarrier=yes
```
```
AFTER
#/etc/systemd/network/20-lan.network
[Match]
Name=<interface>

[Network]
VLAN=vlan100
LinkLocalAddressing=no
IPv6AcceptRA=no
ConfigureWithoutCarrier=yes
```
3. Create a .network file for the VLAN that contains the name and the address which is the subnet for the VLAN.
```
[Match]
Name=vlan100

[Network]
Address=192.168.1.1/24
```
5. Add the new VLAN IP to listen-on in named.conf
### DHCP
1. Add the new VLAN to the interfaces-config in the kea config file.
2. Add a subnet and subsequent configs within the subnet for the VLAN where the interface is the name of the VLAN.
3. Add the VLAN subnet to kea-dhcp-ddns.conf
   - Add forward zone entry for the VLAN domain
   - Add reverse zone entry for each /24 in the subnet
4. Add reservations for names and/or IP addresses to the reservation JSON that will update with the Postgresql db.

### NFTables
1. Configure any rules for the given VLAN including inbound traffic, outbound traffic, forwarded traffic, and inter-VLAN communication.

### DNS Internal
1. Add the blocks in `named.conf.local` for the zone and reverse zone(s) for this VLAN.
    - There could be multiple reverse zones IF the VLAN has a greater address pool than /24, each /24 pool gets their own reverse zone, so a /23 subnet would have 2 reverse zones.
2. Create the zone file for the VLAN
3. Create the reverse zone file(s) for the subnet
4. Add the VLAN domain to resolved.conf Domains= line on Sol
5. Add allow-update to both zone declarations in named.conf.local
6. Update listen-on in named.conf with Sol's IP on new VLAN
7. Update allow-query in named.conf with new VLAN subnet
8. Update allow-recursion in named.conf with new VLAN subnet

### Unifi Controller
1. Create new VLAN network in Unifi controller
2. Assign VLAN ID matching your scheme
3. Configure port profiles for switches
4. Assign SSID to VLAN if wireless access needed
5. Verify tagged traffic on trunk port

### DNS External
1. No changes needed unless VLAN needs custom upstream resolver
   - Default: all VLANs forward to Unbound via Bind9

### Verification
1. Connect test device to new VLAN
2. Confirm DHCP lease issued from correct subnet
3. Confirm correct hostname registered in DNS
4. Confirm forward resolution works
5. Confirm reverse resolution works
6. Confirm external resolution works
7. Confirm inter-VLAN rules working as expected
8. Confirm VLAN isolation - attempt to reach restricted VLANs
9. Update VLAN table in public documentation README