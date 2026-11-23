# Hardware Inventory — ESXi Host 01

## System
- Model: Dell OptiPlex 7060
- CPU: Intel Core i7-8700, 6 cores / 12 logical processors
- RAM: 32 GB DDR4
- BIOS Mode: UEFI
- BIOS Version: Dell 1.28.0, 2023-12-13

## Storage

### Original Storage
- Model: SK hynix BC511 NVMe
- Capacity: 512 GB
- BIOS detection: Successful
- ESXi installer detection: Unsuccessful
- Root cause: Not definitively established

### Current ESXi Storage
- Model: Samsung SSD 870
- Capacity: 1 TB
- Interface: SATA
- ESXi installation: Successful
- Local datastore: Created successfully

## Network
- Wired NIC: Intel Ethernet Connection (7) I219-LM
- Link Test: 1 Gbps link confirmed
- Wi-Fi: Intel Wireless-AC 9560, not planned for ESXi use

## RAM Layout
- 4 × 8 GB DDR4
- Upgrade note: 64 GB upgrade requires replacing all four sticks with 4 × 16 GB
