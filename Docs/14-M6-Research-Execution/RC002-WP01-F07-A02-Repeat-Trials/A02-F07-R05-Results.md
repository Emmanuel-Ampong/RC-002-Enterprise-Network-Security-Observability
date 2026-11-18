
# A02-F07-R05 — Quantitative Repeat Trial Results

## 1. Experiment Identification

| Field | Details |
|---|---|
| Project | Project Aegis |
| Research Component | RC-002 — Enterprise Network Security Observability |
| Milestone | M6 — Research Execution |
| Work Package | WP01 — Evidence Audit and Gap Resolution |
| Finding | F07 — Missing A02 Quantitative Measurements |
| Experiment | A02 — Increased Destination and Service Diversity |
| Repeat Trial | R05 |
| Execution Date | 2026-11-17 |
| Environment | GNS3 |
| Status | Completed |

## 2. Research Objective

The objective of R05 was to execute the fifth controlled quantitative repeat trial of A02 and obtain reproducible packet-level measurements of destination and service diversity within the RC-002 enterprise network environment.

This trial contributes to the remediation of finding WP01-F07, which identified missing quantitative measurements in the original A02 experimental evidence.

R05 constitutes newly generated evidence and does not replace or retrospectively reconstruct measurements from the original A02 trials.

## 3. Experimental Environment

| Component | Configuration |
|---|---|
| Traffic Source | SERVER-1 |
| Source IP Address | 192.168.20.10 |
| Internal Destination | TELEMETRY-1 |
| Internal Destination IP | 192.168.30.10 |
| External Destination IP | 8.8.8.8 |
| Capture Location | SERVER-1 to SERVER-SW1 |
| Packet Analysis Tool | TShark |
| Log Collection Service | rsyslog |
| Internal Services | ICMP, TCP/22, TCP/514, UDP/514 |
| External Protocol | ICMP |

## 4. Experimental Procedure

The following controlled traffic sequence was executed from SERVER-1:

1. Transmit the R05 START marker through UDP/514.
2. Generate three ICMP echo requests to TELEMETRY-1.
3. Verify TCP/22 connectivity.
4. Verify TCP/514 connectivity.
5. Transmit the R05 UDP514 marker.
6. Generate three ICMP echo requests to 8.8.8.8.
7. Transmit the R05 END marker.
8. Stop and preserve the packet capture.
9. Extract marker-defined quantitative measurements.
10. Verify collector receipt and generate a SHA-256 fingerprint.

## 5. Pretrial Validation

Internal connectivity was verified before experimental execution.

| Metric | Result |
|---|---|
| Packets Transmitted | 1 |
| Packets Received | 1 |
| Packet Loss | 0% |
| Round-Trip Time | 3.877 ms |
| Connectivity Status | PASS |

Packet capture functionality was independently verified through the presence of an ICMP echo request and corresponding echo reply in frames 1 and 2.

These pretrial packets were excluded from the marker-defined experimental observation window.

## 6. Experimental Observation Window

The observation window was established using the START and END markers identified within the preserved packet capture.

| Marker | Frame Number | Timestamp (UTC) |
|---|---:|---|
| RC002-F07-R05-START | 7 | 2026-11-17T22:07:41.623201Z |
| RC002-F07-R05-UDP514 | 30 | 2026-11-17T22:07:47.715354Z |
| RC002-F07-R05-END | 39 | 2026-11-17T22:07:53.885298Z |

- Starting Frame: 7
- Ending Frame: 39
- Observation Window: Inclusive
- Experimental Duration: 12.262097 seconds

The full capture session reported by TShark was approximately 173.3 seconds. This represents the capture-session duration rather than the marker-defined experimental duration.

## 7. Quantitative Results

| Metric | Observed Value |
|---|---:|
| Total Captured Packets | 33 |
| Total Frame Bytes | 2551 |
| Experimental Duration | 12.262 seconds |
| IPv4 Packets | 29 |
| IPv4 Frame Bytes | 2347 |
| ARP Packets | 4 |
| ARP Frame Bytes | 204 |

The measurements include all frames within the experimental observation window, including observed infrastructure traffic.

## 8. Protocol Distribution

