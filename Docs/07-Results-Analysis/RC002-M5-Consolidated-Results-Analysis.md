# RC-002 M5 — Consolidated Results Analysis

## 1. Research Context

**Project:** RC-002 — Enterprise Network Security Observability and Telemetry
**Project Aegis Phase:** OBSERVE
**Milestone:** M5 — Controlled Abnormal Behavior
**Experimental Scenarios:** A01–A04
**Total Trials:** 20
**Milestone Verification:** PASS WITH DOCUMENTED LIMITATIONS
**Analysis Date:** 2026-10-11

---

## 2. Purpose

This document consolidates the experimental findings from the four controlled abnormal-behavior scenarios conducted during RC-002 M5.

The analysis evaluates whether the implemented telemetry infrastructure exposes measurable characteristics of predefined behavioral deviations, how those observations relate to the M4 normal-operation baseline, and what they imply for the research hypotheses.

The purpose is analytical rather than administrative: M5 verification is documented separately.

No claim of automated intrusion detection, malicious-activity classification, or production-scale detection effectiveness is made.

---

## 3. Research Questions

The consolidated analysis addresses four questions:

1. Which predefined experimental behaviors were observable through the implemented telemetry infrastructure?
2. Which observations could be meaningfully compared with the M4 normal-operation baseline?
3. What evidence supports or limits H2 and H3?
4. Which measurement gaps should guide the next stage of RC-002?

---

## 4. Experimental Overview

| Scenario | Manipulated behavior | Trials | Primary evidence |
|---|---|---:|---|
| A01 | Elevated HTTP connection frequency | 5 | HTTP responses, TCP conversations, packet captures |
| A02 | Increased destination/service diversity | 5 | ICMP, TCP reachability, UDP/514 telemetry |
| A03 | Controlled TCP/22 denial | 5 | Connection timeouts, firewall DROP counters, SYN retransmissions |
| A04 | Controlled Syslog event burst | 5 | Source-generated markers, centralized event receipt |

All four scenarios were executed in the RC-002 laboratory environment.

The experiments deliberately varied different behavioral dimensions. Consequently, their packet counts, durations, and event counts should not be treated as directly comparable measures of a single underlying quantity.

---

## 5. Consolidated Experimental Findings

### 5.1 A01 — Elevated Connection Frequency

A01 generated 20 sequential HTTP requests per trial from TELEMETRY-1 to SERVER-1, with a nominal inter-request interval of 0.25 seconds.

Across five trials:

| Measurement | Result |
|---|---:|
| Total requests | 100 |
| HTTP 200 responses | 100 |
| Failed requests | 0 |
| TCP conversations | 100 |
| Displayed packets | 1,000 |
| Displayed bytes | 132,800 |
| Filtered traffic span | 6.090–6.232 seconds per trial |

The repeated trials produced consistent observable HTTP connection activity.

The M4 N02 reference consisted of one successful HTTP transaction per trial, whereas A01 generated 20 sequential requests per trial.

This establishes a reproducible difference in the observed transaction pattern and request frequency under the selected experimental conditions.

However, M4 N02 was not designed as a sustained HTTP request-rate baseline. The comparison therefore does not justify a general normal-versus-abnormal rate threshold.

**Finding A01-F1:** Increased legitimate HTTP activity produced repeatable, measurable traffic characteristics while the tested HTTP service remained functional.

### 5.2 A02 — Destination and Service Diversity

A02 used a predefined sequence involving internal ICMP, TCP/22, TCP/514, UDP/514, and external ICMP.

Across five trials:

- Internal and external ICMP checks succeeded in all five trials.
- TCP/22 and TCP/514 reachability checks succeeded in all five trials.
- UDP/514 collector receipt was confirmed in four trials.
- A02-T02 lacked confirmed UDP/514 packet and collector evidence.
- A02-T05 contained an unexplained ICMP Network Unreachable message associated with a different UDP/514 datagram from the confirmed marker.

The experiment demonstrated that several types of network interactions could be examined using the available telemetry sources.

Nevertheless, M4 did not establish a complete quantitative reference distribution for destination or service diversity.

