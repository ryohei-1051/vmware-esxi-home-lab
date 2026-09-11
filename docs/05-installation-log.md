# ESXi Installation Log — Host 01

## Date
- Installation date: 2026-08-30

## Steps Completed
- [x] Confirmed hardware inventory in Windows
- [x] Confirmed Ethernet link at 1 Gbps
- [x] Created ESXi installer USB
- [x] Entered BIOS
- [x] Disabled Secure Boot
- [x] Confirmed virtualization settings
- [x] Booted from USB installer
- [x] Selected internal NVMe SSD as install target
- [x] Installed ESXi
- [x] Configured management IP
- [x] Accessed ESXi Host Client from browser

## Issues Encountered
- Issue:
- Cause:
- Fix:
- Verification:

## Final Result
- ESXi Host Client accessible: Yes
- Management IP: 192.168.1.1
- Local datastore detected: Yes
- First VM deployed: Windows Server 2022

## Installation Result

- ESXi Version: VMware ESXi 8.0.3
- Host Hardware: Dell OptiPlex 7060
- CPU: Intel Core i7-8700
- Memory: 31.7 GiB detected
- Install Target: Samsung SSD 870, 1 TB SATA SSD
- NVMe Issue: SK hynix BC511 NVMe was detected by BIOS but not shown by ESXi installer
- Workaround: Installed Samsung SATA SSD and selected it as ESXi install target
- Management IP: 192.168.1.1
- ESXi Host Client Access: Successful
- Windows Server 2022 VM deployed successfully after attaching the datastore ISO file to the virtual CD/DVD drive and enabling Connect at power on.
