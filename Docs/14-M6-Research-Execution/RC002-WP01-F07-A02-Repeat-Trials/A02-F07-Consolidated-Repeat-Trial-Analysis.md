
# A02-F07 — Consolidated Quantitative Repeat Trial Analysis

## 1. Research Identification

| Field | Details |
|---|---|
| Project | Project Aegis |
| Research Component | RC-002 — Enterprise Network Security Observability |
| Project Aegis Phase | OBSERVE |
| Milestone | M6 — Research Execution |
| Work Package | WP01 — Evidence Audit and Gap Resolution |
| Finding | F07 — Missing A02 Quantitative Measurements |
| Experiment | A02 — Increased Destination and Service Diversity |
| Repeat Trials | R01–R05 |
| Number of Trials | 5 |
| Experimental Environment | GNS3 |
| Execution Period | 2026-11-16 to 2026-11-17 |
| Analysis Date | 2026-11-18 |
| Analysis Status | Complete |

## 2. Background and Research Motivation

During the WP01 evidence audit, finding F07 identified missing quantitative packet-level measurements associated with the original A02 experiment.

Although the original experiment contained qualitative evidence of increased destination and service diversity, the available evidence was insufficient to reconstruct complete quantitative measurements for all original trials.

To address this gap, five controlled quantitative repeat trials were designed and executed.

The repeat trials constitute newly generated experimental evidence. They do not replace the original A02 evidence or retrospectively reconstruct its missing measurements.

This distinction preserves research integrity, experimental traceability, and transparency.

## 3. Research Objectives

The quantitative repeat-trial exercise was designed to:

1. Establish a controlled and repeatable traffic-generation procedure.
2. Verify internal and external destination reachability.
3. Verify TCP and UDP service diversity.
4. Quantify packet counts and frame-byte measurements.
5. Characterize protocol distribution across repeated executions.
6. Verify controlled syslog marker receipt.
7. Investigate measurement variability and timestamp discrepancies.
8. Preserve independently identifiable experimental evidence.
9. Assess whether the repeat-trial evidence sufficiently addresses WP01-F07.

## 4. Research Questions

### RQ1 — Experimental Observability

Can the RC-002 environment capture and quantitatively characterize traffic associated with multiple destinations and network services?

### RQ2 — Experimental Repeatability

Does repeated execution of a controlled traffic-generation procedure produce comparable quantitative measurements?

### RQ3 — Telemetry Collection

Can controlled UDP/514 experimental markers be observed in packet captures and independently verified at the telemetry collector?

### RQ4 — Measurement Integrity

What limitations affect the interpretation of packet-level measurements and cross-system timestamps?

## 5. Experimental Environment

| Component | Configuration |
|---|---|
| Traffic Source | SERVER-1 |
| Source IP Address | 192.168.20.10 |
| Internal Destination | TELEMETRY-1 |
| Internal Destination IP | 192.168.30.10 |
| External Destination IP | 8.8.8.8 |
| Packet Capture Location | SERVER-1 to SERVER-SW1 |
| Network Emulation Platform | GNS3 |
| Packet Analysis Tool | TShark |
| Telemetry Collection Service | rsyslog |
| Integrity Verification | SHA-256 |

### 5.1 Controlled Traffic Types

| Traffic Type | Destination | Protocol or Service |
|---|---|---|
| Internal connectivity | 192.168.30.10 | ICMP |
| SSH reachability | 192.168.30.10 | TCP/22 |
| Syslog service reachability | 192.168.30.10 | TCP/514 |
| Experimental markers | 192.168.30.10 | UDP/514 |
| External connectivity | 8.8.8.8 | ICMP |

ARP and other background traffic were not explicitly generated as controlled experimental stimuli but were included when observed within the experimental measurement window.

## 6. Experimental Methodology

Each repeat trial followed a common controlled traffic-generation sequence.

