# VMware ESXi Home Lab

A staged, evidence-driven infrastructure home lab using a Dell OptiPlex 7060 running VMware ESXi. This repository documents installation, troubleshooting, network design, secure remote management, virtualization clustering, and (later) TrueNAS/ZFS storage engineering.

The lab also provides an environment for Windows Server, Linux, PowerShell, networking, and infrastructure operations practice.

## Current status

**Stage 1: technically complete. Stage 2: equipment ordered; deployment not yet verified.**

### Verified foundation (Stage 1)

| Component | Verified state |
| --- | --- |
| Physical host (`esxi01`) | Dell OptiPlex 7060 Tower, Intel Core i7-8700, approximately 32 GB RAM |
| Hypervisor | VMware ESXi 8.0.3, bare-metal installation |
| Local storage | Samsung SSD 870 1 TB SATA SSD used as ESXi installation target and local storage |
| First guest | Windows Server 2022, 2 vCPU, 4 GB RAM, 90 GB virtual disk |
| Guest integration | VMware Tools installed and reported compliant |
| Guest baseline | Windows Update completed; baseline snapshot `Baseline-WinServer2022-Updated` created |
| Temporary networking | Windows laptop Wi-Fi -> Windows Internet Connection Sharing (ICS) -> Ethernet -> ESXi |
| Last verified ESXi management IP | `192.168.137.2` (temporary ICS subnet) |
| Last verified Windows Server IP | `192.168.137.221` (temporary ICS subnet) |

**Storage troubleshooting note:** The SK hynix BC511 NVMe SSD was detected in BIOS but not offered by the ESXi installer. Installation succeeded after adding a Samsung SATA SSD. The definitive root cause of the NVMe detection issue has not been established.

### Stage 2 equipment

| Equipment | Model | Status |
| --- | --- | --- |
| Wi-Fi-to-Ethernet uplink | TP-Link RE705X | Ordered; not yet configured |
| Managed switch | TP-Link Omada ES210X-M2 | Ordered; not yet configured |
| Existing compute | Dell OptiPlex 7060 Tower (`esxi01`) | Operational |
| Ethernet patch cables | Cat6 or suitable existing cables | Availability to be verified |

## Roadmap

| Stage | Scope | Exit criterion | Status |
| --- | --- | --- | --- |
| **1** | [Single Bare-Metal ESXi Host](https://github.com/ryohei-1051/vmware-esxi-home-lab/issues/1) | ESXi and initial Windows Server baseline verified | Technically complete; issue closure pending |
| **2** | [Managed Switching and Internet Connectivity](https://github.com/ryohei-1051/vmware-esxi-home-lab/issues/2) | ESXi and VM connect through RE705X + managed switch **without** laptop ICS | In preparation |
| **3** | [Secure Remote Access with Tailscale](https://github.com/ryohei-1051/vmware-esxi-home-lab/issues/3) | Remote private access to ESXi verified from outside the home network | Planned |
| **4** | [vCenter and Two-Host Cluster](https://github.com/ryohei-1051/vmware-esxi-home-lab/issues/4) | Both ESXi hosts managed by vCenter under appropriate licensing | Planned; licensing prerequisite |
| **5** | [TrueNAS/ZFS Shared Storage Engineering](https://github.com/ryohei-1051/vmware-esxi-home-lab/issues/5) | ZFS protection/recovery tests and NFS/iSCSI datastore tests documented | Planned; intentionally deferred |
| **Final (optional)** | [Third ESXi Host and Advanced Cluster Testing](https://github.com/ryohei-1051/vmware-esxi-home-lab/issues/6) | Expansion justified and tested | Optional |

[GitHub Project board](https://github.com/users/ryohei-1051/projects/2/views/1)

## Near-term architecture (Stage 2 — planned)

```text
Home router / home Wi-Fi
          ))) Wireless uplink
TP-Link RE705X (Range Extender mode)
          |
      Ethernet
          |
TP-Link ES210X-M2 managed switch
          |
          +---- esxi01 (Dell OptiPlex 7060)
          +---- Admin laptop (optional wired connection)
          +---- Future hosts / TrueNAS (later stages)
```

This is **not yet an implemented topology**. The RE705X must be tested for simultaneous connectivity from the ESXi management interface and multiple VM MAC addresses; a working single-device Ethernet test is not sufficient proof.

The switch provides Layer 2 connectivity and VLAN management; routing between future VLANs requires a separate router/firewall capable of it. Initial Stage 2 testing will use the home network's ordinary untagged LAN, with VLAN segmentation deferred.

## Repository documentation

- [Hardware inventory](docs/01-hardware-inventory.md)
- [BIOS settings](docs/02-bios-settings.md)
- [ESXi installation plan](docs/03-installation-plan.md)
- [Network plan and migration checklist](docs/04-network-plan.md)
- [Installation and baseline verification log](docs/05-installation-log.md)
- [Storage installation troubleshooting](docs/06-storage-troubleshooting.md)
- [VM inventory](docs/07-vm-inventory.md)
- [Architecture diagrams](diagrams/)

## Documentation and evidence principles

1. Distinguish **planned**, **ordered**, **configured**, **verified**, and **completed** states.
2. Include expected results, actual test results, dates, screenshots/command outputs, and recovery steps.
3. Redact public IP addresses where needed, identifying details, credentials, serial/service tags, and sensitive configuration before publishing.
4. A snapshot is a short-term rollback point, **not** an independent backup; establish tested backups before destructive storage experiments.
5. Do not publicly forward ESXi, vCenter, SSH, or RDP management ports. Stage 3 uses private VPN access.

## Current next action

Finish the [Stage 2 network plan](docs/04-network-plan.md), record the ICS baseline, and verify the RE705X and switch when they arrive. TrueNAS/ZFS purchases are reserved for Stage 5.
