# ESXi Installation Plan — Host 01

## Goal
Install VMware ESXi bare-metal on Dell OptiPlex 7060 and access the ESXi Host Client from a browser.

## Install Target
- Internal NVMe SSD: SK hynix BC511 512 GB
- Warning: Existing Windows 11 installation will be erased.

## Installation Media
- ESXi installer USB
- Boot method: F12 one-time boot menu

## Post-install Target
- Hostname: esxi01.lab.local
- Management network: wired Ethernet only
- Wi-Fi: not used
- Initial storage: local NVMe datastore
