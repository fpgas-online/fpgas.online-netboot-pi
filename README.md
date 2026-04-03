# fpgas.online-netboot-pi

Tools for preparing Raspberry Pi netboot filesystems for the fpgas.online FPGA-as-a-Service platform.

## Overview

This repository contains operator-side tools that run on the fpgas.online server (tweed), not on the Pis themselves. They handle SD card image extraction, NFS root filesystem preparation, QEMU/chroot for ARM package installation on x86 hosts, maintenance/production mode switching, and TFTP symlink management for Pi serial numbers.

fpgas.online uses PoE-powered, network-booted Raspberry Pis. These tools prepare and maintain the netboot infrastructure that those Pis boot from.

## Key Scripts

### Image Extraction

- **`img/img2files.sh`** -- Extracts a Raspberry Pi OS SD card image into a filesystem tree suitable for NFS serving.

### NFS Root Preparation

- **`setup-nfs.sh`** -- Prepares an NFS root filesystem for Pi netboot.

### Mode Switching

- **`scripts/maintenance.sh`** -- Switches a Pi's netboot root to maintenance mode.
- **`scripts/production.sh`** -- Switches a Pi's netboot root to production mode.

### TFTP and Network

- **`scripts/mktftpln.sh`** -- Creates TFTP symlinks mapping Pi serial numbers to their boot directories.
- **`scripts/pipw.sh`** -- Generates Pi user passwords.

### QEMU/Chroot

- **`qemu/`** -- Scripts for running ARM chroot environments on x86 hosts via QEMU user-mode emulation, used to install packages into Pi filesystem images.

### SD Card

- **`mkpisd/`** -- Scripts for creating bootable SD cards.

### Pi 4 Updates

- **`update-pi4/`** -- Scripts specific to Raspberry Pi 4 firmware and bootloader updates.

## Directory Structure

```
img/          SD card image extraction (img2files.sh)
pinet/        Netboot configuration files
mkpisd/       SD card creation scripts
qemu/         QEMU/chroot scripts for ARM-on-x86
scripts/      Operational scripts (maintenance, production, TFTP, passwords)
setup-nfs.sh  NFS root preparation
update-pi4/   Pi 4-specific update scripts
```

## Usage

These tools are intended for infrastructure operators. They are run on the server that hosts the NFS roots and TFTP boot files, not on individual Pis.

Typical workflow:

1. Download a Raspberry Pi OS image.
2. Extract it with `img/img2files.sh`.
3. Prepare the NFS root with `setup-nfs.sh`.
4. Use `qemu/` chroot to install packages into the ARM filesystem.
5. Create TFTP symlinks with `scripts/mktftpln.sh` for each Pi's serial number.
6. Switch between maintenance and production modes as needed.

## Linting

- Shell: [shellcheck](https://www.shellcheck.net/)

## Related Repositories

- [fpgas.online-infra](https://github.com/fpgas-online/fpgas.online-infra) -- Ansible playbooks that orchestrate deployment
- [fpgas.online-setup-pi](https://github.com/fpgas-online/fpgas.online-setup-pi) -- On-Pi configuration (installed into the netboot root)
- [apt](https://github.com/fpgas-online/apt) -- APT repository for Pi packages

## License

See [LICENSE](LICENSE).
