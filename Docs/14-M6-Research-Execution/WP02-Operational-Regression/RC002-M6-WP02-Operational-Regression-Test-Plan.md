# RC-002 M6 — WP-02 Operational Regression Test Plan

## 1. Purpose and Scope

This document defines a repeatable operational regression verification procedure for Project Aegis RC-002. Its purpose is to evaluate whether legitimate enterprise connectivity and essential infrastructure services remain operational before and after selected controlled configuration changes or experiments.

The plan addresses the operational regression and recovery gaps identified during the M6 WP-01 evidence audit and requirements traceability review.

### 1.1 Requirements Traceability

- **FR-11 — Operational Regression Verification:** Verify preservation of legitimate connectivity and essential services.
- **NFR-03 — Operational Preservation:** Evaluate whether the observability architecture materially disrupts legitimate network operations, using documented baseline and post-change measurements. The requirement does not prescribe a numerical degradation threshold.
- **RG-05 — Operational Regression and Recovery Verification:** Address incomplete before-and-after testing, recovery measurements, and unresolved operational failures.
- **M-05 — Operational Preservation:** Compare selected connectivity and service measurements against a predefined baseline.

### 1.2 Scope

The planned regression suite will evaluate selected USER, SERVER, OBSERVABILITY, and WAN connectivity paths; HTTP service availability; DNS resolution; routing and NAT operation; and continued Syslog collection from existing telemetry sources.

Tests will be performed against the existing RC-002 laboratory architecture. No configuration change will be introduced until the initial baseline has been recorded.

### 1.3 Evidence and Interpretation Rules

Each test must record its source, destination, expected result, observed result, execution context, and supporting evidence. Post-change results must be compared with corresponding baseline results.

Failures, unexpected degradation, recovery actions, and unresolved limitations must be documented. Historical M2–M5 results provide context but do not substitute for fresh WP-02 measurements.

Passing selected regression tests does not establish uninterrupted operation of every service or zero operational impact under all conditions.

## 2. Operational Regression-Test Matrix

The following tests are planned for the existing RC-002 laboratory. All baseline results must be measured before any new controlled configuration change. A successful historical M2–M5 test does not automatically count as a WP-02 baseline pass.

| Test ID | Source | Destination / Service | Verification | Expected Outcome |
|---|---|---|---|---|
| REG-01 | CLIENT-1 | 192.168.10.1 | ICMP to USER gateway | Gateway reachable |
| REG-02 | CLIENT-1 | 192.168.10.11 | Same-zone ICMP | CLIENT-2 reachable |
| REG-03 | CLIENT-1 | 192.168.20.10 | Inter-zone ICMP | SERVER-1 reachable |
| REG-04 | CLIENT-1 | 192.168.30.10 | Inter-zone ICMP | TELEMETRY-1 reachable |
| REG-05 | TELEMETRY-1 | http://192.168.20.10/ | HTTP request | Expected test page returned |
| REG-06 | SERVER-1 | 8.8.8.8 | External ICMP | Replies received; loss and latency recorded |
| REG-07 | SERVER-1 | External hostname | DNS lookup | Hostname resolves successfully |
| REG-08 | EDGE-R1 | WAN interface and routing table | Address and route inspection | WAN address and expected default route present |
| REG-09 | EDGE-R1 | NAT configuration | Rule and counter inspection | Expected MASQUERADE rule present; translation activity evaluated using controlled traffic |
| REG-10 | SERVER-1 and EDGE-R1 | TELEMETRY-1 Syslog collector | Source-attributed log verification | Fresh events from both sources observed at collector |

### 2.1 Execution Rules

1. Confirm the GNS3 topology and device states before testing.
2. Record execution date, time, device, command, and relevant configuration context.
3. Execute each applicable baseline test and retain its raw output.
4. Record PASS, FAIL, or NOT TESTED with supporting evidence; do not assume success from historical results.
5. Record packet loss, latency, HTTP response, DNS result, or other measurable outputs where applicable.
6. Repeat the same tests after each selected controlled change, maintaining comparable test conditions.
7. Document unexpected failures, troubleshooting, recovery actions, and unresolved limitations.
8. Compare baseline and post-change results against FR-11, NFR-03, RG-05, and M-05.

**Measurement limitation:** A successful ICMP or HTTP check establishes only the observed result under the tested conditions. It does not prove continuous availability or zero performance impact.
