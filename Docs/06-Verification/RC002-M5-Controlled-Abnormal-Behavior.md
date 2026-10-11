# RC-002 M5 — Controlled Abnormal Behavior Verification

## 1. Milestone

**Milestone:** M5 — Controlled Abnormal Behavior
**Project:** RC-002 — Enterprise Network Security Observability and Telemetry
**Project Aegis Phase:** OBSERVE
**Experiment Type:** Controlled abnormal-behavior experiments
**Verification Status:** PASS WITH DOCUMENTED LIMITATIONS
**Verification Date:** 2026-10-11

---

## 2. Objective

The objective of M5 was to introduce predefined, controlled deviations from legitimate network and system behavior and determine whether the RC-002 observability infrastructure could capture measurable characteristics of those deviations while preserving the principal tested network services.

M5 builds on the normal-operation reference established during M4.

The milestone evaluates observable behavioral differences, telemetry collection, experimental repeatability, and operational preservation.

It does not attempt to demonstrate automated anomaly detection, malicious-activity classification, or production-level threat-detection effectiveness.

---

## 3. Research Question

> Can selected network and system telemetry sources expose measurable characteristics of predefined controlled abnormal-behavior conditions, and can those conditions be studied without materially disrupting the principal tested network functions?

The assessment considers:

- Repeatability of controlled experimental conditions.
- Packet-level and centralized telemetry visibility.
- Comparison with relevant M4 normal-operation references.
- Operational preservation during experimental activity.
- Evidence completeness and measurement limitations.
- The extent to which the observations support H2 and H3.

---

## 4. Experimental Scenarios

Four controlled scenarios were executed.

| ID | Scenario | Manipulated condition | Principal observations |
|---|---|---|---|
| A01 | Elevated Connection Frequency | Increased legitimate HTTP request frequency | HTTP responses, TCP conversations, packet counts, traffic duration |
| A02 | Increased Destination or Service Diversity | Interactions involving multiple destinations, protocols, and services | ICMP, TCP reachability, UDP/514 telemetry |
| A03 | Controlled Denied-Access Activity | Firewall-enforced TCP/22 denial | Connection timeouts, firewall DROP counters, SYN retransmissions |
| A04 | Controlled Infrastructure-Event Burst | Increased frequency of predefined benign Syslog events | Event generation, centralized receipt, completeness, duplication |

Five trials were executed for each scenario.

**Total controlled trials executed: 20.**

---

## 5. Experimental Environment and Evidence Collection

The experiments used the RC-002 laboratory topology and telemetry infrastructure established during earlier milestones.

Principal components included:

- **SERVER-1:** `192.168.20.10`
- **TELEMETRY-1:** `192.168.30.10`
- **EDGE-R1:** Routing and network-edge functionality
- **Centralized telemetry:** Syslog collection using rsyslog
- **Packet observation:** Packet captures examined using Wireshark
- **Application observation:** HTTP response checks
- **Network observation:** ICMP and TCP connectivity checks

The scenarios used different traffic paths and observation methods according to their experimental objectives.

Trial-level measurements and evidence are retained in the corresponding scenario datasets and supporting records.

Derived interpretation is documented separately in the A01–A04 results reports.

---

## 6. A01 — Elevated Connection Frequency

### Procedure

TELEMETRY-1 generated 20 sequential legitimate HTTP requests per trial toward SERVER-1 on TCP/80, using a nominal 0.25-second inter-request interval.

Five trials were executed.

### Results

| Metric | Aggregate result |
|---|---:|
| Trials | 5 |
| HTTP requests | 100 |
| HTTP 200 responses | 100 |
| HTTP failures | 0 |
| TCP conversations | 100 |
| Displayed packets | 1,000 |
| Displayed bytes | 132,800 |

Observed filtered traffic spans ranged from **6.090 to 6.232 seconds** across the five trials.

### Interpretation

A01 produced repeatable HTTP connection activity and documented measurable differences from the M4 N02 normal HTTP reference condition.

All requests returned HTTP 200, supporting preservation of the tested application function.

Manual shell timestamps were not consistently aligned with the exact beginning and end of request generation. The report therefore uses Wireshark's filtered traffic span as the principal duration measurement.

