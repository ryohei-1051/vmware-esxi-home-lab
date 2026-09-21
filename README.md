# VMware ESXi Home Lab

This repository documents my bare-metal VMware ESXi home lab built on a Dell OptiPlex 7060. The lab is used to practice virtualization, Windows Server administration, Linux administration, networking, storage, backup/recovery, and infrastructure troubleshooting.

## Current Status

- Installed VMware ESXi 8.0.3 on Dell OptiPlex 7060
- Configured local datastore using Samsung SSD 870 1TB SATA SSD
- Deployed first Windows Server 2022 VM
- Confirmed ESXi Host Client access from browser

## Roadmap

The lab roadmap is tracked using GitHub Projects.

- Stage 1: Single bare-metal ESXi host
- Stage 2: Managed switch and shared storage
- Stage 3: Additional ESXi host and vCenter deployment
- Final Stage: Optional third ESXi host

Project board: https://github.com/users/ryohei-1051/projects/2/views/1

## Lab Goals

- Build a reusable infrastructure platform for Windows, Linux, networking, PowerShell, Azure hybrid, and security labs
- Practice ESXi host management, VM provisioning, datastore management, and virtual networking
- Later expand to multiple ESXi hosts, vCenter, clustering, vMotion, HA, and shared storage using NFS/iSCSI
