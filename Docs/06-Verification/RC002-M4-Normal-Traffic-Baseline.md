# RC-002 M4 — Normal Traffic Baseline and Telemetry Characterization

## 1. Milestone

**Milestone:** M4 — Normal Traffic Baseline and Telemetry Characterization
**Project:** RC-002 — Enterprise Network Security Observability and Telemetry
**Project Aegis Phase:** OBSERVE
**Experiment Type:** Controlled normal-baseline experiment
**Verification Status:** PASS

---

## 2. Objective

The objective of M4 was to characterize the observable telemetry produced by legitimate network operations within the RC-002 laboratory environment and determine whether those observations could be measured reproducibly enough to establish a reference condition for later abnormal-behavior experiments.

M4 establishes a normal reference condition. It does not attempt to classify, detect, or distinguish abnormal behavior.

---

## 3. Research Question

> What observable telemetry characteristics are produced by legitimate network operations in the RC-002 environment, and can those characteristics be measured reproducibly enough to establish a baseline for subsequent abnormal-behavior experiments?

---

## 4. Experimental Scenarios

Four legitimate-operation scenarios were evaluated.

| ID | Scenario | Source | Destination / Observation | Primary Evidence |
|---|---|---|---|---|
| N01 | Inter-zone ICMP | SERVER-1 (192.168.20.10) | TELEMETRY-1 (192.168.30.10) | ICMP packet capture and RTT |
| N02 | HTTP application access | TELEMETRY-1 (192.168.30.10) | SERVER-1 (192.168.20.10:80) | TCP packet capture and HTTP response |
| N03 | Internet ICMP | SERVER-1 (192.168.20.10) | 8.8.8.8 through EDGE-R1/NAT | ICMP connectivity and RTT |
| N04 | Normal system activity | SERVER-1 and EDGE-R1 | TELEMETRY-1 centralized Syslog | Controlled and routine log events |

N01, N02, and N03 each used five controlled trials.

N04 examined source-attributed centralized telemetry from two infrastructure sources.

---

## 5. Measurement Approach

Packet-level evidence was collected using tcpdump/Wireshark where applicable.

Centralized host and infrastructure telemetry was examined through the rsyslog collection architecture established during M3.

The following variables were recorded where applicable:

- source and destination addresses;
- protocol;
- packet count;
- request/reply behavior;
- TCP handshake behavior;
- HTTP response status;
- RTT or transaction completion time;
- packet loss;
- capture kernel drops;
- Syslog event presence; and
- telemetry source attribution.

Raw experimental evidence is retained separately from derived analysis.

---

## 6. N01 — Normal Inter-Zone ICMP

### Procedure

SERVER-1 generated three ICMP echo requests per trial toward TELEMETRY-1.

Five controlled trials were performed while traffic was captured on TELEMETRY-1.

### Results

All five trials completed successfully.

- Requests per trial: 3
- Replies per trial: 3
- Packet loss: 0%
- Captured ICMP packets per trial: 6
- Kernel drops: 0
- Mean trial-average RTT: 3.388 ms
- Minimum trial-average RTT: 2.664 ms
- Maximum trial-average RTT: 5.450 ms

### Interpretation

The tested legitimate inter-zone ICMP operation produced a repeatable request/reply packet pattern with no observed packet loss or capture drops.

---

## 7. N02 — Normal HTTP Application Access

### Procedure

TELEMETRY-1 accessed the HTTP service hosted by SERVER-1 on TCP port 80.

Five controlled trials were captured and measured.

### Results

All five trials completed successfully.

- HTTP status: 200 in all trials
- Captured TCP packets: 10 per trial
- Client SYN observed: yes
- Server SYN-ACK observed: yes
- Kernel drops: 0
- Mean completion time: 0.0097632 seconds
- Minimum completion time: 0.007903 seconds
- Maximum completion time: 0.011616 seconds

### Interpretation

Normal HTTP application access produced a repeatable TCP interaction and successful application-layer response in every measured trial.

---

## 8. N03 — Normal Internet ICMP

### Procedure

SERVER-1 generated three ICMP echo requests per trial toward 8.8.8.8 through EDGE-R1 and the GNS3 NAT environment.

Five controlled trials were performed.

### Results

All five trials completed successfully.

- Requests per trial: 3
- Replies per trial: 3
- Packet loss: 0%
- Mean trial-average RTT: 116.3288 ms
- Minimum trial-average RTT: 55.676 ms
- Maximum trial-average RTT: 277.110 ms

### Interpretation

Internet ICMP remained operational throughout all measured trials but exhibited substantially greater latency variability than internal inter-zone ICMP.

This is an important baseline finding: legitimate traffic can exhibit substantial variation while remaining successful. Elevated latency alone therefore cannot be treated as sufficient evidence of abnormal behavior in subsequent experiments.

---

## 9. N04 — Normal System Activity

### Procedure

Centralized Syslog records from SERVER-1 and EDGE-R1 were examined for controlled normal events and routine system activity.

### Results

