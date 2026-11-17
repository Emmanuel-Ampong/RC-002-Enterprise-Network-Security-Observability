# RC-002 — WP01-F07: A02 Quantitative Repeat Trial R04

**Project:** Project Aegis — Enterprise Network Security Observability  
**Experiment:** A02 — Increased Destination and Service Diversity  
**Trial ID:** A02-F07-R04  
**Execution Date:** November 17, 2026  
**Status:** Completed — Packet Evidence and Collector Receipt Verified

## 1. Objective

Conduct the fourth controlled quantitative repeat trial of A02 to evaluate the consistency of network traffic measurements across repeated executions.

The experiment contributes new quantitative evidence toward remediation of WP01-F07, which identified missing packet-count, captured-byte, and duration measurements in the original A02 experiments.

R04 represents newly collected evidence and does not retrospectively reconstruct the original A02 trial data.

## 2. Experimental Environment

- **Source Host:** SERVER-1 — 192.168.20.10
- **Internal Destination:** TELEMETRY-1 — 192.168.30.10
- **External Destination:** 8.8.8.8
- **Capture Point:** SERVER-1 ↔ SERVER-SW1
- **Capture Method:** GNS3 packet capture
- **Analysis Tool:** Wireshark TShark
- **Syslog Collector:** rsyslog on TELEMETRY-1
- **Syslog Transport:** UDP/514

## 3. Experimental Procedure

Before the trial, a single ICMP request was transmitted from SERVER-1 to TELEMETRY-1 to verify packet capture functionality.

The pretrial request and reply were identified in frames 7 and 8 and excluded from the experimental observation window.

The controlled R04 sequence consisted of:

1. Transmitting `RC002-F07-R04-START` via UDP/514.
2. Generating three ICMP echo requests to TELEMETRY-1.
3. Verifying TCP/22 reachability.
4. Verifying TCP/514 reachability.
5. Transmitting `RC002-F07-R04-UDP514`.
6. Generating three ICMP echo requests to 8.8.8.8.
7. Transmitting `RC002-F07-R04-END` via UDP/514.

The START and END marker frames define the experimental measurement window.

## 4. Quantitative Results

| Metric | Verified Result |
|---|---|
| START frame | 11 |
| UDP514 marker frame | 34 |
| END frame | 43 |
| Captured packets | 33 |
| Captured bytes | 2,551 |
| START timestamp (PCAP UTC) | 2026-11-17 06:56:51.767891 |
| END timestamp (PCAP UTC) | 2026-11-17 06:57:03.972725 |
| Experimental duration | 12.205 seconds |
| Internal ICMP packet loss | 0% |
| External ICMP packet loss | 0% |
| Internal average RTT | 2.495 ms |
| External average RTT | 60.066 ms |
| TCP/22 reachability | PASS |
| TCP/514 reachability | PASS |
| Syslog markers recorded | 3/3 |

The experimental duration represents the elapsed time between the START and END packet timestamps.

The packet count and captured-byte total include all traffic recorded between frames 11 and 43, inclusive.

TShark reported a full capture-session duration of 124.1 seconds. This is distinct from the 12.205-second marker-defined experimental window.

## 5. Protocol Distribution

| Protocol | Packets | Captured Bytes |
|---|---:|---:|
| ICMP | 12 | 1,176 |
| TCP | 14 | 986 |
| UDP | 3 | 185 |
| ARP | 4 | 204 |
| **Total** | **33** | **2,551** |

All three UDP packets were decoded as Syslog.

One TCP packet was decoded as SSH. These protocol subcategories are already included in their respective totals.

Four ARP packets were also recorded within the experimental observation window.

## 6. Destination and Service Verification

The packet capture confirms the following traffic:

| Destination | Protocol / Service | Captured Frames |
|---|---|---|
| 192.168.30.10 | ICMP | 12–15, 18–19 |
| 192.168.30.10 | TCP/22 | 20–24, 32–33 |
| 192.168.30.10 | TCP/514 | 25–31 |
| 192.168.30.10 | UDP/514 | 11, 34, 43 |
| 8.8.8.8 | ICMP | 37–42 |

These observations verify that the intended internal and external destinations and service ports were represented during R04.

The TCP checks demonstrate service-port reachability rather than completed application-level sessions.

## 7. Syslog Collector Verification

TELEMETRY-1 successfully recorded all three experimental markers in:

`/var/log/remote/192.168.20.10/syslog.log`

