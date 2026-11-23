# Network Plan — Temporary ICS to Managed Home Lab

**Scope:** Stage 2 (Managed Switching and Internet Connectivity) and its prerequisites. Tailscale is Stage 3; vCenter/second host is Stage 4; TrueNAS/NFS/iSCSI storage is Stage 5.

**State:** Plan prepared. TP-Link RE705X and ES210X-M2 ordered; neither has been installed or validated as part of the lab yet.

## 1. Objective and boundaries

Replace the working but laptop-dependent Windows Internet Connection Sharing (ICS) arrangement with an always-available upstream connection through a TP-Link RE705X, plus managed wired Ethernet via a TP-Link ES210X-M2.

Stage 2 is complete **only when** ESXi and its Windows Server VM retain internet and management connectivity without the laptop serving as a gateway.

No public-facing ESXi/vCenter ports and no NFS/iSCSI deployment are part of this stage.

## 2. Verified temporary topology (Stage 1 baseline)

```text
Home router --Wi-Fi--> Windows laptop (ICS)
                              |
                    Laptop Ethernet: 192.168.137.1/24
                              |
                         Direct cable
                              |
                    esxi01: 192.168.137.2/24
                              |
                          vSwitch0
                              |
              VM Network / Windows Server 2022
                    Last observed: 192.168.137.221
```

| Setting | Last verified | Notes |
| --- | --- | --- |
| Laptop Ethernet/ICS gateway | `192.168.137.1/24` | Windows ICS assigned address |
| ESXi management | `192.168.137.2/24` | Working ESXi Host Client URL: `https://192.168.137.2` |
| Windows Server 2022 VM | `192.168.137.221` | IP observed during Windows Update verification |
| ESXi default gateway | `192.168.137.1` | From prior migration plan; reconfirm on ESXi before changes |
| Home Wi-Fi router LAN/DHCP range | **Not yet recorded** | Discover before specifying target ESXi address |
| Laptop Wi-Fi address | **Not yet recorded** | DHCP from home router |

The historical address `192.168.1.1` was used for early direct-cable management; it is **not** the current verified ESXi management address.

### Record baseline evidence before changing the network

From the Windows laptop (PowerShell):

```powershell
Get-NetIPConfiguration
Get-NetAdapter | Select-Object Name, Status, LinkSpeed
```

From the Windows Server VM (PowerShell):

```powershell
ipconfig /all
Test-NetConnection 1.1.1.1 -Port 443
Resolve-DnsName www.microsoft.com
```

From the ESXi Host Client/DCUI, capture management IPv4 settings, default gateway, DNS, physical uplink, `vSwitch0`, and the `VM Network` port group.

Store sanitized screenshots/logs with the Stage 2 evidence. Avoid exporting identifiable information or credentials to this public repository.

## 3. Target Stage 2 topology (not yet verified)

```text
Home router / Wi-Fi
       ))) wireless (existing home network)
TP-Link RE705X --[1 GbE Ethernet]-- ES210X-M2 managed switch
                                          |
                                          +-- Port 2: esxi01 (1 GbE NIC)
                                          +-- Port 3: admin laptop (optional)
                                          +-- Port 4: future esxi02
                                          +-- Port 5: future TrueNAS
                                          +-- Ports 6-8: spare

     esxi01 (vSwitch0 / VM Network)
         +-- Windows Server 2022 VM
         +-- Future Tailscale Linux VM (Stage 3)
```

**Important physical constraints:** The RE705X Ethernet port is Gigabit Ethernet, and the existing ESXi host's Intel I219-LM NIC is Gigabit Ethernet. The switch's 2.5GbE ports do not increase these link speeds. Keep the RE705X outside metal shelving where Wi-Fi signal is reliable.

**Virtualization compatibility test:** The RE705X must pass simultaneous DHCP/ARP/Internet tests for ESXi management **and** guest VM(s) behind the same physical link. Some Wi-Fi extenders perform MAC-address translation/proxying; do not assume normal wired-switch behavior until measured. If connectivity is unreliable, investigate extender compatibility or use a more capable bridge/router or wired backhaul.

## 4. Planned port allocation

| Port | Device | Expected speed | Stage | Notes |
| --- | --- | --- | --- | --- |
| 1 | RE705X Ethernet | 1 GbE | 2 | Upstream through home Wi-Fi |
| 2 | `esxi01` (Intel I219-LM) | 1 GbE | 2 | ESXi/VMs share one physical uplink |
| 3 | Admin laptop (optional) | NIC-dependent | 2 | Useful for recovery/initial setup |
| 4 | `esxi02` | NIC-dependent | 4 | Reserved |
| 5 | TrueNAS storage server | NIC-dependent | 5 | Reserved; extra links may be needed later |
| 6–8 | Expansion | TBD | Later | Storage, management, or additional hosts |
| 10G SFP+ slots | Not allocated | N/A | Later | Do not assume optics/cables are included |

Use the default untagged LAN during initial bring-up. VLANs do not route between themselves without suitable Layer 3 routing/firewall configuration. Do not assume VLAN tags traverse the RE705X wireless link.

## 5. Permanent addressing plan — to be discovered

**Do not apply fixed `192.168.1.x` addresses until the real home router LAN subnet, DHCP pool, and gateway are confirmed.**

| Device | Initial approach | Long-term approach |
| --- | --- | --- |
| RE705X | Normal extender management address from upstream environment | Record actual address in inventory |
| ES210X-M2 | DHCP or its documented initial management method | Management reservation or planned static address |
| ESXi `esxi01` management | DHCP for the first live migration, with console fallback | DHCP reservation or static IP outside DHCP pool |
| Windows Server VM | DHCP | Reservation if required |
| Tailscale VM (Stage 3) | N/A | Reserved/static private management IP |
| DNS/hostname | Record actual DNS service | Use a suitable internal DNS namespace when DNS exists; `esxi01.lab.home.arpa` is an example only |

