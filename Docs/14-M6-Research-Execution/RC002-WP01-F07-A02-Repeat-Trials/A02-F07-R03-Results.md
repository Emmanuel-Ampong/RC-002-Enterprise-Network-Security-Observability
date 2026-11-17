# RC-002 — WP01-F07: A02 Quantitative Repeat Trial R03

**Project:** Project Aegis — Enterprise Network Security Observability  
**Experiment:** A02 — Increased Destination and Service Diversity  
**Trial ID:** A02-F07-R03  
**Execution Date:** November 17, 2026  
**Status:** Completed — Packet Evidence Verified; Timestamp Limitation Identified

## 1. Objective

Conduct the third controlled quantitative repeat trial of A02 to assess the consistency of destination and service diversity measurements.

This experiment contributes new evidence toward WP01-F07 remediation. It does not replace or retrospectively reconstruct the original A02 trial data.

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

Before the experiment, a single ICMP connectivity probe was executed to verify that the capture was recording traffic. Both request and reply packets were observed.

The following controlled experimental sequence was subsequently executed:

1. Transmit `RC002-F07-R03-START` via UDP/514.
2. Generate three ICMP echo requests to TELEMETRY-1.
3. Verify TCP/22 connectivity.
4. Verify TCP/514 connectivity.
5. Transmit `RC002-F07-R03-UDP514`.
6. Generate three ICMP echo requests to 8.8.8.8.
7. Transmit `RC002-F07-R03-END`.

The START and END marker packets establish the quantitative analysis window. The pretrial ICMP probe is excluded.

## 4. Quantitative Results

| Metric | Verified Result |
|---|---|
| START frame | 14 |
| UDP514 marker frame | 35 |
| END frame | 42 |
| Captured packets | 29 |
| Captured bytes | 2,347 |
| START timestamp (PCAP UTC) | 2026-11-17 06:12:13.688001 |
| END timestamp (PCAP UTC) | 2026-11-17 06:12:25.889948 |
| Experimental duration | 12.202 seconds |
| Internal ICMP packet loss | 0% |
| External ICMP packet loss | 0% |
| Internal average RTT | 2.926 ms |
| External average RTT | 95.516 ms |
| TCP/22 reachability | PASS |
| TCP/514 reachability | PASS |
| Syslog markers found at collector | 3/3 |

The experimental duration is calculated from the timestamps of the START and END packets.

The packet count and byte total include all captured traffic between frames 14 and 42, inclusive.

TShark reported a full capture-session duration of 308.2 seconds, which is distinct from the marker-defined experimental duration.

## 5. Protocol Distribution

| Protocol | Packets | Captured Bytes |
|---|---:|---:|
| ICMP | 12 | 1,176 |
| TCP | 14 | 986 |
| UDP | 3 | 185 |
| ARP | 0 | 0 |
| **Total** | **29** | **2,347** |

All three UDP packets were decoded as Syslog. One TCP packet was decoded as SSH.

The absence of ARP traffic within the marker-defined observation window does not, by itself, indicate experimental failure.

## 6. Destination and Service Verification

| Destination | Protocol / Service | Captured Frames |
|---|---|---|
| 192.168.30.10 | ICMP | 15–20 |
| 192.168.30.10 | TCP/22 | 21–25, 33–34 |
| 192.168.30.10 | TCP/514 | 26–32 |
| 192.168.30.10 | UDP/514 | 14, 35, 42 |
| 8.8.8.8 | ICMP | 36–41 |

The capture confirms successful generation of the intended internal and external traffic.

The TCP checks demonstrate service-port reachability, not completed application-level SSH or Syslog sessions.

## 7. Syslog Collector Verification

TELEMETRY-1 recorded all three R03 markers in:

`/var/log/remote/192.168.20.10/syslog.log`

| Marker | Collector Timestamp (UTC) |
|---|---|
| RC002-F07-R03-START | 2026-11-17 02:28:20.054629 |
| RC002-F07-R03-UDP514 | 2026-11-17 02:28:26.173023 |
| RC002-F07-R03-END | 2026-11-17 02:28:32.255813 |

The matching marker identifiers establish successful receipt and logging of all three transmitted UDP/514 messages.

**Timestamp qualification:** The collector timestamps differ substantially from their corresponding PCAP timestamps. They must not be treated as synchronized observations.

## 8. Timestamp Synchronization Investigation

### 8.1 Discrepancy Identified

| Marker | PCAP UTC | Collector UTC |
|---|---|---|
| START | 06:12:13.688001 | 02:28:20.054629 |
| UDP514 | 06:12:19.803673 | 02:28:26.173023 |
| END | 06:12:25.889948 | 02:28:32.255813 |