| Marker | Collector Timestamp (UTC) |
|---|---|
| RC002-F07-R04-START | 2026-11-17 06:56:51.799736 |
| RC002-F07-R04-UDP514 | 2026-11-17 06:56:57.894524 |
| RC002-F07-R04-END | 2026-11-17 06:57:04.004639 |

These entries independently confirm that the collector received and recorded the three UDP/514 messages.

## 8. Timestamp Alignment Assessment

R03 previously revealed a substantial discrepancy between packet-capture timestamps and collector timestamps.

For R04, the corresponding marker timestamps were much more closely aligned.

| Marker | PCAP UTC | Collector UTC | Observed Difference |
|---|---|---|---:|
| START | 06:56:51.767891 | 06:56:51.799736 | +31.845 ms |
| UDP514 | 06:56:57.859619 | 06:56:57.894524 | +34.905 ms |
| END | 06:57:03.972725 | 06:57:04.004639 | +31.914 ms |

### Interpretation

All three R04 collector timestamps follow the corresponding capture timestamps by approximately 32–35 milliseconds.

This demonstrates substantially closer timestamp alignment than was observed during R03.

However, these differences must **not** be interpreted as precise network delivery latency without independently verifying synchronization accuracy and accounting for logging and processing delays.

The earlier R03 discrepancy remains a documented research limitation.

## 9. Comparison Across Four Repeat Trials

| Metric | R01 | R02 | R03 | R04 |
|---|---:|---:|---:|---:|
| Captured packets | 33 | 33 | 29 | 33 |
| Captured bytes | 2,629 | 2,569 | 2,347 | 2,551 |
| Experimental duration (s) | 12.176 | 12.421 | 12.202 | 12.205 |
| ICMP packets | 12 | 12 | 12 | 12 |
| TCP packets | 14 | 16 | 14 | 14 |
| UDP packets | 5 | 3 | 3 | 3 |
| ARP packets | 2 | 2 | 0 | 4 |
| Internal ICMP packet loss | 0% | 0% | 0% | 0% |
| External ICMP packet loss | 0% | 0% | 0% | 0% |
| Syslog markers recorded | 3/3 | 3/3 | 3/3 | 3/3 |

### Preliminary Findings

- All four trials successfully generated the intended internal and external connectivity traffic.
- Every trial recorded 12 ICMP packets.
- Every trial recorded its three UDP/514 experimental markers at the collector.
- R04 captured the same total number of packets as R01 and R02.
- R04 recorded four additional ARP packets compared with R03.
- Packet-count differences between trials include variations in ARP, TCP, and UDP traffic.
- The marker-defined observation durations were relatively close across the four trials.
- R04 exhibited substantially improved cross-system timestamp alignment compared with R03.

These observations support preliminary consistency but do not yet establish statistical reproducibility or explain all observed variations.

## 10. Evidence Integrity

**Capture filename:** `A02-F07-R04.pcap`

**Local evidence directory:** `C:\Users\Emma\Desktop\RC002-A02-Repeat-Captures`

**Integrity algorithm:** SHA-256

**SHA-256 fingerprint:**

`ce70c0bd1c186c0604c37c659563061611a9f4d70d4225fa472c3365212311b7`

The fingerprint was generated successfully using Windows CertUtil.

The original PCAP remains preserved locally and is not embedded in this Markdown report.

## 11. Research Limitations

- Captured packet and byte totals include background traffic within the experimental window.
- Observed ARP activity contributes to variation in total packet counts.
- The measurement window is defined by UDP marker packets rather than independent measurement instrumentation.
- TCP connectivity testing demonstrates port reachability, not completed application-level sessions.
- The experimental duration depends on the stability of the capture clock.
- Cross-system timestamp differences cannot be treated as precise network delivery latency.
- R03's earlier clock synchronization discrepancy remains relevant to the consolidated study.
- Four repeat trials do not establish statistical reproducibility.
- Repeat trials provide new evidence but cannot retrospectively restore measurements missing from the original A02 trials.

## 12. Preliminary Conclusion

A02-F07-R04 successfully reproduced the controlled destination and service diversity experiment.

The trial recorded **33 captured packets, 2,551 captured bytes, and a 12.205-second marker-defined observation window**.

All three UDP/514 markers were independently identified in TELEMETRY-1's syslog records.

The experiment also demonstrated substantially closer alignment between capture and collector timestamps than was observed in R03.

These findings provide additional quantitative evidence toward resolving WP01-F07.

**WP01-F07 remains OPEN** pending the fifth repeat trial, consolidated analysis, and final evidence-audit review.