**Finding A02-F1:** Multiple network interaction types were observable, but the evidence does not establish a quantified baseline-relative increase in diversity.

**Finding A02-F2:** The A02-T02 telemetry gap demonstrates the importance of distinguishing intended activity from confirmed observation.

### 5.3 A03 — Controlled Denied-Access Activity

A03 applied a controlled firewall restriction affecting TCP/22 access.

Across five trials:

| Measurement | Result |
|---|---:|
| TCP/22 connection attempts | 25 |
| Timed-out attempts | 25 |
| Successful TCP/22 connections during enforcement | 0 |
| Firewall DROP packets | 50 |
| Firewall DROP bytes | 3,000 |
| Initial SYN packets | 25 |
| SYN retransmissions | 25 |

The DROP packet count includes retransmissions and must not be interpreted as the number of distinct connection attempts.

The repeated combination of connection timeouts, firewall DROP observations, and TCP SYN retransmissions provides evidence of controlled denied-access behavior.

Operational checks also confirmed successful HTTP responses and selected connectivity functions during the trials.

A03-T01 included initial external ICMP packet loss that resolved on repeat testing. Its cause remains undetermined.

After the final trial, the experimental firewall rule was removed and TCP/22 connectivity was confirmed restored.

The M4 baseline does not contain a directly comparable controlled TCP/22 denial-rate reference.

**Finding A03-F1:** Firewall-enforced access denial produced repeatable packet-level and firewall-counter evidence.

**Finding A03-F2:** Corroborating evidence from multiple observation points strengthens interpretation of the controlled denial condition, but does not establish automated detection capability.

### 5.4 A04 — Infrastructure-Event Burst

A04 generated 20 predefined benign Syslog markers per trial on SERVER-1 and examined their receipt by TELEMETRY-1.

Across five trials:

| Measurement | Result |
|---|---:|
| Intended event markers | 100 |
| Received event markers | 100 |
| Unique received markers | 100 |
| Missing markers | 0 |
| Duplicate markers | 0 |
| Recorded generation durations | 20, 20, 20, 20, 22 seconds |

The retained evidence supports complete receipt of the predefined event-marker sequences.

The scenario also contained documented operational and methodological exceptions:

- A04-T01 had initial external ICMP loss and incomplete screenshot evidence for certain steps.
- A04-T04 experienced an initial internal connectivity failure before accepted trial checks recovered.
- Other trials exhibited variable external ICMP latency.
- Source and collector clocks were not independently synchronized.
- The separately frozen A04 scenario protocol was incomplete.

M4 N04 established source-attributed Syslog visibility but did not establish a complete normal event-rate distribution.

Therefore, A04 supports observation of the controlled event sequences, not a quantified baseline-relative event-rate increase.

**Finding A04-F1:** Centralized Syslog successfully received the complete set of 100 predefined event markers.

**Finding A04-F2:** Event completeness and event-rate differentiation are distinct research questions and require different measurement designs.

---

## 6. Cross-Scenario Observability Analysis

The four experiments demonstrated complementary forms of observability.

| Observability dimension | Scenario | Demonstrated capability | Evidence limitation |
|---|---|---|---|
| Connection activity | A01 | Repeatable HTTP/TCP measurements | No sustained normal-rate distribution |
| Protocol and service interaction | A02 | Multiple interaction types visible | One unconfirmed UDP/514 marker |
| Access-denial behavior | A03 | DROP counters, timeouts, SYN retransmissions | No comparable M4 denial-rate baseline |
| Centralized event activity | A04 | Complete receipt of predefined event markers | No quantified normal event-rate baseline |

These results indicate that the RC-002 architecture can support investigation of several behavioral dimensions.

However, observability is not equivalent to reliable anomaly detection.

A telemetry source may expose a measurable condition without establishing whether that condition is unusual, harmful, or malicious.

---

## 7. Comparison With M4 Normal-Operation Baseline

### 7.1 N01 — Internal ICMP

M4 N01 recorded repeatable internal ICMP communication with zero packet loss across its five trials.

M5 operational checks generally confirmed internal connectivity during the abnormal-behavior experiments, with the documented A04-T04 pretrial exception.