1. Transmit a uniquely identified START marker through UDP/514.
2. Generate three internal ICMP echo requests.
3. Verify TCP/22 connectivity.
4. Verify TCP/514 connectivity.
5. Transmit a uniquely identified UDP514 marker.
6. Generate three external ICMP echo requests.
7. Transmit a uniquely identified END marker.
8. Stop and preserve the packet capture.
9. Identify the experimental observation window.
10. Extract quantitative packet-level measurements.
11. Verify protocol, destination, and service distribution.
12. Verify experimental marker receipt at TELEMETRY-1.
13. Generate a SHA-256 fingerprint.
14. Document the experimental results.

### 6.1 Experimental Observation Window

For each trial, the observation window was defined by the captured START and END markers.

Both boundary frames were included in the measurements.

This approach reduces the influence of traffic captured before and after the controlled experimental sequence.

However, unrelated traffic occurring within the marker-defined window remains part of the aggregate measurements.

### 6.2 Statistical Methodology

The consolidated analysis uses descriptive statistics.

The following statistics are reported where applicable:

- Arithmetic mean.
- Median.
- Minimum.
- Maximum.
- Range.
- Sample standard deviation.
- Coefficient of variation.

Sample standard deviation is calculated using an n−1 denominator.

The coefficient of variation is calculated as:

CV (%) = (Sample Standard Deviation / Mean) × 100

The analysis uses five observations and does not attempt population-level inference or statistical significance testing.

Duration statistics are calculated using the trial durations reported to three decimal places.

## 7. Consolidated Experimental Dataset

| Trial | Captured Packets | Frame Bytes | Duration (s) | Markers Received |
|---|---:|---:|---:|---:|
| R01 | 33 | 2629 | 12.176 | 3/3 |
| R02 | 33 | 2569 | 12.421 | 3/3 |
| R03 | 29 | 2347 | 12.202 | 3/3 |
| R04 | 33 | 2551 | 12.205 | 3/3 |
| R05 | 33 | 2551 | 12.262 | 3/3 |
| Total | 161 | 12647 | 61.266 | 15/15 |

The combined duration represents the sum of five independent marker-defined observation windows, not a continuous experimental session.

## 8. Descriptive Statistical Analysis

| Statistic | Packets | Frame Bytes | Duration (s) |
|---|---:|---:|---:|
| Number of Observations | 5 | 5 | 5 |
| Mean | 32.20 | 2529.40 | 12.2532 |
| Median | 33.00 | 2551.00 | 12.205 |
| Minimum | 29.00 | 2347.00 | 12.176 |
| Maximum | 33.00 | 2629.00 | 12.421 |
| Range | 4.00 | 282.00 | 0.245 |
| Sample Standard Deviation | 1.789 | 106.877 | 0.0989 |
| Coefficient of Variation | 5.56% | 4.23% | 0.81% |

### 8.1 Packet Count Interpretation

Four of the five trials recorded 33 packets.

R03 recorded 29 packets, representing the lowest aggregate packet count.

The mean packet count was 32.20, with a sample standard deviation of approximately 1.789 packets.

The coefficient of variation was approximately 5.56%.

These results indicate relatively limited aggregate packet-count variation across the five controlled executions.

### 8.2 Frame-Byte Interpretation

The mean frame-byte measurement was 2529.40 bytes.

The smallest measurement was 2347 bytes in R03, while the largest was 2629 bytes in R01.

R04 and R05 each produced 2551 frame bytes.

The coefficient of variation was approximately 4.23%.

The observed variation reflects differences in protocol activity and individual frame sizes.

### 8.3 Experimental Duration Interpretation

The mean marker-defined experimental duration was approximately 12.253 seconds.

All five durations fell within the interval from 12.176 to 12.421 seconds.

The difference between the shortest and longest trial was 0.245 seconds.

The coefficient of variation was approximately 0.81%.

This indicates relatively consistent execution timing for the prescribed traffic-generation sequence.

The duration measurements must not be interpreted as network response latency.

## 9. Protocol Distribution Analysis

### 9.1 Packet Counts by Protocol

