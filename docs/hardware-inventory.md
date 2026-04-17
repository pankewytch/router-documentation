# Hardware Inventory

## Protectli VP6670

### Specs
 - i7-1255U CPU
 - 32 Gb DDR5 Ram
 - 1 TB Samsung 990 Pro NVME
 - coreboot Bios
 - TPM-02

 ### Role
Router for the network being built.

## Reason for choosing
This firewall was selected to act as the router for 3 main reasons:
- Provided a blank canvas to work with allowing for a native linux environment to build the router from the ground up.
- i7-1255U and 32GB RAM give enough headroom for VPN, DPI, DNS filtering, and containerized services.
- Powerful i7 processor allows for DPI at speed.
- Coreboot BIOS provides firmware transparency and faster boot control.
- TPM 2.0 enables hardware root of trust for artifact verification and future attestation.
- Dual SFP+ plus multiple RJ45 ports give flexible WAN/LAN/VLAN split options.

## Ubiquiti Switch Pro Max 16

### Specs
- 16 RJ45 ports supporting 2.5 Gpbs speeds
- 2 SFP+ ports for high bandwidth internal networking