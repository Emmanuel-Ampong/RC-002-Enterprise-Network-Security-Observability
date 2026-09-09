# RC-002 M1 — GNS3 Laboratory Environment Deployment

## Milestone

**M1 — GNS3 Laboratory Environment Deployment**

## Status

**VERIFIED**

---

## Objective

Establish and verify the network-emulation environment required for
RC-002 telemetry and observability experiments.

---

## Host Environment

- Host Operating System: Windows 11
- Processor: AMD Ryzen 5 4500U
- Processor Cores: 6
- Installed RAM: 8 GB
- GNS3 Desktop: 2.2.61
- Virtualization Platform: Oracle VirtualBox 7.2.4
- GNS3 VM Server: 2.2.61
- Initial GNS3 VM RAM Allocation: 2048 MB
- Initial GNS3 VM vCPU Allocation: 1

---

## Implementation

The GNS3 VM was imported into Oracle VirtualBox and configured with:

- Host-only networking
- NAT connectivity
- 2048 MB RAM
- 1 vCPU

GNS3 Desktop was configured to use VirtualBox as the virtualization
engine and automatically manage the GNS3 VM.

---

## Issue Encountered

During initial deployment, the GNS3 VM failed to start with the
following VirtualBox networking error:

`VERR_INTNET_FLT_IF_NOT_FOUND`

The failure occurred while VirtualBox attempted to attach the VM to the
VirtualBox Host-Only Ethernet Adapter.

---

## Troubleshooting

The following components were independently verified:

1. The VirtualBox host-only network existed.
2. The Windows VirtualBox Host-Only Ethernet Adapter was enabled.
3. GNS3 VM Adapter 1 referenced the correct host-only adapter.
4. The VM failed when started directly from VirtualBox, confirming that
   the issue was below the GNS3 application layer.

The VirtualBox installation was then repaired/reinstalled with the
VirtualBox networking and host-only networking components enabled.

The host operating system was restarted after the repair.

---

## Verification

Following the repair:

- The GNS3 VM successfully booted directly in VirtualBox.
- The GNS3 VM obtained host-only address `192.168.56.102`.
- The GNS3 server reported version `2.2.61`.
- KVM support inside the GNS3 VM reported `True`.
- GNS3 Desktop successfully started and managed the VM.
- The local GNS3 server reported healthy status.
- The GNS3 VM server reported healthy status.
- Both servers displayed green status indicators in GNS3.
- Successful operation was verified when GNS3 was launched without
  administrator privileges.

---

## Result

**PASS**

The RC-002 network-emulation platform is operational and ready for
controlled topology development.

---

## Research Significance

This milestone establishes the experimental infrastructure required to
move Project Aegis from logical network validation toward practical
security observability experiments.

The environment provides a foundation for integrating real operating
systems, network telemetry mechanisms, packet capture, and controlled
experimental traffic.

---

## Next Step

Develop the minimum RC-002 experimental network topology and evaluate
the network-device images and lightweight Linux systems required for
telemetry generation and collection.