| Protocol | R01 | R02 | R03 | R04 | R05 | Total |
|---|---:|---:|---:|---:|---:|---:|
| ICMP | 12 | 12 | 12 | 12 | 12 | 60 |
| TCP | 14 | 16 | 14 | 14 | 14 | 72 |
| UDP | 5 | 3 | 3 | 3 | 3 | 17 |
| ARP | 2 | 2 | 0 | 4 | 4 | 12 |
| Total | 33 | 33 | 29 | 33 | 33 | 161 |

### 9.2 Frame Bytes by Protocol

| Protocol | R01 | R02 | R03 | R04 | R05 | Total |
|---|---:|---:|---:|---:|---:|---:|
| ICMP | 1176 | 1176 | 1176 | 1176 | 1176 | 5880 |
| TCP | 986 | 1106 | 986 | 986 | 986 | 5050 |
| UDP | 365 | 185 | 185 | 185 | 185 | 1105 |
| ARP | 102 | 102 | 0 | 204 | 204 | 612 |
| Total | 2629 | 2569 | 2347 | 2551 | 2551 | 12647 |

The byte measurements represent frame-level byte totals grouped by protocol classification rather than application payload sizes.

Nested application-layer classifications are not counted separately.

### 9.3 Aggregate Protocol Composition

| Protocol | Total Packets | Packet Share | Total Frame Bytes | Byte Share |
|---|---:|---:|---:|---:|
| ICMP | 60 | 37.27% | 5880 | 46.49% |
| TCP | 72 | 44.72% | 5050 | 39.93% |
| UDP | 17 | 10.56% | 1105 | 8.74% |
| ARP | 12 | 7.45% | 612 | 4.84% |
| Total | 161 | 100.00% | 12647 | 100.00% |

### 9.4 Protocol-Level Descriptive Statistics

| Protocol | Mean Packets | Sample SD | Minimum | Maximum |
|---|---:|---:|---:|---:|
| ICMP | 12.00 | 0.000 | 12 | 12 |
| TCP | 14.40 | 0.894 | 14 | 16 |
| UDP | 3.40 | 0.894 | 3 | 5 |
| ARP | 2.40 | 1.673 | 0 | 4 |

## 10. Protocol Variation and Repeatability

### 10.1 ICMP Consistency

Every repeat trial generated exactly 12 ICMP packets.

These consisted of the internal and external ICMP request-and-reply exchanges associated with the controlled connectivity tests.

The sample standard deviation of ICMP packet counts was zero.

This provides evidence of consistent ICMP traffic generation and packet observation across all five trials.

### 10.2 TCP Variation

Four trials produced 14 TCP packets, while R02 produced 16 TCP packets.

Despite this variation, TCP/22 and TCP/514 connectivity was successfully demonstrated across all five trials.

Differences in TCP packet counts may reflect connection establishment, termination, or other transport-layer behavior.

A definitive explanation for the additional TCP packets would require further packet-level TCP sequence and flag analysis.

### 10.3 UDP Variation

Each trial transmitted three uniquely identified experimental syslog markers.

R01 contained two additional UDP packets associated with background NTP activity.

Therefore, the aggregate UDP count for R01 was five packets, compared with three packets in each subsequent trial.

This demonstrates the importance of distinguishing controlled experimental traffic from incidental background traffic.

### 10.4 ARP Variation

ARP packet counts ranged from zero to four packets.

R03 contained no ARP packets, while R04 and R05 each contained four.

Differences in ARP activity may be associated with address-resolution requirements and changing local network state.

The exact ARP cache state was not independently recorded for each trial.

Consequently, this explanation remains a plausible interpretation rather than a confirmed causal finding.

### 10.5 Comparison of R04 and R05

R04 and R05 produced identical aggregate measurements:

- 33 packets.
- 2551 frame bytes.
- 12 ICMP packets.
- 14 TCP packets.
- 3 UDP packets.
- 4 ARP packets.

This demonstrates consistency at the aggregate protocol-measurement level.

It does not establish packet-by-packet identity, identical network state, or complete experimental determinism.

