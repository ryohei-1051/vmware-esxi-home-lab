# Network and Storage Plan

## Stage 1 — Initial ESXi Network Setup
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

## Stage 2 — Managed Switch and Shared Storage Plan
## Goal
Expand the current single-host ESXi lab by adding a managed switch and shared storage device. This stage will introduce physical network switching, VLAN planning, NFS/iSCSI datastore testing, and shared storage concepts.

## Planned Equipment

- Managed switch: TBD
- Shared storage: 4-bay NAS or TrueNAS box
- Initial drives: 2 × NAS HDDs in mirror configuration
- ESXi host: esxi01
- Future host: esxi02

## Planned Storage Tests

- Create NAS storage pool
- Create NFS share for ESXi datastore
- Mount NFS datastore from ESXi
- Create iSCSI target/LUN
- Mount iSCSI datastore from ESXi
- Compare local datastore vs NFS vs iSCSI behavior

## Planned Network Tests

- Connect ESXi host, NAS, and admin laptop through managed switch
- Document port usage
- Confirm link speed
- Plan future VLANs for management, storage, vMotion, and VM networks

## Success Criteria

- ESXi can access NAS over the managed switch
- NFS datastore is mounted successfully
- iSCSI datastore is tested successfully
- Storage/network configuration is documented with screenshots