**A01 assessment:** COMPLETE — direct evidence of reproducible behavioral differentiation within the selected HTTP measurements.

---

## 7. A02 — Increased Destination or Service Diversity

### Procedure

SERVER-1 performed a predefined sequence involving:

1. Internal ICMP connectivity.
2. TCP/22 reachability.
3. TCP/514 reachability.
4. UDP/514 test-marker transmission.
5. External ICMP connectivity.

The intended destinations were TELEMETRY-1 and `8.8.8.8`.

Five trials were executed.

### Results

| Metric | Result |
|---|---:|
| Trials executed | 5 |
| Intended destinations per trial | 2 |
| Protocol types | 3 |
| Trials with successful internal ICMP | 5 |
| Trials with successful external ICMP | 5 |
| Trials reporting TCP/22 reachability | 5 |
| Trials reporting TCP/514 reachability | 5 |
| Confirmed UDP/514 collector receipt | 4 |
| Trials with unresolved UDP/514 evidence | 1 |

### Exceptions

**A02-T02:** The retained packet-filter evidence showed no matching UDP/514 packet, and the collector search did not confirm the test marker.

The result remains PARTIAL. Successful UDP/514 delivery cannot be assumed.

**A02-T05:** An ICMP Network Unreachable message was observed for a UDP/514 datagram distinct from the confirmed test-marker packet.

The cause remains undetermined.

### Interpretation

A02 demonstrated observability of multiple network interaction types.

However, the quantitative M4 reference required to establish a numerical increase in destination or service diversity was not incorporated into the scenario analysis.

Consequently, A02 supports protocol and service observability but does not independently establish a quantified baseline-relative diversity increase.

**A02 assessment:** COMPLETE WITH LIMITATIONS.

---

## 8. A03 — Controlled Denied-Access Activity

### Procedure

A controlled firewall rule denied the predefined TCP/22 connections while other tested network functions were checked for continued availability.

Five trials were executed, each containing five connection attempts.

### Results

| Metric | Aggregate result |
|---|---:|
| Trials | 5 |
| TCP/22 connection attempts | 25 |
| Timed-out attempts | 25 |
| Successful TCP/22 connections during enforcement | 0 |
| Firewall DROP packets | 50 |
| Firewall DROP bytes | 3,000 |
| Initial SYN packets | 25 |
| SYN retransmissions | 25 |
| Confirmed UDP/514 collector receipt | 5/5 |
| HTTP 200 checks | 5/5 |

The 50 firewall DROP packets represent observed dropped packets, including retransmissions, rather than 50 distinct connection attempts.

### Operational observations

Internal ICMP connectivity and TCP/514 reachability passed across the five trials.

During A03-T01, the initial external ICMP test recorded approximately 66% packet loss. A repeat check recorded zero packet loss.

During A03-T04, external ICMP average RTT was 152.122 ms and maximum RTT was 262.432 ms. Both were within the ranges observed in the M4 N03 baseline.

After A03-T05, the experimental firewall rule was removed and TCP/22 connectivity was confirmed restored.

### Interpretation

A03 produced repeatable, observable TCP/22 denial behavior.

The M4 datasets do not contain a dedicated quantitative TCP/22 denial baseline. Therefore, A03 cannot establish a numerical baseline-relative increase in denial activity.

The original A03 results report was discovered to be truncated. Its missing analytical sections were subsequently completed using the retained trial dataset and M4 reference measurements. The original experimental results were preserved.

**A03 assessment:** COMPLETE WITH DOCUMENTED EXCEPTION AND BASELINE LIMITATION.

---

## 9. A04 — Controlled Infrastructure-Event Burst

### Procedure

SERVER-1 generated predefined benign Syslog event markers for centralized collection by TELEMETRY-1.

Each trial intended to generate 20 uniquely identified event markers.

Five trials were executed.

### Results

| Metric | Aggregate result |
|---|---:|
| Trials | 5 |
| Intended events | 100 |
| Received events | 100 |
| Unique received events | 100 |
| Missing events | 0 |
| Duplicate events | 0 |

Recorded event-generation intervals were 20, 20, 20, 20, and 22 seconds.