## 11. Destination and Service Diversity

| Destination or Service | R01 | R02 | R03 | R04 | R05 |
|---|---|---|---|---|---|
| Internal ICMP | PASS | PASS | PASS | PASS | PASS |
| External ICMP | PASS | PASS | PASS | PASS | PASS |
| TCP/22 | PASS | PASS | PASS | PASS | PASS |
| TCP/514 | PASS | PASS | PASS | PASS | PASS |
| UDP/514 | PASS | PASS | PASS | PASS | PASS |

All five trials successfully demonstrated the destination and service diversity prescribed by the controlled repeat procedure.

### 11.1 Internal Destination Reachability

SERVER-1 successfully communicated with TELEMETRY-1 in every trial.

All 15 internal ICMP echo requests received corresponding replies.

Observed internal ICMP packet loss was zero across the controlled tests.

### 11.2 External Destination Reachability

SERVER-1 successfully communicated with 8.8.8.8 in every trial.

All 15 external ICMP echo requests received corresponding replies.

Observed external ICMP packet loss was zero across the controlled tests.

### 11.3 TCP Service Reachability

TCP/22 and TCP/514 returned successful connectivity-test exit codes in all five trials.

This establishes successful transport-layer reachability for the tested services.

It does not independently establish complete application-layer functionality or security.

## 12. Telemetry Collector Analysis

| Trial | Expected Markers | Observed Markers | Observed Receipt Rate |
|---|---:|---:|---:|
| R01 | 3 | 3 | 100% |
| R02 | 3 | 3 | 100% |
| R03 | 3 | 3 | 100% |
| R04 | 3 | 3 | 100% |
| R05 | 3 | 3 | 100% |
| Total | 15 | 15 | 100% |

All 15 controlled experimental markers were observed at the TELEMETRY-1 syslog collector.

The successful collector observations establish that the controlled UDP/514 messages were not only captured in transit but were also recorded by the receiving telemetry infrastructure.

However, the observed 100% receipt rate is limited to the 15 experimental markers.

It must not be generalized to unrestricted network conditions, sustained event bursts, collector saturation, packet-loss scenarios, or production-scale telemetry workloads.

## 13. Cross-System Timestamp Analysis

Timestamp comparison between the packet capture and the telemetry collector revealed varying levels of temporal alignment.

### 13.1 R03 Timestamp Discrepancy

R03 exhibited a substantial discrepancy between packet-capture timestamps and collector timestamps.

The collector timestamps were approximately three hours and forty-four minutes earlier than the corresponding PCAP timestamps.

Subsequent clock diagnostics identified synchronization irregularities within the experimental environment.

The definitive root cause of the discrepancy was not established.

### 13.2 R04 Timestamp Alignment

R04 exhibited substantially closer temporal alignment between PCAP and collector timestamps.

The observed differences were approximately 32–35 milliseconds.

These observations indicate closer timestamp agreement but do not establish precise one-way delivery latency.

### 13.3 R05 Timestamp Discrepancy

| Marker | Observed Collector Minus PCAP Difference |
|---|---:|
| START | 1.024302 s |
| UDP514 | 1.026044 s |
| END | 1.021172 s |

R05 collector timestamps were consistently approximately 1.02 seconds later than the corresponding packet-capture timestamps.

### 13.4 Consolidated Interpretation

The observed cross-system timestamp differences demonstrate that temporal alignment cannot be assumed to remain constant across experimental executions.

Possible contributing factors include:

- Virtual-machine clock behavior.
- Clock synchronization irregularities.
- Host scheduling.
- Collector processing behavior.
- Differences in timestamp-generation mechanisms.

The relative contribution of these factors was not independently determined.

Cross-system timestamp differences are therefore excluded from network delivery latency calculations.

Within-PCAP experimental duration measurements remain usable as descriptive timing observations, subject to the behavior and limitations of the capture clock.

All original timestamps are preserved without retrospective modification.

## 14. Evidence Integrity and Traceability

