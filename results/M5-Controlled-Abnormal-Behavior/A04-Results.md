# RC-002 M5 - A04 Results

## Controlled Infrastructure-Event Burst

**Project:** Project Aegis  
**Research Cycle:** RC-002  
**Milestone:** M5 - Controlled Abnormal Behavior  
**Scenario:** A04 - Controlled Infrastructure-Event Burst  
**Status:** Five controlled trials executed; results consolidated with documented exceptions and limitations  
**Primary Hypothesis:** H2 - Behavioral Differentiation  
**Secondary Hypothesis:** H3 - Operational Preservation

---

## 1. Objective

A04 investigated whether a controlled sequence of benign infrastructure-generated Syslog events could be observed, attributed, and counted consistently through centralized telemetry while preserving legitimate network operations.

The experiment examined event generation, centralized Syslog collection, source attribution, event-count consistency, and operational preservation.

The experiment did not simulate actual infrastructure failures or evaluate automated threat detection.

---

## 2. Experimental Environment

| Component | Configuration |
|---|---|
| Event source | SERVER-1 |
| Source IP | 192.168.20.10 |
| Source operating system | BusyBox Linux |
| Event-generation utility | BusyBox `logger` |
| Forwarding service | BusyBox `syslogd` |
| Central collector | TELEMETRY-1 |
| Collector IP | 192.168.30.10 |
| Collector software | rsyslog |
| Transport | UDP/514 |
| Centralized log path | `/var/log/remote/192.168.20.10/syslog.log` |
| Event identifier prefix | `RC002-A04` |

The existing RC-002 network topology was retained.

Each trial generated 20 uniquely identifiable benign Syslog markers, numbered E01 through E20.

Five trials were executed, designated A04-T01 through A04-T05.

---

## 3. Experimental Procedure

The procedure reconstructed from the recorded trial executions consisted of:

1. Confirming the absence of the upcoming trial's unique event markers in the collector log.
2. Checking internal ICMP connectivity from SERVER-1 to TELEMETRY-1.
3. Checking HTTP availability on SERVER-1.
4. Checking external ICMP connectivity.
5. Generating 20 uniquely numbered benign Syslog events at approximately one-second intervals.
6. Inspecting centralized Syslog for the generated markers.
7. Counting received event records.
8. Checking identifier uniqueness and detecting missing or duplicate records.
9. Repeating internal ICMP, HTTP, and external ICMP operational checks.
10. Recording results, anomalies, and supporting evidence.

The procedure is documented retrospectively from the executed trials. The separately frozen A04 protocol was incomplete after the beginning of Section 3, although the master M5 protocol documented the A04 objective, variables, general method, evidence requirements, and operational-preservation expectations.

The retrospective procedural description must not be interpreted as a complete pre-execution specification.

---

## 4. Individual Trial Results

| Trial | Intended | Received | Unique | Missing | Duplicates | Duration | Result |
|---|---:|---:|---:|---:|---:|---:|---|
| A04-T01 | 20 | 20 | 20 | 0 | 0 | 20 s | PASS_WITH_EXCEPTION |
| A04-T02 | 20 | 20 | 20 | 0 | 0 | 20 s | PASS |
| A04-T03 | 20 | 20 | 20 | 0 | 0 | 20 s | PASS |
| A04-T04 | 20 | 20 | 20 | 0 | 0 | 20 s | PASS_WITH_EXCEPTION |
| A04-T05 | 20 | 20 | 20 | 0 | 0 | 22 s | PASS |
| **Total** | **100** | **100** | **100** | **0** | **0** | **102 s** | **5 trials completed** |

The centralized collector recorded all intended trial-specific event identifiers.

No missing identifiers or duplicate records were reported in the documented trial checks.

The aggregate delivery completeness was:

**100 / 100 × 100 = 100%**

This finding applies to the tested event volume and laboratory conditions. It does not establish delivery reliability under sustained high-volume traffic or network congestion.

---

## 5. Aggregate Measurements

