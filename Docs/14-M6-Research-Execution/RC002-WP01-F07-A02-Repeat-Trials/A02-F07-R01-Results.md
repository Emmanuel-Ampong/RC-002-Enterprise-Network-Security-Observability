# RC-002 — WP01-F07: A02 Quantitative Repeat Trial R01

**Project:** Project Aegis — Enterprise Network Security Observability  
**Experiment:** A02 — Increased Destination and Service Diversity  
**Trial ID:** A02-F07-R01  
**Execution Date:** November 16, 2026  
**Status:** Completed — Evidence Verified

## 1. Objective

Produce reproducible quantitative evidence for the A02 destination and service diversity experiment, addressing the limitations identified under WP01-F07.

This is a new repeat trial and does not replace or retroactively validate the original A02 trial measurements.

## 2. Experimental Environment

- **Source:** SERVER-1 — 192.168.20.10
- **Internal Destination:** TELEMETRY-1 — 192.168.30.10
- **External Test Destination:** 8.8.8.8
- **Capture Point:** SERVER-1 ↔ SERVER-SW1
- **Capture Method:** GNS3 packet capture
- **Analysis Tool:** Wireshark TShark
- **Syslog Transport:** UDP/514

## 3. Experimental Procedure

A controlled traffic sequence was executed from SERVER-1:

1. Transmit the START marker via UDP/514.
2. Perform three internal ICMP echo requests.
3. Verify TCP/22 connectivity.
4. Verify TCP/514 connectivity.
5. Transmit the UDP514 marker.
6. Perform three external ICMP echo requests.
7. Transmit the END marker via UDP/514.

START and END markers established identifiable boundaries for quantitative analysis.

## 4. Quantitative Results

| Metric | Result |
|---|---|
| START frame | 108 |
| END frame | 140 |
| Total captured packets | 33 |
| Total captured bytes | 2,629 |
| START timestamp (UTC) | 19:08:12.932148 |
| END timestamp (UTC) | 19:08:25.108306 |
| Experimental duration | 12.176 seconds |
| Internal ICMP packet loss | 0% |
| External ICMP packet loss | 0% |
| TCP/22 reachability | PASS |
| TCP/514 reachability | PASS |
| Syslog markers received | 3/3 |

The reported packet count and byte total include both marker frames and all other traffic observed within the selected frame window.

## 5. Protocol Distribution

| Protocol | Packets | Captured Bytes |
|---|---:|---:|
| ICMP | 12 | 1,176 |
| TCP | 14 | 986 |
| UDP | 5 | 365 |
| ARP | 2 | 102 |
| **Total** | **33** | **2,629** |

Additional protocol decoding identified three Syslog packets, two NTP packets, and one SSH packet. These are included in the TCP/UDP totals.

## 6. Destination and Service Verification

The packet capture confirms experimental traffic involving:

- 192.168.30.10 — ICMP
- 192.168.30.10 — TCP/22
- 192.168.30.10 — TCP/514
- 192.168.30.10 — UDP/514
- 8.8.8.8 — ICMP

Additional NTP traffic involving 162.159.200.1 was observed but was not part of the intentionally generated test sequence.

## 7. Syslog Collector Verification

TELEMETRY-1 independently recorded all three experimental markers.

| Marker | Collector Timestamp (UTC) |
|---|---|
| RC002-F07-R01-START | 19:08:12.974764 |
| RC002-F07-R01-UDP514 | 19:08:19.054942 |
| RC002-F07-R01-END | 19:08:25.148549 |

**Collector log:** `/var/log/remote/192.168.20.10/syslog.log`

This confirms receipt and logging of the UDP/514 markers.

## 8. Evidence Integrity

**Capture filename:** `A02-F07-R01.pcap`

**Local evidence directory:** `C:\Users\Emma\Desktop\RC002-A02-Repeat-Captures`

**SHA-256:**

`4c1b29295087f017033d6d5670d28cc50d61ead5dd382f658dfd31f9a5c74a71`

The hash was generated successfully using Windows CertUtil.

## 9. Research Limitations

- The observation window includes background network traffic, including NTP and ARP.
- Packet and byte totals represent all captured frames between the START and END markers, not exclusively experiment-generated traffic.
- The full capture-session duration reported by TShark is not the experimental duration.
- TCP connectivity results confirm service-port reachability rather than completed application-level sessions.
- This repeat trial provides new evidence but cannot reconstruct measurements absent from the original A02 trials.
- Additional repeat trials are required before assessing consistency and reproducibility across trials.

## 10. Preliminary Conclusion

A02-F07-R01 successfully demonstrated measurable internal and external connectivity, multiple protocols and service ports, and verified syslog collector receipt.

The experiment produced 33 captured packets, 2,629 captured bytes, and a 12.176-second marker-defined observation window.

The resulting measurements provide a reproducible baseline for subsequent A02 repeat trials. WP01-F07 remains open pending further trials and consolidated research documentation.