Each repeat trial produced an independently identifiable PCAP file and a dedicated research results document.

### 14.1 Evidence Register

| Trial | Evidence File | Research Record | Git Commit |
|---|---|---|---|
| R01 | A02-F07-R01.pcap | [R01 Results](A02-F07-R01-Results.md) | 2695050 |
| R02 | A02-F07-R02.pcap | [R02 Results](A02-F07-R02-Results.md) | 5abe07a |
| R03 | A02-F07-R03.pcap | [R03 Results](A02-F07-R03-Results.md) | 545313b |
| R04 | A02-F07-R04.pcap | [R04 Results](A02-F07-R04-Results.md) | fdd33c9 |
| R05 | A02-F07-R05.pcap | [R05 Results](A02-F07-R05-Results.md) | e675bc7 |

### 14.2 Evidence Preservation

The original repeat-trial PCAP files are retained in the dedicated local evidence directory:

`RC002-A02-Repeat-Captures`

A SHA-256 fingerprint was generated for each preserved PCAP.

The individual research records contain their corresponding integrity fingerprints and quantitative findings.

The five research records have been committed and published to the RC-002 GitHub repository.

The raw PCAP files remain locally preserved. Their publication or independent third-party inspection has not been established.

SHA-256 fingerprints support future integrity verification but do not independently establish the accuracy of the experimental procedure.

## 15. Research Question Assessment

| Research Question | Assessment | Supporting Evidence |
|---|---|---|
| RQ1 — Experimental Observability | Supported | Intended traffic types observed in all five trials |
| RQ2 — Experimental Repeatability | Supported within tested conditions | Limited variation in frame counts, bytes, and duration |
| RQ3 — Telemetry Collection | Supported within tested conditions | 15 of 15 controlled markers recorded |
| RQ4 — Measurement Integrity | Supported with limitations | Evidence fingerprints and documented timestamp discrepancies |

The findings support the observability and preliminary repeatability of the controlled experimental procedure.

They do not establish complete operational reliability or generalizability beyond the tested environment.

## 16. Key Research Findings

### Finding 1 — Consistent Controlled ICMP Traffic

All five trials generated exactly 12 ICMP packets.

This represents the most consistent protocol-level packet-count measurement.

### Finding 2 — Limited Aggregate Measurement Variation

Packet counts and frame-byte measurements exhibited coefficients of variation of approximately 5.56% and 4.23%, respectively.

### Finding 3 — Consistent Experimental Execution Timing

The coefficient of variation for experimental duration was approximately 0.81%.

This indicates relatively consistent timing across the controlled executions.

### Finding 4 — Verified Destination and Service Diversity

All five trials successfully demonstrated the prescribed internal and external connectivity and service reachability tests.

### Finding 5 — Successful Controlled Telemetry Collection

All 15 experimental syslog markers were recorded by TELEMETRY-1.

### Finding 6 — Influence of Background and Infrastructure Traffic

Additional UDP activity and variable ARP activity contributed to differences in aggregate measurements.

TCP packet-count differences also contributed to the observed variation.

### Finding 7 — Temporal Alignment Is a Research Limitation

Cross-system timestamp agreement varied between trials.

Reliable latency-sensitive event correlation requires additional clock validation.

### Finding 8 — Improved Research Evidence Traceability

Each repeat trial has a dedicated research record, preserved PCAP, SHA-256 fingerprint, and associated Git commit.

## 17. Experimental Limitations

1. Only five controlled repeat trials were conducted.

2. The experiments were performed in an emulated enterprise network environment.

3. Measurements were limited to the SERVER-1 capture location.

4. Background and infrastructure traffic was not fully eliminated.

5. TCP reachability tests do not independently establish complete application-layer functionality.

6. External network conditions were not experimentally controlled.

7. Cross-system timestamp discrepancies prevent reliable inference of one-way network or telemetry delivery latency.

8. ARP cache state and other transient network conditions were not independently controlled for every trial.

