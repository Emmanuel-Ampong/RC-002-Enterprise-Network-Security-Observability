# RC-002 Initial Implementation Stack

## Document Information

**Project:** RC-002 — Enterprise Network Security Observability and Telemetry  
**Project Aegis Phase:** OBSERVE  
**Document Version:** 0.1  
**Status:** Proposed Implementation Stack

---

## 1. Purpose

This document defines the proposed minimum implementation stack for
RC-002.

The stack is designed to support the RC-002 research question and
experimental hypotheses while remaining compatible with the available
laboratory computing resources.

The objective is to implement the minimum architecture capable of
generating, collecting, preserving, and analyzing selected network
telemetry.

---

## 2. Design Principle

RC-002 follows the principle:

> Implement the minimum observability architecture required to test the
> research question, and introduce additional technology only when it
> provides measurable experimental value.

The project does not initially attempt to implement a complete security
operations center.

---

## 3. Proposed Architecture

The proposed implementation contains the following layers:

1. Enterprise network emulation
2. Telemetry generation
3. Centralized telemetry collection
4. Experimental traffic generation
5. Evidence preservation
6. Measurement and analysis

---

## 4. Network Emulation Layer

### Proposed Technology

**GNS3**

### Purpose

GNS3 will provide the primary network experimentation environment for
RC-002.

The environment will support:

- Multi-site network topology development
- Network-device configuration
- Integration with Linux systems
- Packet capture
- Controlled communication experiments

### Relationship to RC-001

RC-001 established and experimentally validated the initial enterprise
network architecture.

RC-002 will use that architectural work as a reference baseline while
developing a more capable environment for telemetry experiments.

---

## 5. Central Telemetry Node

### Proposed Platform

A lightweight Ubuntu Server environment.

### Purpose

The telemetry node will provide a centralized observation point for
selected network and host telemetry.

Initial responsibilities include:

- Syslog collection
- Event storage
- Experimental log preservation
- Selected packet or traffic evidence
- Basic event analysis

The telemetry node will initially remain limited to the functions
required by the RC-002 experiments.

---

## 6. Network Event Logging

### Proposed Mechanism

**Syslog using rsyslog or an equivalent lightweight collector.**

### Initial Telemetry Categories

The implementation will attempt to collect selected events including:

- Device and infrastructure events
- Administrative events
- Security-control events
- Communication-related events where supported

Collected events will be retained as experimental evidence.

---

## 7. Packet and Traffic Evidence

### Proposed Tools

- Wireshark
- tcpdump

### Purpose

Packet capture will support:

- Verification of controlled traffic
- Experimental evidence preservation
- Investigation of selected communication behavior
- Comparison of expected and observed traffic

Packet capture will not automatically be interpreted as proof of threat
detection.

---

## 8. Experimental Endpoints

Lightweight Linux systems will be used where practical to generate:

### Normal Baseline Activity

Examples may include:

- Legitimate service communication
- DNS queries
- Web requests
- Authorized management activity

### Controlled Abnormal Activity

Examples will be defined and approved within individual experimental
procedures.

Controlled abnormal activity may include intentionally unauthorized or
unexpected communication patterns within the isolated laboratory
environment.

No uncontrolled external traffic or production systems will be involved.

---

## 9. Time Context

Where technically practical, the environment will use a consistent time
reference.

The objective is to support:

- Event ordering
- Correlation of observations
- Measurement of observability latency where supported

The experimental environment will document timestamp limitations.

---

## 10. Evidence Preservation

Evidence generated during experiments may include:

- Syslog records
- Packet-capture files
- Test outputs
- Screenshots
- Configuration files
- Experimental logs

Evidence will be associated with specific experimental procedures and
retained within the repository structure where practical.

Large generated datasets or sensitive artifacts may be represented by
summaries or reproducible generation procedures rather than stored
directly in the repository.

---

## 11. Deferred Technologies

The following technologies are intentionally deferred from the initial
implementation:

- Large-scale Elasticsearch deployments
- OpenSearch clusters
- Full Wazuh deployments
- Machine-learning detection systems
- Automated response systems
- Large multi-sensor security architectures

These technologies may be reconsidered only when the initial RC-002
experiments demonstrate a measurable requirement for additional
capability.

---

## 12. Expected Experimental Support

The proposed stack is expected to support:

### H1 — Centralized Observability

Centralized collection of selected telemetry from required sources.

### H2 — Behavioral Differentiation

Comparison of predefined telemetry measurements between normal and
controlled abnormal experimental conditions.

### H3 — Operational Preservation

Regression testing to confirm that required enterprise services remain
functional after observability components are introduced.

---

## 13. Current Status

**Proposed implementation stack defined.**

Final platform installation and configuration will begin only after the
technology decision is formally recorded.