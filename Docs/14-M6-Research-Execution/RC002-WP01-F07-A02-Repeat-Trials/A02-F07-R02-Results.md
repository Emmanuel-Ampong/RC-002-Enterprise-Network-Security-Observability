# RC-002 — WP01-F07: A02 Quantitative Repeat Trial R02

**Project:** Project Aegis — Enterprise Network Security Observability  
**Experiment:** A02 — Increased Destination and Service Diversity  
**Trial ID:** A02-F07-R02  
**Execution Date:** November 17, 2026  
**Status:** Completed — Evidence Verified

## 1. Objective

Conduct a second controlled quantitative repeat trial to evaluate the consistency and reproducibility of the A02 destination and service diversity experiment.

This trial addresses evidence limitations identified under WP01-F07 and uses the same experimental traffic-generation procedure as A02-F07-R01.

R02 represents newly collected evidence and does not replace or retroactively reconstruct the original A02 trials.

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

A controlled traffic sequence was executed from SERVER-1:

1. Transmit `RC002-F07-R02-START` via UDP/514.
2. Generate three ICMP echo requests to TELEMETRY-1.
3. Verify TCP/22 connectivity.
4. Verify TCP/514 connectivity.
5. Transmit `RC002-F07-R02-UDP514` via UDP/514.
6. Generate three ICMP echo requests to 8.8.8.8.
7. Transmit `RC002-F07-R02-END` via UDP/514.

The START and END markers establish identifiable experimental boundaries within the packet capture.

## 4. Quantitative Results

| Metric | Verified Result |
|---|---|
| START frame | 10 |
| UDP514 marker frame | 32 |
| END frame | 42 |
| Captured packets | 33 |
| Captured bytes | 2,569 |
| START timestamp (UTC) | 2026-11-17 01:45:14.812671 |
| END timestamp (UTC) | 2026-11-17 01:45:27.233192 |
| Experimental duration | 12.421 seconds |
| Internal ICMP packet loss | 0% |
| External ICMP packet loss | 0% |
| TCP/22 reachability | PASS |
| TCP/514 reachability | PASS |
| Syslog markers received | 3/3 |

The experimental duration was calculated using the difference between the START and END packet timestamps.

The packet count and captured-byte total include all traffic recorded between frames 10 and 42, including marker packets and background traffic.

## 5. Protocol Distribution

| Protocol | Packets | Captured Bytes |
|---|---:|---:|
| ICMP | 12 | 1,176 |
| TCP | 16 | 1,106 |
| UDP | 3 | 185 |
| ARP | 2 | 102 |
| **Total** | **33** | **2,569** |

TShark's protocol hierarchy identified three Syslog packets and one SSH packet. These are included within the UDP and TCP totals respectively.

## 6. Destination and Service Verification

The packet capture confirms the following traffic:

| Destination | Protocol / Service | Evidence |
|---|---|---|
| 192.168.30.10 | ICMP | Frames 11–16 |
| 192.168.30.10 | TCP/22 | Frames 17–21 and 29–33 |
| 192.168.30.10 | TCP/514 | Frames 22–28 |
| 192.168.30.10 | UDP/514 | Frames 10, 32 and 42 |
| 8.8.8.8 | ICMP | Frames 36–41 |

These observations establish the presence of internal and external destination traffic and multiple service protocols.

## 7. Syslog Collector Verification

TELEMETRY-1 successfully received and recorded all three R02 experimental markers.

| Marker | Collector Timestamp (UTC) |
|---|---|
| RC002-F07-R02-START | 01:45:15.040727 |
| RC002-F07-R02-UDP514 | 01:45:21.121698 |
| RC002-F07-R02-END | 01:45:27.461318 |

**Collector log:** `/var/log/remote/192.168.20.10/syslog.log`

The collector entries independently verify successful UDP/514 message receipt and logging.

## 8. Comparison with A02-F07-R01

| Metric | R01 | R02 |
|---|---:|---:|
| Captured packets | 33 | 33 |
| Captured bytes | 2,629 | 2,569 |
| Experimental duration | 12.176 s | 12.421 s |
| ICMP packets | 12 | 12 |
| TCP packets | 14 | 16 |
| UDP packets | 5 | 3 |
| ARP packets | 2 | 2 |
| Internal ICMP loss | 0% | 0% |
| External ICMP loss | 0% | 0% |
| Syslog markers received | 3/3 | 3/3 |

### Preliminary Observations

- Both trials recorded exactly 33 packets.
- R02 captured 60 fewer bytes than R01.
- The R02 observation window was approximately 0.245 seconds longer.
- Both trials recorded 12 ICMP packets and two ARP packets.
- R01 included two NTP packets not present within the R02 experimental window.
- Both trials confirmed all three syslog markers at the collector.

These findings indicate consistency in several measured outcomes but are insufficient to establish statistical reproducibility.

## 9. Evidence Integrity

**Capture filename:** `A02-F07-R02.pcap`

**Local evidence directory:** `C:\Users\Emma\Desktop\RC002-A02-Repeat-Captures`

**Hash algorithm:** SHA-256

**SHA-256 fingerprint:**

`95f1b2be50c7f6761f54179458366564ac2b3ee6901f2e44f3d48f10831ea400`

The fingerprint was generated successfully using Windows CertUtil.

The PCAP is preserved locally and is not included in this Markdown documentation file.

## 10. Research Limitations

- The experimental packet totals include background network traffic.
- The observation window is defined by the START and END marker packets rather than independent instrumentation.
- TCP connectivity checks establish port reachability but do not demonstrate completed application-level sessions.
- Packet counts and captured bytes are specific to the selected capture point.
- Two repeat trials are insufficient to establish statistical reproducibility.
- The repeat trials cannot reconstruct missing evidence from the original A02 trials.
- Observed external ICMP latency variation requires further investigation before attributing causes.

## 11. Preliminary Conclusion

A02-F07-R02 successfully reproduced the destination and service diversity tests conducted during R01.

The experiment recorded:

- 33 captured packets
- 2,569 captured bytes
- A 12.421-second marker-defined observation window
- Successful internal and external ICMP connectivity
- Successful TCP/22 and TCP/514 reachability checks
- Three verified UDP/514 syslog marker receipts

The results provide additional quantitative evidence supporting WP01-F07 remediation.

**WP01-F07 remains OPEN** pending further controlled repeat trials, consolidated analysis, and completion of the evidence-audit documentation.