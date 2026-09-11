# Network Plan — Initial ESXi Setup

## Current Temporary Setup

- ESXi Management IP: 192.168.1.1
- Access URL: https://192.168.1.1
- Connection type: Direct Ethernet connection to admin laptop
- Note: This IP is temporary and will be changed before connecting the host to the managed switch/router environment.

## Planned Stable Setup

- ESXi Hostname: esxi01.lab.local
- ESXi Management IP: 192.168.1.50
- Subnet Mask: 255.255.255.0
- Gateway: home router IP
- DNS: home router IP or lab DNS server
- VLAN: none initially; VLAN segmentation planned in Stage 2/3