`*.local` is commonly used by multicast DNS; avoid assuming `esxi01.lab.local` will behave as ordinary unicast DNS without confirming the name-resolution design. A hostname only works when the chosen DNS server actually serves it.

Capture after deployment:

```text
Router LAN subnet:         <TO VERIFY>
Router LAN gateway:        <TO VERIFY>
DHCP pool:                 <TO VERIFY>
Switch management IP:      <TO VERIFY>
RE705X management IP:      <TO VERIFY>
esxi01 management IP:      <TO VERIFY>
esxi01 gateway / DNS:      <TO VERIFY>
Windows Server VM IPv4:    <TO VERIFY>
```

## 6. Deployment sequence and checkpoints

### A. Validate the wireless bridge in isolation

- [ ] Record RE705X hardware revision, firmware version, management access method, and Wi-Fi signal quality.
- [ ] Configure **Range Extender mode**; do **not** configure Access Point mode, which expects wired upstream.
- [ ] Connect laptop directly to RE705X Ethernet and verify DHCP, gateway, DNS, and internet access.
- [ ] Record measured link speed and any MAC/address differences shown in router DHCP clients.

### B. Add the managed switch

- [ ] Connect RE705X Ethernet to switch port 1.
- [ ] Connect laptop to port 3; verify DHCP and internet without laptop ICS.
- [ ] Access switch management; record firmware, management IP, and port/link state.
- [ ] Leave host ports on default untagged LAN; defer VLAN configuration.
- [ ] Label cables and export/record the switch configuration if supported.

### C. Migrate ESXi management carefully

- [ ] Confirm physical DCUI/monitor recovery path before changing the ESXi management IP.
- [ ] Record current ESXi IP, gateway, DNS, port group, and uplink.
- [ ] Attach esxi01 physical Ethernet to switch port 2.
- [ ] Configure ESXi to obtain management addressing via DHCP initially (or use a verified nonconflicting static assignment).
- [ ] Confirm the IP on ESXi DCUI or another trusted management view.
- [ ] Reconnect to `https://<actual-new-ESXi-IP>`.
- [ ] Configure reservation or stable address **only after** confirming the home LAN subnet.
- [ ] Confirm VM Network guest DHCP and guest internet independently from ESXi management.

### D. Verify independence and restart behavior

- [ ] Disable Windows ICS on the laptop (after successful migration).
- [ ] Confirm ESXi management and Windows Server guest connectivity simultaneously.
- [ ] Power cycle/reconnect the extender and/or switch in a controlled test; verify that both recover.
- [ ] Verify recovery without depending on an active laptop connection.
- [ ] Capture final port map, addressing, link speeds, and sanitized screenshots.

## 7. Acceptance tests

| Test ID | Test | Expected result | Actual / evidence |
| --- | --- | --- | --- |
| NET-01 | RE705X wired laptop test | Laptop obtains usable IP/gateway/DNS and reaches internet | Pending |
| NET-02 | Managed switch link and management | Correct ports up; switch management reachable | Pending |
| NET-03 | ESXi management migration | New ESXi Host Client URL reachable | Pending |
| NET-04 | ESXi and guest DHCP/ARP coexistence | Both ESXi and VM receive usable distinct addresses and function together | Pending |
| NET-05 | Guest DNS and HTTPS | `Resolve-DnsName` succeeds; TCP 443 test succeeds | Pending |
| NET-06 | No laptop ICS | NET-03 and NET-05 still pass with ICS disabled | Pending |
| NET-07 | Reboot/reconnect recovery | Management/guest connectivity return after controlled restart | Pending |
| NET-08 | Evidence completeness | Diagram, IP table, screenshots, incident notes saved and redacted | Pending |

Do not mark any test as passed before collecting output or screenshots. Internet access in the VM alone is not proof that ESXi host management, DNS, or remote access is working correctly.

## 8. Troubleshooting and rollback

- If the extender is connected but the guest fails, compare ESXi vs guest DHCP leases, ARP/MAC behavior, bridge proxying, vSwitch uplink, guest port-group connection, gateway, and DNS.
- If ESXi Host Client becomes unreachable, **do not** reset the host. Check the current ESXi address on physical DCUI first; verify laptop subnet and switch port connectivity.
- Preserve the pre-migration IP information. Revert to the temporary direct-cable/ICS arrangement or use DCUI to recover management networking if necessary.
- An unsuccessful `ping` alone does not establish a failed link (ICMP may be filtered or the test source may be in a different subnet). Use link state, IP configuration, DNS, TCP 443, and Host Client access together.

## 9. Future boundaries

- **Stage 3:** Tailscale Linux subnet router for private remote ESXi access. No public management port forwarding.
- **Stage 4:** Second ESXi host, vCenter, and cluster management after licensing/resource checks.
- **Stage 5:** TrueNAS/ZFS engineering, NFS/iSCSI, backup and resilver tests. Revisit VLANs, storage traffic isolation, and network capacity then.

## References

- [Stage 2 GitHub Issue](https://github.com/ryohei-1051/vmware-esxi-home-lab/issues/2)
- [TP-Link RE705X official documentation](https://www.tp-link.com/ca/home-networking/range-extender/re705x/)
- [TP-Link ES210X-M2 product support](https://support.omadanetworks.com/en/product/es210x-m2)