The evidence supports preservation of tested connectivity at recorded checkpoints rather than uninterrupted availability.

### 7.2 N02 — HTTP Application Access

M4 N02 recorded one successful HTTP transaction per trial.

A01 generated 20 sequential HTTP requests per trial, all successful.

This provides the clearest direct comparison between an M5 manipulated behavioral dimension and an M4 reference condition.

However, the different experimental workloads prevent treating the comparison as a statistically established normal HTTP request-rate threshold.

### 7.3 N03 — External ICMP

M4 N03 recorded zero packet loss across its five trials but substantial RTT variability.

Trial-average RTT ranged from 55.676 to 277.110 ms, with observed maximum RTT values reaching 578.026 ms.

M5 also recorded external ICMP latency variability and selected packet-loss exceptions.

The available observations do not establish that the manipulated M5 conditions caused those variations.

External path conditions, including the GNS3 NAT environment, remain possible confounding factors.

### 7.4 N04 — Normal System Activity

M4 N04 established centralized visibility of selected routine and controlled infrastructure events.

A04 demonstrated complete receipt of predefined event bursts.

Because M4 N04 did not establish a comparable quantitative normal event-rate distribution, the available evidence cannot support a numerical burst-to-baseline ratio.

### 7.5 Overall Baseline Assessment

M4 provides useful reference observations for selected normal network functions.

Its strongest direct behavioral comparison with M5 concerns the HTTP activity examined in A01.

For A02–A04, the M4 evidence provides contextual operational references but does not fully quantify the manipulated variables.

This limits the strength of generalized H2 conclusions.

---

## 8. Hypothesis Evaluation

### 8.1 H1 — Centralized Observability

**Assessment: SUPPORTED WITHIN THE TESTED SCOPE**

The M5 experiments provide further evidence that the RC-002 telemetry architecture can expose selected network and system activity.

The clearest centralized event-collection result is A04's confirmed receipt of 100 out of 100 predefined markers.

The A02-T02 gap prevents a claim of perfect collection reliability across all experiments.

### 8.2 H2 — Normal/Abnormal Behavioral Differentiation

**Assessment: PARTIALLY SUPPORTED**

A01 demonstrates a measurable and reproducible difference in the selected HTTP activity characteristics.

A02–A04 demonstrate observable manipulated conditions, but their baseline-relative quantitative comparisons remain limited.

Therefore, M5 supports the feasibility of measuring selected controlled behavioral differences.

It does not establish universal differentiation, reliable anomaly classification, or detection accuracy.

### 8.3 H3 — Preservation of Legitimate Operations

**Assessment: SUPPORTED WITH QUALIFICATIONS**

The principal tested application and connectivity functions generally remained available at the recorded observation points.

A01 returned 100 successful HTTP responses.

A03 demonstrated controlled TCP/22 denial while other selected functions remained accessible.

A04's pre/post checks generally supported operational preservation, subject to documented exceptions.

The evidence does not establish uninterrupted service availability, zero performance degradation, or absence of transient faults.

---

## 9. Cross-Scenario Research Insights

### Insight 1 — Multi-source evidence improves interpretation

A03 illustrates the value of corroborating connection outcomes with packet captures and firewall counters.

This provides stronger evidence of the experimental condition than a timeout alone.

### Insight 2 — Measurement design determines the strength of differentiation claims

A01 contains a directly comparable behavioral dimension.

A02–A04 contain observable experimental conditions but lack complete quantitative normal-reference distributions for their manipulated variables.

Future experiments should define comparable baseline measurements before interpreting deviations.

### Insight 3 — Successful collection does not establish detection

The ability to receive Syslog events or observe TCP retransmissions does not establish that a detection system can distinguish abnormal activity from legitimate variation.

Detection logic and validation would require separate experiments.

### Insight 4 — Operational exceptions must remain visible

The unresolved A02, A03, and A04 exceptions are part of the experimental record.

Their preservation improves the transparency of the analysis and prevents unsupported claims of perfect telemetry collection or uninterrupted network availability.

### Insight 5 — Legitimate variability complicates simple thresholds