| Protocol | Packet Count | Frame Bytes |
|---|---:|---:|
| ICMP | 12 | 1176 |
| TCP | 14 | 986 |
| UDP | 3 | 185 |
| ARP | 4 | 204 |
| Total | 33 | 2551 |

Additional protocol observations:

- All three UDP packets were decoded as Syslog.
- One TCP packet was decoded as SSH.
- Four ARP packets were observed within the experimental window.
- The protocol-level packet counts and byte totals reconcile with the aggregate measurements.

SSH and Syslog are nested protocol classifications and are not counted separately in the aggregate totals.

## 9. Destination and Service Verification

| Destination | Protocol or Service | Observed Frames | Status |
|---|---|---|---|
| 192.168.30.10 | ICMP | 8–11, 14–15 | PASS |
| 192.168.30.10 | TCP/22 | 16–20, 28–29 | PASS |
| 192.168.30.10 | TCP/514 | 21–27 | PASS |
| 192.168.30.10 | UDP/514 | 7, 30, 39 | PASS |
| 8.8.8.8 | ICMP | 33–38 | PASS |

The TCP frame ranges include bidirectional communication between SERVER-1 and TELEMETRY-1.

Both internal and external ICMP exchanges were successfully captured.

## 10. Connectivity Measurements

### 10.1 Internal ICMP Connectivity

Destination: 192.168.30.10

| Metric | Result |
|---|---:|
| Packets Transmitted | 3 |
| Packets Received | 3 |
| Packet Loss | 0% |
| Minimum RTT | 2.327 ms |
| Average RTT | 3.181 ms |
| Maximum RTT | 3.848 ms |

### 10.2 External ICMP Connectivity

Destination: 8.8.8.8

| Metric | Result |
|---|---:|
| Packets Transmitted | 3 |
| Packets Received | 3 |
| Packet Loss | 0% |
| Minimum RTT | 133.587 ms |
| Average RTT | 142.574 ms |
| Maximum RTT | 154.148 ms |

### 10.3 TCP Service Connectivity

| Service | Exit Code | Status |
|---|---:|---|
| TCP/22 | 0 | PASS |
| TCP/514 | 0 | PASS |

The TCP connectivity tests establish successful connection initiation but do not independently demonstrate complete application-layer functionality.

## 11. Syslog Collector Verification

All three experimental markers were successfully located in the TELEMETRY-1 remote syslog file.

Collector Log Location:

`/var/log/remote/192.168.20.10/syslog.log`

| Marker | Collector Timestamp (UTC) |
|---|---|
| RC002-F07-R05-START | 2026-11-17T22:07:42.647503Z |
| RC002-F07-R05-UDP514 | 2026-11-17T22:07:48.741398Z |
| RC002-F07-R05-END | 2026-11-17T22:07:54.906470Z |

| Metric | Result |
|---|---:|
| Expected Markers | 3 |
| Observed Markers | 3 |
| Marker Receipt Rate | 100% |

Collector Verification Status: PASS.

The receipt rate applies specifically to the three controlled experimental markers and should not be interpreted as a general syslog delivery reliability measurement.

## 12. Cross-System Timestamp Analysis

| Marker | PCAP Timestamp (UTC) | Collector Timestamp (UTC) | Observed Difference |
|---|---|---|---:|
| START | 22:07:41.623201 | 22:07:42.647503 | 1.024302 s |
| UDP514 | 22:07:47.715354 | 22:07:48.741398 | 1.026044 s |
| END | 22:07:53.885298 | 22:07:54.906470 | 1.021172 s |

The collector timestamps were consistently approximately 1.02 seconds later than the corresponding packet-capture timestamps.

This observation differs from R04, where the timestamp differences were approximately 32–35 milliseconds.

The observed differences may reflect clock synchronization variation, processing delays, collector timestamping behavior, or a combination of these factors.

The underlying cause was not independently established.

Consequently:

- Original timestamps are preserved without modification.
- Cross-system timestamp differences are not interpreted as network delivery latency.
- Experimental duration is calculated exclusively from timestamps within the same packet capture.
- Further clock synchronization validation is recommended before latency-sensitive cross-system analysis.

## 13. Comparison with Previous Repeat Trials