9. The repeat procedure does not independently reproduce every HTTP and HTTPS activity associated with the original A02 experiment.

10. The observed syslog marker receipt rate is limited to 15 controlled messages.

11. Raw packet captures are retained locally rather than independently verified through public evidence publication.

12. Newly generated quantitative measurements cannot reconstruct missing measurements from the original A02 trials.

13. Descriptive statistics do not establish statistical significance or population-level generalizability.

## 18. Contribution to Project Aegis

Project Aegis investigates a progressive approach to proactive network defense through the following research phases:

BUILD → HARDEN → OBSERVE → DETECT → INVESTIGATE → RESPOND → ADAPT

RC-002 contributes to the OBSERVE phase by establishing experimental methods for collecting and characterizing network activity.

The A02 repeat-trial exercise strengthens this research foundation through:

- Controlled traffic generation.
- Packet-level quantitative measurement.
- Destination and service verification.
- Independent telemetry receipt verification.
- Evidence integrity fingerprinting.
- Measurement variability characterization.
- Explicit documentation of temporal limitations.

These capabilities provide a foundation for subsequent research involving anomaly detection, event correlation, investigation, and proactive enterprise network defense.

The results do not independently demonstrate detection effectiveness or attack-prevention capability.

Those capabilities require additional research components and experimental validation.

## 19. WP01-F07 Resolution Assessment

| Resolution Criterion | Status |
|---|---|
| Controlled repeat procedure executed | PASS |
| Five repeat trials completed | PASS |
| Experimental markers verified | PASS |
| Packet counts extracted | PASS |
| Frame-byte measurements extracted | PASS |
| Experimental durations calculated | PASS |
| Protocol distributions documented | PASS |
| Destination and service diversity verified | PASS |
| Collector receipt verified | PASS |
| SHA-256 fingerprints generated | PASS |
| Individual research records published | PASS |
| Consolidated quantitative analysis | COMPLETE |
| Cross-system timestamp limitations | DOCUMENTED |
| Original A02 measurement gap | NOT RETROSPECTIVELY RECOVERED |
| Requirements traceability update | PENDING |
| Formal finding closure | PENDING |

### Proposed Finding Disposition

**CLOSED WITH DOCUMENTED LIMITATIONS**

The newly generated quantitative evidence substantially addresses the measurement gap identified by WP01-F07.

However, the original missing A02 measurements remain unrecovered.

The controlled repeat procedure also differs in scope from the original A02 experiment, particularly with respect to HTTP and HTTPS activity.

Accordingly, the proposed finding disposition is closure with documented limitations rather than unconditional closure.

Formal closure remains subject to the relevant evidence register, requirements traceability matrix, and WP01 finding-status updates.

## 20. Conclusion

Five controlled quantitative repeat trials were successfully executed to address the evidence gap identified by WP01-F07.

Across the five trials, a total of 161 packets and 12647 frame bytes were observed within a combined marker-defined observation duration of 61.266 seconds.

The mean experimental packet count was 32.20, the mean frame-byte measurement was 2529.40, and the mean experimental duration was approximately 12.253 seconds.

All prescribed internal and external connectivity tests were successfully demonstrated.

All 15 controlled experimental syslog markers were independently verified at the telemetry collector.

The results provide evidence of preliminary repeatability, quantitative observability, and successful controlled telemetry collection within the RC-002 experimental environment.

Observed variation in TCP, UDP, and ARP traffic highlights the importance of recognizing transient network state and background activity when interpreting aggregate measurements.

Cross-system timestamp discrepancies further demonstrate the importance of clock validation in future latency-sensitive and event-correlation research.

The exercise strengthens the empirical foundation of RC-002 while preserving a clear distinction between original experimental evidence and newly generated repeat-trial evidence.

WP01-F07 is recommended for formal closure with documented limitations following completion of the outstanding traceability and finding-status updates.

---

Project Aegis — RC-002 Enterprise Network Security Observability

M6 / WP01 / F07 / Consolidated A02 Repeat Trial Analysis

End of Research Record.
