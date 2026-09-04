# RC-002 Measurement Framework

## Document Information

**Project:** RC-002 — Enterprise Network Security Observability and Telemetry  
**Version:** 0.1  
**Status:** Initial Measurement Framework

---

## 1. Purpose

This document defines the preliminary measurements that will be used to
evaluate the RC-002 research question.

RC-002 will evaluate observability using measurable evidence rather than
the presence of a monitoring dashboard or successful installation of a
telemetry platform.

---

## 2. Measurement Dimensions

RC-002 will initially evaluate six dimensions:

1. Telemetry source coverage
2. Event visibility
3. Event observability latency
4. Behavioral differentiation
5. Operational preservation
6. Evidence completeness

---

## 3. M-01 — Telemetry Source Coverage

### Question

How much of the required telemetry-source population is successfully
observable?

### Metric

Telemetry Source Coverage (%) =

(Required telemetry sources successfully reporting /
Total required telemetry sources) × 100

### Evidence

- Source inventory
- Centralized telemetry records
- Source verification results

---

## 4. M-02 — Event Visibility Rate

### Question

How many controlled events expected to be observable actually produce
usable telemetry?

### Metric

Event Visibility Rate (%) =

(Controlled observable events producing usable telemetry /
Total controlled events expected to be observable) × 100

### Important Limitation

An event shall only be included in the denominator when the selected
telemetry architecture is reasonably expected to expose that event.

This avoids treating inherently invisible activity as a collection
failure.

---

## 5. M-03 — Event Observability Latency

### Question

How long does it take for an event to become available at the
centralized analysis point?

### Metric

Event Observability Latency =

Centralized observation time − Event occurrence time

### Possible Units

- milliseconds
- seconds
- minutes

depending on measurement capability.

### Limitation

Latency shall only be reported where clock synchronization and
timestamp precision permit meaningful measurement.

---

## 6. M-04 — Behavioral Differentiation

### Question

Can selected measurements distinguish defined normal-operation periods
from controlled abnormal test periods?

### Candidate Measurements

Depending on available telemetry:

- Connection frequency
- Destination diversity
- Port/service distribution
- Denied-access frequency
- Authentication failures
- Traffic volume
- Infrastructure event frequency
- Other measurable telemetry characteristics

### Evaluation

The exact statistical or comparative method will be defined after
telemetry capabilities and baseline data are known.

RC-002 shall not claim anomaly detection solely because two observations
appear visually different.

---

## 7. M-05 — Operational Preservation

### Question

Does introducing the observability architecture materially disrupt
required legitimate enterprise operations?

### Candidate Measurements

- Required connectivity-test success
- DNS resolution success
- Web/intranet service success
- Required management connectivity
- Routing stability
- Other inherited RC-001 regression tests

### Evaluation

Results will be compared against a predefined regression-test baseline.

---

## 8. M-06 — Evidence Completeness

### Question

Is sufficient evidence retained to support each experimental claim?

### Candidate Measurement

Evidence Completeness (%) =

(Experiment acceptance criteria supported by retained evidence /
Total evaluated experiment acceptance criteria) × 100

### Evidence Examples

- Logs
- Queries
- Screenshots
- Exported data
- Configuration extracts
- Test records
- Result tables

---

## 9. Baseline vs Experimental Observation

RC-002 will distinguish between at least two observation states:

### State A — Normal Operation

Defined legitimate enterprise activity under controlled baseline
conditions.

### State B — Controlled Abnormal Activity

Predefined activity introduced specifically for experimental
observation.

Measurements from these states may be compared to determine whether
centralized telemetry provides useful behavioral differentiation.

---

## 10. Measurement Integrity

Measurements shall not be reported with greater precision than the
experimental environment supports.

Where measurement is unreliable, unavailable, or affected by laboratory
limitations, the result shall be documented accordingly.

Estimated values shall not be presented as directly observed
measurements.

---

## 11. Current Status

**Initial measurement framework defined.**

Thresholds, test durations, statistical methods, and final measurement
procedures remain to be defined after technology capability evaluation
and experimental design.