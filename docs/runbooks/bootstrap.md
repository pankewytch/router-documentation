# Bootstrap Guide

## Note on Ubuntu 24 Fresh Install
Netplan on Ubuntu 24 generates configs to /run/systemd/network/ not /etc/systemd/network/ which is ephemeral and wiped on reboot.
Transition process:
1. Write your configs to /etc/systemd/network/
2. Remove Netplan package
3. Reboot
4. systemd-networkd picks up your configs automatically
No manual config cleanup required.