HTTP pre-trial and post-trial checks returned HTTP 200 across the recorded trials.

### Exceptions and limitations

- T01 had initial external ICMP packet loss and incomplete retained screenshot evidence for some experimental steps.
- T02's retained count screenshot did not clearly display the numerical count, although the event markers were visible.
- T03 and T05 included elevated external ICMP latency observations.
- T04 experienced an initial internal connectivity failure before the accepted trial checks recovered. Its root cause was not established.
- The source and collector clocks were not independently demonstrated to be synchronized.
- The M4 N04 baseline did not establish a complete numerical event-frequency distribution.
- The separately frozen A04 scenario protocol was incomplete; the master M5 methodology and retained execution records provide additional procedural context.

### Interpretation

A04 demonstrated complete receipt of the 100 predefined event markers within the recorded experiment.

This supports the observability and completeness of the controlled event sequences.

However, it does not establish a quantified increase over the M4 normal event rate or uninterrupted operational availability throughout each burst.

**A04 assessment:** COMPLETE WITH DOCUMENTED METHODOLOGICAL AND OPERATIONAL LIMITATIONS.

---

## 10. Cross-Scenario Results Summary

| Scenario | Trials | Principal finding | Assessment |
|---|---:|---|---|
| A01 | 5 | Repeatable measurable HTTP frequency differences | COMPLETE |
| A02 | 5 | Multiple protocol and service interactions observable | COMPLETE WITH LIMITATIONS |
| A03 | 5 | Repeatable firewall-denied TCP/22 activity observable | COMPLETE WITH LIMITATIONS |
| A04 | 5 | 100/100 predefined Syslog events received | COMPLETE WITH LIMITATIONS |

**Total: 20 completed controlled trials.**

The results demonstrate that selected RC-002 telemetry sources can expose several predefined experimental behaviors.

They do not establish universal anomaly detection, malicious-activity identification, or reliable classification across arbitrary enterprise workloads.

---

## 11. M5 Completion-Criteria Verification

The nine completion criteria are defined in Section 16 of:

`Docs/05-Experimental-Methodology/RC002-M5-Controlled-Abnormal-Behavior-Protocol.md`

| # | Completion criterion | Evidence and assessment | Result |
|---|---|---|---|
| 1 | Predefined controlled scenarios executed | A01–A04 executed | PASS |
| 2 | Required repetitions completed or deviations justified | Five trials per scenario; exceptions documented | PASS |
| 3 | Raw evidence retained | Scenario datasets and supporting evidence records retained; specific evidence gaps disclosed | PASS WITH LIMITATIONS |
| 4 | Derived measurements documented | Four scenario results reports and trial summaries | PASS |
| 5 | Observations compared with relevant M4 baseline | Direct A01 comparison; contextual or limited comparisons for A02–A04 | PASS WITH LIMITATIONS |
| 6 | Operational impact documented | ICMP, TCP, HTTP and telemetry checks; exceptions retained | PASS |
| 7 | Confounding factors and limitations recorded | Scenario-specific limitations and operational exceptions documented | PASS |
| 8 | H2 evaluated within evidence-supported scope | Scenario-level assessments distinguish direct differentiation from limited observability | PASS |
| 9 | Formal milestone verification document produced | This verification document | PASS |

**Completion assessment:** All nine criteria are addressed within the defined experimental scope, with explicit limitations on evidence completeness and baseline-relative inference.

This assessment does not imply that every experimental observation was successful or that every hypothesis was fully demonstrated.

---

## 12. Hypothesis Implications

### H1 — Centralized Observability

M5 provides additional evidence that the implemented RC-002 architecture can collect selected network and system telemetry.

A04 confirmed centralized receipt of all 100 predefined Syslog event markers.

A02 and A03 also provided evidence of centralized telemetry receipt, subject to the A02-T02 gap.

These observations support centralized observability within the tested scope, not complete coverage of all network events.

### H2 — Behavioral Differentiation

**Overall assessment: PARTIALLY SUPPORTED WITHIN THE TESTED SCOPE.**

A01 provides direct evidence of reproducible differences between the controlled HTTP frequency condition and the M4 N02 reference.

