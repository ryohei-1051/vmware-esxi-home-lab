# Storage Troubleshooting — NVMe Not Detected by ESXi Installer

## Issue

During ESXi 8.0.3 installation, the installer did not detect the internal SK hynix BC511 NVMe 512 GB SSD. The installer only showed the USB installation media.

## Investigation

- Confirmed BIOS detected the internal M.2 PCIe SSD
- Confirmed SATA Operation was set to AHCI
- Disabled Secure Boot
- Rebooted and refreshed the ESXi disk selection screen

## Root Cause / Likely Cause

The NVMe SSD was visible to BIOS but not available as an ESXi installation target. This suggests an ESXi compatibility issue with the specific NVMe device/controller rather than a hardware failure.

## Resolution

Installed a Samsung SSD 870 1TB SATA SSD. ESXi detected the SATA SSD successfully, and installation completed.

## Verification

- ESXi 8.0.3 booted successfully
- ESXi Host Client was accessible from browser
- Local datastore was created on the Samsung SSD
