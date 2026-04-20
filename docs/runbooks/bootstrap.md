# Bootstrap Guide

## Purpose
This runbook documents the complete procedure for bootstrapping Sol (Protectli VP6670) from a fresh Ubuntu 24 LTS install to a configured router. Follow this exactly in order on any rebuild.

## Prerequisites
- Serial console connected before powering on the router
- Ubuntu 24 LTS installed
- Admin user created
- Physical interface map verified

## Step 1 - Harden SSH (from COM1 TTY)
1. Copy ssh key from local machine - `ssh-copy-id <user>@sol`
    - This can only be run after sol is set up as a known host on your local machine's config file in .ssh
2. Disable 50-cloud-init.conf override config
   - Rename -> `mv /etc/ssh/sshd_config.d/50-cloud-init.conf /etc/ssh/sshd_config.d/50-cloud-init.conf.backup`
3. Deploy or copy `99-custom.conf` from the `bootstrap` directory IaC repo to `/etc/ssh/sshd_config.d/`
4. Restart SSH - `sudo systemctl restart ssh`
5. Verify public key auth is enabled, root log in is disabled, and password log in is disabled - `sudo sshd -T | grep -E "passwordauthentication|permitrootlogin|pubkeyauthentication"`

## Step 2 - Network Interface Transition
1. Connect the router to ethernet via RJ45 Port 4 - `enp6s0`
2. Check that systemd-networkd is running - `sudo systemctl status systemd-networkd`
3. Check the port to ensure it is active - `sudo networkctl status enp6s0`
4. Deploy or copy the config `ethernet.network` from the `bootstrap` into `/etc/systemd/network/`
    - If desired, modify the config for a static IP. Prefer editing the file on the router once it is reachable.
5. Restart networkd - `sudo systemctl restart systemd-networkd`
6. Verify systemd-networkd is running and that the interface is running.

## Step 3 - Install Packages
1. Manually run install-packages.sh as admin user
   - sudo ./scripts/packages/install-packages.sh
2. Verify all packages installed successfully
3. From this point forward pipeline owns package management

## Step 4 - Create Deploy User
[to be written]

## Step 5 - Verify Pipeline
[to be written]

## Notes
- Steps 4 and 5 are pending implementation

# Bootstrap Guide

## Note on Ubuntu 24 Fresh Install
Netplan on Ubuntu 24 generates configs to /run/systemd/network/ not /etc/systemd/network/ which is ephemeral and wiped on reboot.
Transition process:
1. Write your configs to /etc/systemd/network/
2. Remove Netplan package
3. Reboot
4. systemd-networkd picks up your configs automatically
No manual config cleanup required.

## Prerequisites Before Running Bootstrap script

The following must be done manually before running bootstrap.sh:

1. Copy signing.pub from git runner pi to ~/signing.pub on router
   - On git runner pi: scp ~/.secrets/signing-key-pub.pem USER@ROUTER_IP:~/signing.pub
2. Ensure the IaC repo is cloned on router
3. Ensure you are running as your admin user with sudo access

## Bootstrap Order
Running ./bootstrap.sh all will execute steps in this exact order:
1. install-packages   - installs tpm2-tools and all required packages
2. harden-ssh         - disables password auth, deploys custom config
3. create-deploy-user - creates deploy user and directory structure
4. enroll-tpm         - enrolls signing.pub hash into TPM, moves key to final location
5. setup-sudoers      - configures least privilege sudo for deploy user
6. setup-network      - deploys systemd-networkd config