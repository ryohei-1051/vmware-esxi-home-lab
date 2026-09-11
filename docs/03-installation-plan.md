# ESXi Installation Plan — Host 01

## Goal
Install VMware ESXi bare-metal on Dell OptiPlex 7060 and access the ESXi Host Client from a browser.

## Install Target

### Original planned target
- Internal NVMe SSD: SK hynix BC511 512 GB

### Final install target
- Samsung SSD 870 1 TB SATA SSD

## Note
The original NVMe SSD was detected by BIOS but was not detected by the ESXi installer. A Samsung SATA SSD was installed and used as the final ESXi installation target.

## Installation Media
- ESXi installer USB
- Boot method: F12 one-time boot menu

## Post-install Target
- Hostname: esxi01.lab.local
- Management network: wired Ethernet only
- Wi-Fi: not used
- Initial storage: local NVMe datastore
