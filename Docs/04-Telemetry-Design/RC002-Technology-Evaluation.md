# RC-002 Technology and Lab Platform Evaluation

## Document Information

**Project:** RC-002 — Enterprise Network Security Observability and Telemetry  
**Project Aegis Phase:** OBSERVE  
**Document Version:** 0.1  
**Status:** Initial Technology Evaluation

---

## 1. Purpose

This document evaluates candidate laboratory platforms and telemetry
technologies for RC-002.

Technology selection is based on the requirements, telemetry model,
measurement framework, experimental reproducibility, and available
computing resources.

The objective is to select the minimum technical architecture capable of
supporting meaningful observability experiments.

---

## 2. Available Laboratory Resources

The initial RC-002 laboratory environment is constrained by the
following local computing resources:

- CPU: AMD Ryzen 5 4500U
- Processor configuration: 6 cores
- Base frequency: approximately 2.4 GHz
- Installed RAM: 8 GB
- Available storage: approximately 68 GB
- Host operating system: Windows 11

These constraints are considered during platform and technology
selection.

---

## 3. Technology Selection Principles

Candidate technologies are evaluated according to:

1. Requirements coverage
2. Measurement capability
3. Experimental reproducibility
4. Research relevance
5. Resource requirements
6. Implementation complexity
7. Evidence and export capability

The preferred architecture should satisfy RC-002 requirements while
avoiding unnecessary infrastructure complexity.

---

## 4. Laboratory Platform Candidates

The following approaches are considered.

### Option A — Packet Tracer

Packet Tracer provides a lightweight environment for network design,
routing, switching, segmentation, and basic security-control
experiments.

#### Advantages

- Low resource requirements
- Familiar environment from RC-001
- Rapid topology development
- Strong support for logical enterprise-network configuration

#### Limitations

- Limited support for real operating systems
- Limited support for real telemetry pipelines
- Limited packet-level experimentation
- Limited integration with external monitoring and analysis tools

#### Assessment

Packet Tracer remains useful for conceptual network design but provides
limited support for the telemetry and observability experiments required
by RC-002.

---

### Option B — GNS3 Network Emulation

GNS3 provides a more flexible environment capable of integrating
network devices with Linux-based systems and telemetry tools.

#### Advantages

- Greater realism than Packet Tracer
- Support for real network-device software where available
- Integration with Linux hosts
- Support for packet capture
- Greater telemetry experimentation capability

#### Limitations

- Higher CPU and memory requirements
- Requires compatible network-device images
- Greater configuration complexity

#### Assessment

GNS3 provides a strong foundation for RC-002 but requires careful
resource management.

---

### Option C — GNS3 with Lightweight Linux Telemetry Node

This approach combines network emulation with a single lightweight Linux
system responsible for selected telemetry collection and analysis.

#### Candidate Responsibilities

The telemetry node may provide:

- Centralized Syslog collection
- Selected network-event storage
- Packet-capture support
- Basic traffic analysis
- Experimental evidence storage

#### Advantages

- Improved telemetry realism
- Centralized observation point
- Support for controlled experimentation
- Lower resource requirements than a large multi-tool stack
- Expandable architecture

#### Limitations

- Requires initial platform configuration
- Available telemetry capabilities depend on selected network images
- Limited RAM constrains the number of simultaneous systems

#### Assessment

This option provides the strongest balance between experimental value and
available laboratory resources.

---

### Option D — Full Multi-Tool Virtual Security Laboratory

This approach would combine multiple telemetry, monitoring, detection,
storage, and analysis platforms.

Potential technologies could include:

- Network monitoring systems
- Security sensors
- Host telemetry platforms
- Search and storage platforms
- Visualization platforms

#### Advantages

- Extensive functionality
- High potential analytical capability

#### Limitations

- Significant memory requirements
- Increased storage requirements
- Greater implementation complexity
- Higher risk of introducing unnecessary variables into experiments

#### Assessment

This approach is not selected for the initial RC-002 implementation
because available computing resources are limited and the research
question does not initially require a full security-operations platform.

---

## 5. Comparative Evaluation

| Evaluation Criterion | Packet Tracer | GNS3 | GNS3 + Telemetry Node | Full Multi-Tool Lab |
|---|---|---|---|---|
| Requirements coverage | Medium | High | High | Very High |
| Measurement capability | Low | High | High | Very High |
| Reproducibility | High | High | High | Medium |
| Research relevance | Medium | High | Very High | High |
| Resource requirements | Low | Medium | Medium | High |
| Implementation complexity | Low | Medium | Medium | High |
| Evidence capability | Medium | High | High | Very High |
| Suitability for RC-002 | Limited | Strong | **Preferred** | Deferred |

---

## 6. Preliminary Platform Decision

The preliminary preferred RC-002 implementation environment is:

**GNS3 with a lightweight Linux telemetry node.**

Packet Tracer may continue to be used for conceptual reference or
lightweight topology development where appropriate.

The full experimental observability environment will prioritize GNS3 and
a limited number of lightweight telemetry components.

---

## 7. Initial Telemetry Technology Categories

The initial implementation will evaluate the following categories before
selecting specific products:

### Network Event Logging

Purpose:

- Infrastructure events
- Device state changes
- Security-control events
- Administrative events

Candidate mechanism:

- Syslog

---

### Network Traffic Observation

Purpose:

- Observe selected communication behavior
- Support controlled experiments
- Preserve packet-level evidence where required

Candidate mechanisms:

- Packet capture
- Flow telemetry where supported

---

### Host and Service Observation

Purpose:

- Provide context for selected infrastructure services
- Observe service availability and relevant events

Initial approach:

- Native operating-system logging
- Selected service logs

---

### Time Context

Purpose:

- Support event ordering
- Support observability-latency measurements where practical

Candidate mechanism:

- NTP or controlled timestamp synchronization

---

## 8. Technologies Deferred from Initial Implementation

The following technologies are not excluded from future RC-002 expansion,
but are not required for the initial implementation:

- Large-scale Elasticsearch deployments
- OpenSearch clusters
- Full Wazuh deployments
- Multiple security sensors
- Automated threat-detection platforms
- Machine-learning systems
- Automated response systems

These technologies may be considered only if later experimental
requirements justify their complexity.

---

## 9. Selection Rationale

RC-002 prioritizes experimental validity over technology quantity.

The selected environment should allow the project to:

1. Generate meaningful telemetry.
2. Centralize selected observations.
3. Compare normal and controlled abnormal activity.
4. Preserve legitimate enterprise operations.
5. Retain evidence supporting experimental conclusions.

Additional technologies will only be introduced where they provide
measurable value to these objectives.

---

## 10. Current Status

**Preliminary platform direction selected.**

The exact implementation stack remains subject to detailed capability
evaluation.

A formal architectural decision record will document the final platform
selection before implementation begins.