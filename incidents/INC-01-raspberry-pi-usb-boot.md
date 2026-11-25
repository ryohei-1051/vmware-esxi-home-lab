
# INC-01 — Raspberry Pi USB Boot Failure After OS Update

## Environment
- Hardware: Raspberry Pi 3 Model B Rev 1.2
- OS: Raspberry Pi OS (Debian 13 Trixie)
- Storage: SanDisk Cruzer Fit USB flash drive
- Role: Planned Tailscale subnet router

## Incident Summary
After running `sudo apt full-upgrade -y` and rebooting,
the Raspberry Pi became unreachable through SSH and
stopped producing HDMI output.

## Symptoms
- SSH connection timed out
- HDMI monitor displayed "No Signal"
- Red PWR LED remained solid
- Green ACT LED showed no observed activity

## Investigation
1. Booted successfully using the original microSD card.
2. Confirmed that Linux detected the USB flash drive.
3. Verified USB boot and root partitions.
4. Confirmed required boot files were present.
5. Verified root PARTUUID matched cmdline.txt.
6. Confirmed USB boot OTP was enabled (17:3020000a).
7. Verified updated kernel and initramfs files were present.

## Recovery
- Subsequent USB boot attempt succeeded.
- Raspberry Pi OS started successfully.
- SSH connectivity was restored.
- Tailscale was installed and authenticated successfully.

## Root Cause
Not definitively established.

Possible areas include intermittent USB initialization,
USB flash drive compatibility, or boot behavior following
the kernel update.

## Verification
- Raspberry Pi boots from USB.
- SSH connection succeeds.
- Tailscale service is active.
- Tailscale peer connectivity succeeds.
- SSH through Tailscale succeeds.

## Lessons Learned
- Separate hardware, boot, network, and service failures.
- Maintain independent recovery media.
- Verify filesystem and boot configuration before making changes.
- Do not assume a kernel update caused a failure without evidence.
