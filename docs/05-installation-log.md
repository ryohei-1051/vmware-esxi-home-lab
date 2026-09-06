# ESXi Installation Log — Host 01

## Date
- Installation date:

## Steps Completed
- [ ] Confirmed hardware inventory in Windows
- [ ] Confirmed Ethernet link at 1 Gbps
- [ ] Created ESXi installer USB
- [ ] Entered BIOS
- [ ] Disabled Secure Boot
- [ ] Confirmed virtualization settings
- [ ] Booted from USB installer
- [ ] Selected internal NVMe SSD as install target
- [ ] Installed ESXi
- [ ] Configured management IP
- [ ] Accessed ESXi Host Client from browser

## Issues Encountered
- Issue:
- Cause:
- Fix:
- Verification:

## Final Result
- ESXi Host Client accessible: Yes / No
- Management IP:
- Local datastore detected: Yes / No

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