The corresponding markers show a nearly constant timestamp difference of approximately **3 hours, 43 minutes, and 54 seconds**.

### 8.2 Diagnostic Evidence

The investigation established the following:

- Windows Time service was stopped and configured for manual startup.
- Windows was configured to use `time.windows.com` for NTP synchronization.
- A read-only Windows NTP comparison against `185.125.190.58` returned offsets of +17.269 ms, +15.853 ms, and −2.585 ms.
- TELEMETRY-1 reported an active NTP service using `ntp.ubuntu.com`.
- TELEMETRY-1 initially displayed a system clock inconsistent with the Windows host.
- TELEMETRY-1's NTP logs documented repeated synchronization request timeouts.
- The journal subsequently recorded contact with `185.125.190.58` at 06:27:07 UTC on November 17.
- A later TELEMETRY-1 clock check reported 06:33:49 UTC.
- The later NTP status reported an offset of −43.850 ms, but the jitter remained unusually high at approximately 1 hour, 24 minutes, and 37 seconds.

### 8.3 Interpretation

The evidence is consistent with clock synchronization instability affecting TELEMETRY-1 during the R03 experiment.

However, the exact cause, timing, and magnitude of any clock correction have not been independently established.

Consequently:

1. Original packet and collector timestamps are retained without modification.
2. Cross-system delivery latency must not be calculated directly from these timestamps.
3. Packet counts and captured-byte measurements remain valid for the selected PCAP frames.
4. The marker-defined experimental duration remains valid as a within-PCAP measurement, assuming the capture clock did not undergo a material adjustment during the short observation window.
5. Further clock validation is recommended before future experiments requiring cross-system event correlation.

This limitation is relevant to Project Aegis's broader network security observability and detection research objectives.

## 9. Comparison Across Three Repeat Trials

| Metric | R01 | R02 | R03 |
|---|---:|---:|---:|
| Captured packets | 33 | 33 | 29 |
| Captured bytes | 2,629 | 2,569 | 2,347 |
| Experimental duration | 12.176 s | 12.421 s | 12.202 s |
| ICMP packets | 12 | 12 | 12 |
| TCP packets | 14 | 16 | 14 |
| UDP packets | 5 | 3 | 3 |
| ARP packets | 2 | 2 | 0 |
| Internal ICMP loss | 0% | 0% | 0% |
| External ICMP loss | 0% | 0% | 0% |
| Syslog markers recorded | 3/3 | 3/3 | 3/3 |

### Preliminary Observations

- All three repeat trials successfully generated internal and external connectivity traffic.
- Each trial recorded 12 ICMP packets.
- All three trials recorded their three Syslog markers.
- Differences in total packet counts reflect changes in other observed traffic, including ARP, TCP, and NTP.
- R03 recorded 29 packets, compared with 33 in R01 and R02.
- The three marker-defined experimental durations were relatively close.
- R03 exposed a material timestamp synchronization limitation that must be considered in subsequent research.

Three repeat trials provide preliminary consistency evidence but are insufficient to establish statistical reproducibility.

## 10. Evidence Integrity

**Capture filename:** `A02-F07-R03.pcap`

**Local evidence directory:** `C:\Users\Emma\Desktop\RC002-A02-Repeat-Captures`

**Integrity algorithm:** SHA-256

**SHA-256 fingerprint:**

`aa94eed932e384079c4059468e6c1852a2120e17f67933618b3b0be6fc43d239`

The fingerprint was generated successfully using Windows CertUtil.

The PCAP remains preserved locally and is not embedded in this research report.

## 11. Research Limitations

- Packet and byte totals include all captured traffic within the marker-defined observation window.
- Background traffic may influence quantitative measurements.
- TCP connectivity checks establish port reachability, not completed application-level sessions.
- The trial duration depends on the consistency of the capture clock during the observation window.
- Cross-system timestamp alignment was not reliable during R03.
- The root cause of the synchronization discrepancy remains unconfirmed.
- Only three quantitative repeat trials have been completed.
- New repeat-trial evidence cannot retrospectively restore missing measurements from the original A02 trials.

## 12. Preliminary Conclusion

A02-F07-R03 successfully reproduced the controlled destination and service diversity experiment.

The trial recorded **29 packets, 2,347 captured bytes, and a 12.202-second marker-defined observation window**, with all three UDP/514 markers subsequently identified in the collector's log.

The results contribute quantitative evidence toward WP01-F07 remediation while identifying an important clock synchronization limitation affecting cross-system event correlation.

**WP01-F07 remains OPEN** pending additional repeat trials, consolidated analysis, timestamp limitation documentation, and final evidence-audit review.