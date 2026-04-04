## Background

This repo is part of the [fpgas.online](https://fpgas.online) FPGA-as-a-Service platform.
The platform provides remote access to real FPGA boards (Arty A7, NeTV2, Fomu, TinyTapeout)
via PoE-powered Raspberry Pis that are network-booted. There are two deployment sites:
Welland (private test lab) and PS1 hackerspace (public service in Chicago).

This codebase was extracted from the original monorepo [`carlfk/pici`](https://github.com/CarlFK/pici)
in April 2026 using `git filter-repo` to preserve commit history. The monorepo was split into
purpose-specific repos under the `fpgas-online` GitHub organization, where each repo produces
installable artifacts (pip packages or deb packages) consumed by the infrastructure repo.

## Repository Overview

Server-side tools for preparing Raspberry Pi netboot filesystems. These scripts
run on the server (tweed), NOT on the Pis themselves.

### What This Provides

- **Image extraction**: `img/img2files.sh` extracts files from Raspberry Pi OS images into NFS roots
- **Netboot config**: `pinet/` contains config.txt, cmdline.txt, fstab, userconf.txt for netbooting
- **SD card tools**: `mkpisd/` scripts for creating SD cards
- **QEMU/chroot**: `qemu/` for running ARM binaries on x86 servers
- **Operator scripts**: `scripts/` for maintenance/production mode switching, TFTP symlinks, password generation
- **NFS setup**: `setup-nfs.sh` for NFS server debugging
- **Pi4 updates**: `update-pi4/` for Pi 4-specific boot config

### Key Scripts

- `scripts/maintenance.sh` -- Switch Pi NFS root to writable mode
- `scripts/production.sh` -- Switch Pi NFS root to read-only mode
- `scripts/mktftpln.sh` -- Create TFTP symlinks for Pi serial numbers (Pi v3 workaround)
- `scripts/pipw.sh` -- Generate random Pi user password and userconf.txt
- `img/img2files.sh` -- Extract Raspberry Pi OS image files to NFS server

### Deployment

Used by the infra repo's `img` and `pxe` roles on the server.

## Conventions

- **Python**: Use `uv` for all Python commands (`uv run`, `uv pip`). Never use bare `python` or `pip`.
- **Dates**: Use ISO 8601 (YYYY-MM-DD) or day-first formats. Never American-style month-first dates.
- **Commits**: Make small, discrete commits. Each logical unit of work gets its own commit.
- **License**: Apache 2.0.
- **Linting**: All repos have CI lint workflows. Fix lint errors before pushing.
- **No force push**: Branch protection is enabled on main. Never force push.

## Related Repos

| Repo | Purpose |
|------|---------|
| [fpgas.online-infra](https://github.com/fpgas-online/fpgas.online-infra) | Ansible infrastructure (playbooks, roles, inventory) |
| [fpgas.online-site](https://github.com/fpgas-online/fpgas.online-site) | Django web application |
| [fpgas.online-poe](https://github.com/fpgas-online/fpgas.online-poe) | SNMP PoE switch management |
| [fpgas.online-cam](https://github.com/fpgas-online/fpgas.online-cam) | Camera capture and streaming |
| [fpgas.online-setup-pi](https://github.com/fpgas-online/fpgas.online-setup-pi) | Raspberry Pi environment setup |
| [fpgas.online-netboot-pi](https://github.com/fpgas-online/fpgas.online-netboot-pi) | Netboot filesystem tools |
| [fpgas.online-tools](https://github.com/fpgas-online/fpgas.online-tools) | Utility scripts |
| [fpgas.online-test-designs](https://github.com/fpgas-online/fpgas.online-test-designs) | FPGA test designs |
| [apt](https://github.com/fpgas-online/apt) | APT package repository (GitHub Pages) |

## Linting

- shellcheck: blocking
