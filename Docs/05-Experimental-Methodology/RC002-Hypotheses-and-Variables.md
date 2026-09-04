# RC-002 Hypotheses and Variables

## Document Information

**Project:** RC-002 — Enterprise Network Security Observability and Telemetry  
**Project Aegis Phase:** OBSERVE  
**Document Version:** 0.1  
**Status:** Initial Experimental Hypotheses

---

## 1. Purpose

This document defines the initial hypotheses and experimental variables
used to evaluate the RC-002 research question.

The hypotheses are intentionally technology-neutral and are derived from
the research question, objectives, requirements, telemetry model, and
measurement framework.

---

## 2. Primary Research Question

**RQ-RC002**

> To what extent can centralized network telemetry improve the
> observability of normal and abnormal behavior within a segmented
> multi-site enterprise network while preserving legitimate network
> operations?

---

## 3. H1 — Centralized Observability

### Research Hypothesis H1

Centralized collection of selected network telemetry will provide
measurable visibility into events occurring across the required
enterprise telemetry sources and network sites.

### Null Hypothesis H01

Centralized collection of selected network telemetry will not provide
measurable visibility into events occurring across the required
enterprise telemetry sources and network sites.

### Primary Measurements

- M-01 — Telemetry Source Coverage
- M-02 — Event Visibility Rate
- M-06 — Evidence Completeness

### Independent Variable

Availability and use of the centralized telemetry architecture.

### Dependent Variables

- Number and proportion of required telemetry sources observable
- Number and proportion of controlled observable events producing
  usable centralized telemetry

---

## 4. H2 — Behavioral Differentiation

### Research Hypothesis H2

Selected telemetry measurements collected during predefined controlled
abnormal test periods will exhibit measurable differences from the same
measurements collected during defined normal-operation baseline periods.

### Null Hypothesis H02

Selected telemetry measurements collected during predefined controlled
abnormal test periods will not exhibit measurable differences from the
same measurements collected during defined normal-operation baseline
periods.

### Primary Measurement

- M-04 — Behavioral Differentiation

### Candidate Dependent Variables

Depending on technology capabilities, these may include:

- Connection frequency
- Destination diversity
- Port or service distribution
- Denied-access frequency
- Authentication-failure frequency
- Traffic volume
- Infrastructure-event frequency

### Independent Variable

Experimental operating condition:

- Normal-operation baseline
- Controlled abnormal test condition

### Important Interpretation Boundary

A measurable difference between experimental conditions does not, by
itself, demonstrate automated anomaly or threat detection.

RC-002 evaluates whether the selected telemetry makes behavioral
differences observable.

---

## 5. H3 — Operational Preservation

### Research Hypothesis H3

Introduction of the RC-002 observability architecture will preserve the
required legitimate connectivity and infrastructure services inherited
from the RC-001 experimental baseline.

### Null Hypothesis H03

Introduction of the RC-002 observability architecture will result in
failure or material degradation of one or more required legitimate
connectivity or infrastructure-service functions.

### Primary Measurement

- M-05 — Operational Preservation

### Independent Variable

Observability architecture state:

- Baseline/pre-observability condition where measurable
- RC-002 observability architecture active

### Dependent Variables

- Connectivity-test results
- DNS-resolution results
- Required web/intranet-service results
- Required management-connectivity results
- Routing-stability observations
- Other defined regression-test outcomes

---

## 6. Supporting Measurement — Observability Latency

M-03, Event Observability Latency, will initially be treated as a
supporting measurement rather than as the basis of a standalone
hypothesis.

Where the experimental environment provides sufficiently reliable
timestamps, RC-002 will measure:

**Event Observability Latency = Centralized observation time − Event occurrence time**

Latency results shall not be reported with greater precision than the
experimental environment supports.

---

## 7. Controlled Variables

Where technically possible, RC-002 experiments should control or record:

- Network topology
- Device configurations
- Routing configuration
- VLAN structure
- Security-policy configuration
- Telemetry-source configuration
- Test endpoints
- Test duration
- Test procedure
- Observation window
- Time synchronization
- Background traffic conditions

Variables that cannot be adequately controlled shall be documented as
experimental limitations.

---

## 8. Confounding Factors

Potential confounding factors include:

- Background network traffic
- Clock synchronization differences
- Telemetry buffering
- Collection or processing delays
- Packet loss
- Device resource constraints
- Simulator or emulator limitations
- Host operating-system behavior
- Differences between repeated test runs
- Telemetry-source configuration changes

These factors shall be considered when interpreting experimental
results.

---

## 9. Experimental Comparison Model

The basic RC-002 comparison model is:

**NORMAL BASELINE → CONTROLLED CHANGE → OBSERVATION → COMPARISON**

For behavioral experiments:

**State A:** Defined normal operation  
**State B:** Defined controlled abnormal activity

Measurements from State A and State B will be compared using predefined
metrics appropriate to the available telemetry.

---

## 10. Hypothesis Evaluation

Hypotheses shall not be accepted or rejected solely on the basis of
visual inspection of dashboards.

Evaluation shall use retained measurements and evidence associated with
the relevant acceptance criteria.

Where the experimental environment does not provide sufficient evidence
to evaluate a hypothesis, the result shall be reported as inconclusive
or not testable rather than inferred.

---

## 11. Current Status

**Initial hypotheses and variables defined.**

Final thresholds, sample sizes, observation durations, repetitions, and
statistical methods remain to be determined during detailed
experimental-methodology development.