| Metric | R01 | R02 | R03 | R04 | R05 |
|---|---:|---:|---:|---:|---:|
| Total Packets | 33 | 33 | 29 | 33 | 33 |
| Total Frame Bytes | 2629 | 2569 | 2347 | 2551 | 2551 |
| Duration (s) | 12.176 | 12.421 | 12.202 | 12.205 | 12.262 |
| ICMP Packets | 12 | 12 | 12 | 12 | 12 |
| TCP Packets | 14 | 16 | 14 | 14 | 14 |
| UDP Packets | 5 | 3 | 3 | 3 | 3 |
| ARP Packets | 2 | 2 | 0 | 4 | 4 |
| Syslog Markers Received | 3/3 | 3/3 | 3/3 | 3/3 | 3/3 |

### Preliminary Observations

- All five repeat trials generated exactly 12 ICMP packets.
- All five repeat trials demonstrated internal and external ICMP connectivity.
- All five repeat trials produced evidence of TCP/22 and TCP/514 connectivity.
- All 15 controlled UDP/514 markers across the five repeat trials were observed by the collector.
- R04 and R05 produced identical protocol-level packet counts and aggregate frame-byte totals.
- Differences in TCP, UDP, and ARP activity account for observed variation in aggregate packet counts.
- The five experimental durations remained within a relatively narrow interval.

These observations support preliminary repeatability of the controlled traffic-generation procedure.

Identical aggregate measurements do not establish packet-level identity or identical network state.

## 14. Evidence Preservation and Integrity

| Field | Details |
|---|---|
| Evidence Filename | A02-F07-R05.pcap |
| Evidence Directory | RC002-A02-Repeat-Captures |
| Evidence Format | PCAP |
| Hash Algorithm | SHA-256 |
| Integrity Fingerprint Generation | PASS |

### SHA-256 Fingerprint

```text
5e8c2683239419ba5e3cda5926810893062648a59d2ddfcc7f9e84b9fe6bd7ab
```

The fingerprint was generated using Windows CertUtil after the completed packet capture was preserved.

The original PCAP is retained in the dedicated local repeat-trial evidence directory.

## 15. Experimental Limitations

1. Measurements are limited to the SERVER-1 capture interface.
2. Observed aggregate measurements include infrastructure traffic within the marker-defined observation window.
3. TCP connectivity checks do not establish complete application-layer functionality.
4. Cross-system clock differences prevent reliable inference of one-way network or log delivery latency.
5. External ICMP round-trip measurements may be influenced by uncontrolled external network conditions.
6. The controlled repeat procedure does not independently reproduce every HTTP or HTTPS activity associated with the original A02 experiment.
7. The repeat-trial evidence must remain distinguishable from the original A02 evidence.
8. Five trials provide preliminary repeatability evidence but do not establish broader statistical generalizability.

## 16. Conclusion

A02-F07-R05 successfully completed the fifth planned quantitative repeat trial.

The experiment generated 33 captured packets and 2551 frame bytes within a marker-defined observation window of approximately 12.262 seconds.

Internal and external ICMP connectivity, TCP service reachability, UDP/514 transmission, and successful collector receipt were verified.

The resulting evidence demonstrates that the controlled destination and service diversity sequence can be observed and quantitatively characterized within the RC-002 experimental environment.

R05 completes the planned five-trial repeat execution for WP01-F07.

Formal resolution of WP01-F07 remains subject to consolidated analysis, documentation, traceability updates, and evidence publication.

## 17. Final Trial Assessment

| Validation Criterion | Status |
|---|---|
| Pretrial Connectivity | PASS |
| Packet Capture Verification | PASS |
| Experimental Execution | PASS |
| Experimental Marker Verification | PASS |
| Quantitative Measurement Extraction | PASS |
| Protocol Distribution Analysis | PASS |
| Destination and Service Verification | PASS |
| Syslog Collector Verification | PASS |
| SHA-256 Fingerprint Generation | PASS |
| Cross-System Timestamp Analysis | PASS WITH LIMITATION |
| Overall Trial Assessment | PASS WITH DOCUMENTED LIMITATIONS |

---

Project Aegis — RC-002 Enterprise Network Security Observability

M6 / WP01 / F07 / A02-R05

End of Research Record.