| Measurement | Result |
|---|---|
| Completed trials | 5 |
| Intended event markers | 100 |
| Received records | 100 |
| Unique event identifiers | 100 |
| Missing events | 0 |
| Duplicate records | 0 |
| Aggregate delivery completeness | 100% |
| Total recorded generation duration | 102 seconds |
| Mean generation duration | 20.4 seconds |
| Shortest generation duration | 20 seconds |
| Longest generation duration | 22 seconds |
| Trials marked PASS | 3 |
| Trials marked PASS_WITH_EXCEPTION | 2 |

The 102-second aggregate represents the sum of five separate generation intervals, not one continuous 102-second experiment.

The intended pacing was approximately one marker per second. Actual elapsed generation durations were obtained from the recorded generation start and end timestamps.

---

## 6. Operational Preservation

### 6.1 Internal ICMP Connectivity

| Trial | Pre-trial loss | Post-trial loss | Pre-trial mean RTT | Post-trial mean RTT |
|---|---:|---:|---:|---:|
| T01 | 0% | 0% | 6.798 ms | 3.904 ms |
| T02 | 0% | 0% | 3.025 ms | 3.155 ms |
| T03 | 0% | 0% | 3.089 ms | 2.716 ms |
| T04 | 0% | 0% | 2.593 ms | 3.648 ms |
| T05 | 0% | 0% | 8.773 ms | 13.304 ms |

All recorded successful pre-execution and post-execution internal ICMP checks showed zero packet loss.

T04 also experienced earlier pretrial internal connectivity failures, which recovered before event generation.

### 6.2 HTTP Application Availability

All five trials recorded HTTP status **200** during both pretrial and post-trial application checks.

This supports application availability at the tested checkpoints.

### 6.3 External ICMP Connectivity

| Trial | Pre-trial loss | Post-trial loss | Pre-trial mean RTT | Post-trial mean RTT |
|---|---:|---:|---:|---:|
| T01 | 33% | 0% | 69.706 ms | 62.309 ms |
| T02 | 0% | 0% | 51.232 ms | 43.989 ms |
| T03 | 0% | 0% | 195.150 ms | 49.108 ms |
| T04 | 0% | 0% | 50.481 ms | 67.359 ms |
| T05 | 0% | 0% | 418.502 ms | 134.400 ms |

T04's table entry reflects the successful pretrial check after recovery. An earlier pretrial external check experienced 33% packet loss and a mean RTT of 3210.547 ms.

External RTT was variable across trials. The observations do not establish that the event bursts caused the latency differences.

### 6.4 Operational Interpretation

The recorded checks support preservation of required connectivity and HTTP functionality immediately before and after the controlled event bursts.

However, these checks do not demonstrate uninterrupted service availability during every moment of event generation.

---

## 7. Trial Exceptions and Anomalies

### A04-T01

The pretrial external ICMP check recorded 33% packet loss, while the post-trial check recorded zero packet loss.

Original event-generation and post-trial operational-check screenshots were unavailable. Results were retained in the recorded terminal outputs and trial summary.

The trial was classified as `PASS_WITH_EXCEPTION`.

### A04-T02

The event-count screenshot did not clearly display the numeric count. However, the recorded collector evidence showed event identifiers E01 through E20.

The trial was classified as `PASS`, subject to this evidence-quality limitation.

### A04-T03

Pretrial external ICMP latency was elevated, with a recorded maximum RTT of 408.947 ms.

The trial's event-delivery checks passed. The latency variation cannot be attributed to event generation.

### A04-T04

Before event generation, transient internal connectivity failures were observed between SERVER-1, EDGE-R1, and TELEMETRY-1.

An earlier external ICMP check also recorded 33% packet loss and a mean RTT of 3210.547 ms.

Connectivity recovered before the controlled event sequence began, without a documented configuration change explaining the recovery.

The cause of these anomalies remains undetermined.

The trial was classified as `PASS_WITH_EXCEPTION`.

### A04-T05

The generation interval lasted 22 seconds rather than 20 seconds.

All 20 intended identifiers were received exactly once.

Pretrial external RTT was elevated, reaching a maximum of 593.822 ms.

The trial was classified as `PASS`, with these timing and latency observations documented.

---

## 8. Comparison with M4 Normal Baseline

The relevant baseline is M4 N04 - System Activity.

N04 established that controlled Syslog events and routine `crond` activity from SERVER-1 and EDGE-R1 were observable in centralized telemetry.

