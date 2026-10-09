# RC-002 M5 - A03 Results

## Controlled Denied-Access Activity

**Project:** Project Aegis
**Research Cycle:** RC-002
**Milestone:** M5 - Controlled Abnormal Behavior
**Scenario:** A03 - Controlled Denied-Access Activity
**Status:** Five controlled trials executed; firewall rollback verified; results documented with limitations

---

## 1. Objective

A03 evaluated whether deliberately denied TCP connection attempts could be observed and measured through packet capture and firewall counters while preserving legitimate network operations.

The experiment examined connection-attempt outcomes, TCP SYN retransmissions, firewall DROP counters, centralized telemetry availability, and operational preservation.

Unlike A01 and A02, A03 introduced a temporary, narrowly scoped firewall restriction to generate reproducible denied-access behavior.

---

## 2. Experimental Path

**Source:**

`SERVER-1 (192.168.20.10)`

**Destination:**

`TELEMETRY-1 (192.168.30.10)`

**Firewall enforcement point:**

`EDGE-R1`

**Restricted service:**

`TCP/22 - SSH`

**Packet-capture location:**

`EDGE-R1 eth1 to SERVER-SW1 Ethernet0`

The experimental firewall rule was:

```sh
iptables -I FORWARD 1 -s 192.168.20.10 -d 192.168.30.10 -p tcp --dport 22 -j DROP
```

The rule was installed before T01 and retained through T05. Its packet and byte counters were reset before each trial.

Each trial consisted of:

1. Five sequential TCP/22 connection attempts.
2. A three-second timeout per connection attempt.
3. A two-second delay between successive attempts.
4. Firewall DROP-counter inspection.
5. Wireshark packet inspection.
6. Internal ICMP and TCP/514 connectivity checks.
7. UDP/514 telemetry-marker transmission and collector verification.
8. HTTP application and external ICMP connectivity checks.

Five controlled trials, A03-T01 through A03-T05, were executed.

After T05, the experimental rule was removed and TCP/22 connectivity was tested again.

---

## 3. Trial Results

| Trial | Attempts | Timed out | DROP packets | DROP bytes | Initial SYNs | SYN retransmissions | Operational result |
|---|---:|---:|---:|---:|---:|---:|---|
| A03-T01 | 5 | 5 | 10 | 600 | 5 | 5 | PASS with exception |
| A03-T02 | 5 | 5 | 10 | 600 | 5 | 5 | PASS |
| A03-T03 | 5 | 5 | 10 | 600 | 5 | 5 | PASS |
| A03-T04 | 5 | 5 | 10 | 600 | 5 | 5 | PASS |
| A03-T05 | 5 | 5 | 10 | 600 | 5 | 5 | PASS |

All 25 connection attempts timed out under the experimental firewall restriction.

Each trial produced five initial TCP SYN packets and five observed SYN retransmissions.

The firewall DROP counter recorded 10 packets and 600 bytes per trial.

The T01 operational exception is discussed separately below.

---

## 4. Aggregate Results

Across the five trials:

- Controlled trials executed: 5.
- TCP/22 connection attempts: 25.
- Timed-out connection attempts: 25.
- Successful TCP/22 connections during enforcement: 0.
- Firewall DROP packets: 50.
- Firewall DROP bytes: 3,000.
- Initial TCP SYN packets observed: 25.
- TCP SYN retransmissions observed: 25.
- Trials with successful internal ICMP connectivity: 5.
- Trials reporting TCP/514 reachability: 5.
- Trials with confirmed UDP/514 collector receipt: 5.
- Trials returning HTTP 200: 5.
- Trials with successful external ICMP connectivity checks: 5, including T01 after a repeat test.
- Trials with a documented external ICMP packet-loss exception: 1.
- Post-experiment TCP/22 restoration: confirmed.

The aggregate firewall packet count represents observed dropped packets, not the number of distinct connection attempts.

The 25 retransmission observations are distinct from the 25 initial SYN packets.

---

## 5. Comparison with M4 Normal Operation

A03 was conducted within the same RC-002