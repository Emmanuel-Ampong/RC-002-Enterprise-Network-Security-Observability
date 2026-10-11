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

A03 was conducted within the RC-002 experimental environment used for the M4 normal-operation baseline.

The relevant M4 references are:

- N01 — ICMP Interzone
- N02 — HTTP Application
- N03 — Internet ICMP
- N04 — System Activity

### Internal connectivity comparison

The M4 N01 baseline recorded five successful internal ICMP trials from SERVER-1 (`192.168.20.10`) to TELEMETRY-1 (`192.168.30.10`), with zero packet loss in each trial.

All five A03 trials also passed the recorded internal ICMP connectivity checks.

This supports preservation of the tested internal connectivity function during the controlled TCP/22 denial condition.

### HTTP application comparison

The M4 N02 baseline recorded five successful HTTP requests to SERVER-1 on TCP/80, each returning HTTP 200.

All five A03 trials likewise recorded HTTP 200 during their operational checks.

These observations support continued availability of the tested HTTP application under the A03 experimental condition.

The checks do not establish identical response times or uninterrupted availability between measurements.

### External connectivity comparison

The M4 N03 baseline recorded five successful Internet ICMP trials to `8.8.8.8`, with zero packet loss. Baseline average RTT values ranged from 55.676 ms to 277.110 ms, and the highest recorded maximum RTT was 578.026 ms.

During A03-T01, the first external ICMP check returned one of three replies, representing approximately 66% packet loss. A repeat check returned three of three replies, with zero packet loss.

During A03-T04, the external ICMP average RTT was 152.122 ms and the maximum RTT was 262.432 ms. These measurements fall within the observed N03 RTT ranges.

The cause of the T01 packet-loss exception remains undetermined. The available evidence does not establish that the controlled firewall rule caused the loss.

### TCP/22 denial comparison

The M4 reference datasets do not provide a dedicated quantitative baseline for TCP/22 firewall-denial activity.

A03 nevertheless recorded the controlled experimental behavior directly: 25 connection attempts timed out during enforcement, while firewall counters and packet captures showed corresponding dropped packets, initial SYNs, and retransmissions.

The experiment therefore establishes repeatable controlled denial behavior, but not a quantified increase in denied TCP/22 attempts relative to a measured M4 denial baseline.

---

## 6. Operational Preservation

The A03 experimental condition selectively denied the tested TCP/22 connections.

Across all five trials:

- Internal ICMP connectivity checks passed.
- TCP/514 reachability checks passed.
- UDP/514 collector receipt was confirmed.
- HTTP checks returned HTTP 200.
- External ICMP connectivity was ultimately confirmed, including the successful repeat check in T01.

A03-T01 was classified as `PASS_WITH_EXCEPTION` because the initial external ICMP test recorded approximately 66% packet loss.

A03-T04 recorded elevated external ICMP latency relative to some observations, but its recorded average and maximum RTT values were within the ranges present in the M4 N03 baseline.

After A03-T05, the experimental firewall DROP rule was removed and TCP/22 connectivity was confirmed restored.

The results support operational preservation of the principal tested functions within the scope of the recorded checks. They do not establish continuous availability, zero performance impact, or the absence of transient network errors.

---

## 7. H2 Relevance

A03 provides evidence relevant to RC-002 Hypothesis H2.

The controlled TCP/22 denial condition produced repeatable and observable network behavior:

- 25 connection attempts timed out across five trials.
- Firewall counters recorded 50 dropped packets and 3,000 dropped bytes.
- Packet observations recorded 25 initial TCP SYNs and 25 SYN retransmissions.
- No successful TCP/22 connection was recorded during enforcement.
- TCP/22 connectivity was restored after removal of the experimental rule.

The firewall DROP packet count includes retransmissions and must not be interpreted as 50 distinct connection attempts.

These observations demonstrate that the selected packet and firewall telemetry can characterize a predefined denied-access condition.

However, the M4 datasets do not establish a directly comparable normal-operation TCP/22 denial rate. A03 therefore supports the observability of the controlled behavior without independently establishing a quantified baseline-relative increase in denial activity.

The observed behavior is not, by itself, evidence of malicious activity or automated threat detection.

---

## 8. Measurement Limitations and Exceptions

**T01 external ICMP exception:** The first external ICMP test recorded approximately 66% packet loss. A repeat test recorded zero packet loss. The root cause was not established.

**T04 external latency:** Average RTT was 152.122 ms, with a maximum of 262.432 ms. The measurements do not independently establish experiment-induced degradation.

**Firewall counter interpretation:** The 50 observed DROP packets comprise 25 initial SYN packets and 25 retransmissions, rather than 50 distinct connection attempts.

**Baseline limitation:** M4 does not contain a dedicated quantitative reference for TCP/22 firewall-denial activity.

**Operational observation scope:** Successful connectivity and application checks establish the tested functions' status at the recorded observation points, not uninterrupted operation throughout every trial.

**Causal attribution:** The available evidence does not establish the cause of the T01 external packet-loss exception.

These limitations are retained to avoid overstating the findings.

---

## 9. Evidence and Reproducibility

The principal derived dataset is:

`datasets/M5-Controlled-Abnormal-Behavior/A03-Controlled-Denied-Access/A03-Trial-Summary.csv`

The dataset contains five trial records covering connection attempts, timeouts, firewall counters, TCP SYN observations, connectivity checks, collector receipt, HTTP responses, operational classifications, and trial-specific notes.

The experimental results should be interpreted alongside the retained trial evidence and the M4 normal-operation datasets.

This section and Sections 5–8 were completed after identification of the original report's truncation. The additions document analysis of existing evidence; they do not represent new experimental trials or retrospective changes to trial measurements.

---

## 10. Conclusion

A03 completed five controlled denied-access trials.

All 25 TCP/22 connection attempts timed out during enforcement. Firewall counters and packet observations consistently recorded the controlled denial behavior, including SYN retransmissions.

The principal tested network functions remained available at the recorded operational checkpoints, subject to the T01 external ICMP exception and the documented measurement limitations.

A03 provides reproducible evidence that selected telemetry can expose controlled denied-access behavior. It does not independently quantify the increase in denial activity relative to an M4 TCP/22 baseline or demonstrate automated attack detection.

The experimental DROP rule was removed after the final trial, and TCP/22 connectivity was confirmed restored.

**Scenario execution status:** COMPLETE

**Results documentation status:** COMPLETE — repaired following identification of the originally truncated report.

**Milestone status:** Pending consolidated M5 verification.