The baseline confirmed source attribution but did not provide a complete, comparable count of all Syslog events within a defined observation interval.

The baseline files recorded a collector observation window from 2026-09-21 00:44:05 UTC to 00:47:38 UTC, while displayed controlled-event timestamps were dated earlier. This discrepancy further limits direct temporal comparison.

A04 demonstrated repeatable collection of 20 uniquely identified event markers within each approximately 20-second generation interval.

Therefore, the experiment supports qualitative identification of a controlled event burst, but does not establish a quantified increase over the M4 normal Syslog event rate.

No baseline-to-burst frequency multiplier is reported.

---

## 9. Hypothesis Evaluation

### H2 - Behavioral Differentiation

**Finding: Partially supported.**

The controlled event sequences were observable and identifiable in centralized Syslog across all five trials.

The collector preserved trial-specific event identifiers and source attribution.

However, a quantitative difference from normal Syslog frequency could not be established because M4 N04 did not provide a directly comparable normal event-rate measurement.

The findings support observability of the deliberately generated behavior, not validated automated classification of abnormal activity.

### H3 - Operational Preservation

**Finding: Supported within the scope of pretrial and post-trial checks, with exceptions.**

All five trials recorded successful HTTP checks before and after generation.

Internal connectivity checks were successful at the accepted pretrial and post-trial checkpoints.

External connectivity was available after all five trials, although T01 and T04 exhibited documented pretrial anomalies.

These findings do not establish continuous service availability throughout each trial or demonstrate that event generation had no performance impact.

---

## 10. Methodological Limitations

1. The separately frozen A04 protocol was incomplete after the opening of Section 3. The master M5 protocol retained the broader pre-execution methodology.
2. The detailed trial procedure was reconstructed retrospectively from the recorded execution evidence.
3. Source and collector clock synchronization was not independently verified.
4. Consequently, precise end-to-end event-delivery latency cannot be established from the source and collector timestamps.
5. M4 N04 did not provide a directly comparable normal Syslog event-frequency measurement.
6. The M4 baseline contained timestamp inconsistencies affecting temporal interpretation.
7. T01 lacked some original screenshots.
8. T02 had a limited event-count screenshot.
9. T04 experienced unexplained pretrial connectivity anomalies.
10. External ICMP latency varied substantially across trials, without sufficient evidence for causal attribution.
11. Each trial used only 20 generated markers; high-volume telemetry throughput was not evaluated.
12. Operational checks were performed before and after generation rather than continuously throughout the event burst.

---

## 11. Evidence and Reproducibility

**Primary trial dataset:**

`datasets/M5-Controlled-Abnormal-Behavior/A04-Infrastructure-Events/A04-Trial-Summary.csv`

**Supporting screenshots:**

`Images/M5-Evidence/A04-Controlled-Infrastructure-Event-Burst/`

**Frozen scenario protocol:**

`Experiments/M5-A04-Controlled-Infrastructure-Event-Burst.md`

**Master experimental methodology:**

`Docs/05-Experimental-Methodology/RC002-M5-Controlled-Abnormal-Behavior-Protocol.md`

**M4 reference dataset:**

`datasets/M4-Normal-Baseline/N04-System-Activity/`

The original frozen protocol is retained unchanged to preserve the research record.

The consolidated results distinguish recorded observations from retrospective methodological reconstruction and interpretation.

---

## 12. Conclusion

A04 successfully demonstrated repeatable centralized collection of controlled, benign infrastructure-event sequences within the RC-002 laboratory.

Across five trials, all 100 intended event markers were observed, with no missing or duplicate records reported in the trial summaries.

Pretrial and post-trial checks generally supported preservation of legitimate connectivity and HTTP application availability, while several network anomalies and evidence limitations were explicitly documented.

The experiment provides evidence of source-attributed telemetry observability and event-count consistency under the tested conditions.

It does not establish automated threat detection, precise event-delivery latency, high-volume collection capacity, or a quantified frequency difference from the M4 normal-operation baseline.

These findings contribute to Project Aegis's OBSERVE phase by demonstrating controlled telemetry collection and providing documented evidence for subsequent detection-oriented research.

**A04 execution status: Complete.**  
**A04 reporting status: Consolidated results prepared for repository review.**