A02 demonstrates visibility into several protocol and service interactions, but a quantified baseline-relative diversity comparison remains outstanding.

A03 demonstrates repeatable controlled denied-access behavior, but a directly comparable M4 denial-rate baseline is unavailable.

A04 demonstrates observable and complete event-marker sequences, but the M4 N04 baseline does not provide a quantified event-rate reference.

Taken together, the scenarios establish that selected telemetry can expose several predefined controlled behaviors.

They do not establish that all four conditions can be quantitatively distinguished from normal operation, nor that automated anomaly detection has been achieved.

### H3 — Preservation of Legitimate Operations

**Overall assessment: SUPPORTED WITH QUALIFICATIONS.**

The recorded operational checks generally confirmed continued availability of the principal tested network and application functions.

A01 recorded 100 successful HTTP responses.

A02 and A03 recorded successful principal connectivity checks, subject to documented exceptions.

A04 recorded successful HTTP pre/post checks and accepted internal connectivity checks, subject to pretrial anomalies and measurement limitations.

The evidence supports operational preservation at the recorded observation points.

It does not establish uninterrupted availability, absence of transient faults, or zero performance impact throughout every experimental interval.

---

## 13. Evidence Locations

### Master methodology

`Docs/05-Experimental-Methodology/RC002-M5-Controlled-Abnormal-Behavior-Protocol.md`

### Scenario results

`results/M5-Controlled-Abnormal-Behavior/A01-Results.md`

`results/M5-Controlled-Abnormal-Behavior/A02-Results.md`

`results/M5-Controlled-Abnormal-Behavior/A03-Results.md`

`results/M5-Controlled-Abnormal-Behavior/A04-Results.md`

### Trial datasets

`datasets/M5-Controlled-Abnormal-Behavior/`

### M4 baseline

`datasets/M4-Normal-Baseline/`

### Supporting evidence

Retained packet captures, telemetry observations, trial records, and screenshots should be interpreted alongside the corresponding scenario reports.

---

## 14. Research Integrity and Limitations

The M5 experiments were performed in a controlled laboratory environment.

The results are limited to the implemented topology, selected traffic paths, predefined experimental conditions, measurement periods, and retained evidence.

The principal limitations are:

1. Only five trials were executed per abnormal-behavior scenario.
2. Some M4 references do not contain quantitative measurements directly comparable to the manipulated M5 variables.
3. A02-T02 lacks confirmed UDP/514 telemetry evidence.
4. A02-T05 contains an unexplained ICMP Network Unreachable observation.
5. A03-T01 contains an unexplained initial external ICMP packet-loss exception.
6. A04 includes operational anomalies and incomplete screenshot evidence for certain trial steps.
7. The separately frozen A04 protocol was incomplete.
8. Event-generation and collection clocks were not independently verified as synchronized.
9. Pre/post operational checks cannot establish uninterrupted availability between observations.
10. The experiments do not establish malicious intent, automated detection accuracy, or production-network generalizability.

The repaired A03 results report and the incomplete frozen A04 protocol are disclosed rather than concealed.

No missing evidence is treated as a successful observation without supporting records.

No causal explanation is assigned to unexplained network anomalies.

---

## 15. Verification Decision

M5 has satisfied the required experimental execution and documentation objectives within the defined laboratory scope.

All four predefined scenarios were executed across five trials each.

Scenario-level results and derived measurements have been documented, relevant baseline comparisons have been made where supported, and operational exceptions and methodological limitations have been retained.

The findings provide meaningful evidence of controlled behavioral observability and partial support for H2, together with qualified support for H3.

The limitations restrict the strength and generalizability of the research conclusions but do not require the experimental observations to be discarded.

**Final M5 Verification Status: PASS WITH DOCUMENTED LIMITATIONS**

**Milestone Execution Status:** COMPLETE

**Milestone Documentation Status:** COMPLETE

**Milestone Closure Recommendation:** CLOSE M5 WITH LIMITATIONS RETAINED

This verification does not represent completion of the entire RC-002 research cycle.

---

**Project Aegis**

*Securing Tomorrow's Digital Infrastructure Through Research*