SERVER-1:

- Source IP: 192.168.20.10
- Controlled event observed: yes
- Routine cron activity observed: yes
- Source attribution preserved: yes
- Status: PASS

EDGE-R1:

- Source IP: 192.168.30.1
- Controlled event observed: yes
- Routine cron activity observed: yes
- Source attribution preserved: yes
- Status: PASS

### Interpretation

The centralized telemetry architecture preserved source-attributable host and infrastructure events during normal operation.

N04 is primarily categorical rather than a latency or packet-count experiment.

---

## 10. Baseline Summary

| Scenario | Primary Metric | Result | Status |
|---|---|---|---|
| N01 | Trial-average RTT | Mean 3.388 ms; range 2.664–5.450 ms | PASS |
| N02 | HTTP completion time | Mean 0.0097632 s; range 0.007903–0.011616 s | PASS |
| N03 | Trial-average RTT | Mean 116.3288 ms; range 55.676–277.110 ms | PASS |
| N04 | Source-attributed Syslog | Both tested sources observable and attributable | PASS |

---

## 11. Acceptance-Criteria Verification

| # | Acceptance Criterion | Evidence | Result |
|---|---|---|---|
| 1 | All four normal scenarios have explicit procedures | N01–N04 procedures documented | PASS |
| 2 | N01–N03 contain five controlled trials each | Trial-level CSV and packet evidence | PASS |
| 3 | Packet evidence captured where applicable | N01 and N02 PCAPs; N03 packet-capture evidence retained | PASS |
| 4 | Relevant centralized telemetry preserved where observable | N04 centralized Syslog evidence | PASS |
| 5 | Results retain source/destination/protocol or telemetry-source context | Scenario CSVs and logs | PASS |
| 6 | Repeated measurements summarized without hiding variability | Trial-level data plus aggregate statistics | PASS |
| 7 | Normal operations show no unexpected functional failure | Successful ICMP, HTTP and telemetry observations | PASS |
| 8 | Raw observations separated from interpretation | `datasets/` and `results/` separation | PASS |
| 9 | Procedure and evidence are documented sufficiently for laboratory repetition | Scenario definitions, trial records and evidence retained | PASS |
| 10 | Limitations are explicitly recorded | Section 14 | PASS |

**Acceptance result: 10/10 criteria satisfied within the defined M4 scope.**

---

## 12. Evidence Locations

Raw experimental evidence:

`datasets/M4-Normal-Baseline/`

Scenario directories:

`datasets/M4-Normal-Baseline/N01-ICMP-Interzone/`

`datasets/M4-Normal-Baseline/N02-HTTP-Application/`

`datasets/M4-Normal-Baseline/N03-Internet-ICMP/`

`datasets/M4-Normal-Baseline/N04-System-Activity/`

Derived results:

`results/M4-Normal-Baseline/M4-Baseline-Summary.csv`

`results/M4-Normal-Baseline/M4-Observations.md`

---

## 13. Hypothesis Implications

### H1 — Centralized Observability

M4 provides additional evidence that the implemented observability architecture can collect useful and source-attributable information from selected RC-002 components.

This does not establish comprehensive observability of every source or event in the network.

### H2 — Normal/Abnormal Differentiation

**H2 is not tested by M4.**

M4 establishes the legitimate-behavior reference condition required for a subsequent controlled comparison with abnormal behavior.

No claim is made that the measurements obtained during M4 are sufficient to classify future activity as normal or abnormal.

### H3 — Preservation of Legitimate Operations

The legitimate operations tested during M4 remained functional while observability mechanisms were active.

This provides additional evidence consistent with H3 within the tested scope.

It does not establish zero performance impact or universal operational preservation.

---

## 14. Limitations

The M4 baseline is limited to the RC-002 laboratory environment, implemented topology, selected traffic scenarios, and measurement period.

N01, N02, and N03 contain five trials each. These repeated trials provide a controlled laboratory reference but are not sufficient for broad statistical generalization to production enterprise networks.

N03 traverses an external network path through the GNS3 NAT environment. Internet latency may therefore vary because of conditions outside the RC-002 topology.

N04 is primarily categorical and does not quantify event frequency, event-rate distributions, or long-duration host behavior.

The baseline does not represent every form of legitimate enterprise traffic.

Deviation from the M4 measurements must not, by itself, be interpreted as evidence of malicious or abnormal behavior.

---

## 15. Conclusion

M4 successfully established a documented normal-operation baseline for the tested RC-002 scenarios.

The experiments demonstrated reproducible internal ICMP behavior, successful and repeatable HTTP application access, successful Internet connectivity with measurable latency variability, and source-attributed centralized system telemetry.

A particularly important result was the substantial RTT variability observed during legitimate N03 Internet communication despite zero packet loss. This reinforces the need for multi-feature comparison rather than simplistic single-threshold interpretation in later experiments.

M4 therefore provides the empirical normal reference required before controlled abnormal-behavior comparison can begin.

**Final M4 Verification Status: PASS**