M4 N03 demonstrated substantial Internet RTT variability during legitimate communication.

The M5 observations reinforce the need to avoid treating isolated latency changes as evidence of abnormal or malicious activity.

---

## 10. Research Limitations

The consolidated findings are subject to the following limitations:

1. Five trials per scenario provide a controlled laboratory sample, not a statistically representative enterprise dataset.
2. The M4 baseline lacks complete quantitative reference distributions for several M5 manipulated variables.
3. Some operational checks were performed at discrete observation points rather than continuously.
4. External ICMP measurements were affected by uncontrolled path variability.
5. A02-T02 contains an unresolved UDP/514 telemetry gap.
6. A02-T05 and A03-T01 contain unexplained network observations.
7. A04 contains pretrial operational exceptions and incomplete screenshot evidence.
8. The separately frozen A04 protocol was incomplete.
9. Source and collector clock synchronization was not independently verified for A04.
10. The experiments do not measure detection precision, recall, false-positive rate, or false-negative rate.

These limitations restrict the conclusions but do not invalidate the documented observations.

---

## 11. Implications for Subsequent RC-002 Research

The M5 findings identify several useful directions for the next research stage.

**Comparable normal-reference measurements:** Future experiments should establish quantitative baseline distributions for destination diversity, service diversity, denied-connection behavior, and event frequency.

**Telemetry completeness:** Repeated event-marker experiments could examine missing-event rates, duplication, and collector behavior under different controlled workloads.

**Time alignment:** Where latency or event sequencing is important, clock synchronization and timestamp interpretation should be explicitly validated.

**Operational impact:** Continuous or higher-frequency service checks could provide stronger evidence about service availability during experiments.

**Evidence correlation:** Network packet captures, firewall observations, and centralized events could be aligned to investigate whether multiple telemetry sources describe the same controlled activity consistently.

These are research directions, not claims that the corresponding capabilities have already been implemented.

Any subsequent experiments should be defined and approved through their own milestone methodology before execution.

---

## 12. Consolidated Conclusion

RC-002 M5 successfully executed 20 controlled trials across four predefined abnormal-behavior scenarios.

The experiments demonstrated repeatable HTTP connection activity, observable multi-protocol interactions, corroborated firewall-denied connection behavior, and complete receipt of 100 predefined Syslog event markers.

The strongest direct M4-to-M5 behavioral comparison was established by A01.

A02–A04 produced useful observations but did not establish complete quantitative differentiation from the corresponding normal-operation reference conditions.

The consolidated findings therefore provide partial support for H2 and qualified support for H3 within the tested laboratory scope.

The experiments also strengthen the evidence for selected centralized observability capabilities while identifying specific gaps in telemetry completeness, baseline comparability, time alignment, and operational-impact measurement.

**Overall Research Assessment: CONTROLLED BEHAVIORAL OBSERVABILITY DEMONSTRATED WITH DOCUMENTED LIMITATIONS**

**M5 Milestone Status: CLOSED — PASS WITH DOCUMENTED LIMITATIONS**

The next stage should use these findings to define a more rigorous and measurable research objective rather than assuming that observability alone establishes detection capability.

---

## 13. Supporting Research Records

**M5 methodology**

`Docs/05-Experimental-Methodology/RC002-M5-Controlled-Abnormal-Behavior-Protocol.md`

**M5 verification**

`Docs/06-Verification/RC002-M5-Controlled-Abnormal-Behavior.md`

**Scenario results**

`results/M5-Controlled-Abnormal-Behavior/A01-Results.md`

`results/M5-Controlled-Abnormal-Behavior/A02-Results.md`

`results/M5-Controlled-Abnormal-Behavior/A03-Results.md`

`results/M5-Controlled-Abnormal-Behavior/A04-Results.md`

**M4 normal baseline**

`Docs/06-Verification/RC002-M4-Normal-Traffic-Baseline.md`

`datasets/M4-Normal-Baseline/`

**M5 experimental evidence**

`datasets/M5-Controlled-Abnormal-Behavior/`

---

**Project Aegis**

*Securing Tomorrow's Digital Infrastructure Through Research*