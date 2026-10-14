
# RC-002 — WP-02 Baseline Routing and NAT Evidence

**Project:** Project Aegis  
**Milestone:** M6 — Research Execution  
**Work Package:** WP-02 — Operational Regression Verification  
**Test IDs:** REG-08 and REG-09  
**Execution Date:** 13 October 2026  
**Device:** EDGE-R1  
**Environment:** GNS3  
**Evidence Type:** Transcript-derived console observations  

## 1. Evidence Provenance

This document preserves the routing and NAT observations collected during the WP-02 baseline regression tests.

The evidence was reconstructed from EDGE-R1 console outputs shared during execution. It is not an independently exported GNS3 console capture.

Both tests were conducted as read-only inspections. No routing or NAT configuration changes were made.

## 2. REG-08 — IPv4 Routing Verification

**Device:** EDGE-R1  
**Command:** `ip -4 route show`  
**Result:** PASS WITH LIMITATION

### 2.1 Observed Routing Table

```text
default via 192.168.42.1 dev eth3 metric 205
192.168.10.0/24 dev eth0 proto kernel scope link src 192.168.10.1
192.168.20.0/24 dev eth1 proto kernel scope link src 192.168.20.1
192.168.30.0/24 dev eth2 proto kernel scope link src 192.168.30.1
192.168.42.0/24 dev eth3 proto kernel scope link src 192.168.42.184
```

### 2.2 Assessment

The routing table showed:

- A default route through `192.168.42.1` using `eth3`.
- A directly connected USER network (`192.168.10.0/24`) on `eth0`.
- A directly connected SERVER network (`192.168.20.0/24`) on `eth1`.
- A directly connected TELEMETRY network (`192.168.30.0/24`) on `eth2`.
- A directly connected WAN network (`192.168.42.0/24`) on `eth3`.
- WAN source address `192.168.42.184` in the connected route.

These observations support the expected IPv4 routing configuration at the time of inspection.

### 2.3 Evidence Limitation

The routing table alone does not independently establish:

- The operational state of every interface.
- The status or validity of the WAN DHCP lease.
- Continuous route availability.
- Successful forwarding for every possible traffic flow.

For these reasons, the test is classified as **PASS WITH LIMITATION**.

## 3. REG-09 — NAT POSTROUTING Verification

**Device:** EDGE-R1  
**Command:** `iptables -t nat -L POSTROUTING -n -v`  
**Result:** PASS WITH LIMITATION

### 3.1 Observed NAT Output

```text
Chain POSTROUTING (policy ACCEPT 35 packets, 3431 bytes)
 pkts bytes target     prot opt in     out     source               destination
   77 15386 MASQUERADE  all  --  *      eth3    0.0.0.0/0            0.0.0.0/0
```

### 3.2 Assessment

The NAT POSTROUTING chain contained a `MASQUERADE` rule for traffic leaving through `eth3`.

At inspection time, the rule displayed cumulative counters of:

- **77 packets**
- **15,386 bytes**

The rule and nonzero counters support the presence and prior use of the outbound NAT configuration.

### 3.3 Evidence Limitation

The recorded counters are cumulative observations.

No controlled before-and-after counter comparison was performed during REG-09. Therefore, the 77 packets and 15,386 bytes cannot be attributed specifically to the WP-02 connectivity tests.

The output does not independently establish that every external flow was translated successfully.

For these reasons, the test is classified as **PASS WITH LIMITATION**.

## 4. Overall Evidence Assessment

| Test ID | Verification | Result |
|---|---|---|
| REG-08 | IPv4 routing table inspection | PASS WITH LIMITATION |
| REG-09 | NAT POSTROUTING inspection | PASS WITH LIMITATION |

Both tests provided supporting evidence for the operational baseline.

Neither test produced an observed configuration failure, but the available outputs are insufficient to remove the stated verification limitations.

## 5. Evidence Limitations and Follow-Up

- The records are transcript-derived rather than independently exported console captures.
- Interface state and DHCP lease status were not separately verified in REG-08.
- A controlled NAT counter delta was not captured in REG-09.
- No post-change routing or NAT comparison has yet been performed.
- These observations do not, by themselves, establish full operational preservation.

Additional targeted checks may be performed during post-change regression verification.
