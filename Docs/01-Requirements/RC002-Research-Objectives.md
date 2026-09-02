# RC-002 Research Objectives and Scope

## Primary Objective

To design, implement, and experimentally evaluate a centralized network telemetry architecture that improves visibility into normal and abnormal behavior across a segmented multi-site enterprise network while preserving legitimate network operations.

## Specific Objectives

### OBJ-01 — Telemetry Architecture

Design a centralized observability architecture capable of receiving security-relevant telemetry from selected enterprise network sources.

### OBJ-02 — Telemetry Collection

Collect operational and security-relevant telemetry from selected network devices, hosts, infrastructure services, and security controls.

### OBJ-03 — Centralized Visibility

Centralize collected telemetry so that activity across multiple network segments and sites can be examined from a common analysis point.

### OBJ-04 — Behavioral Baseline

Establish an observable and reproducible baseline representing legitimate network behavior under defined normal operating conditions.

### OBJ-05 — Abnormal-Behavior Observation

Introduce controlled abnormal test scenarios and determine whether their telemetry characteristics can be distinguished from the established normal-operation baseline.

### OBJ-06 — Operational Impact

Evaluate whether introducing the observability architecture materially affects legitimate connectivity and infrastructure services inherited from RC-001.

## In Scope

RC-002 includes:

- Centralized network telemetry collection.
- Selected network-device telemetry.
- Selected host and infrastructure-service telemetry.
- Multiple network segments and sites.
- Normal-operation baseline establishment.
- Controlled abnormal test scenarios.
- Centralized telemetry searching and analysis.
- Telemetry coverage measurement.
- Event visibility assessment.
- Event latency measurement where technically measurable.
- Service and connectivity regression testing.
- Documentation of failed, partial, and successful experimental outcomes.
- Analysis of limitations and threats to validity.

## Out of Scope

RC-002 does not attempt to provide:

- Autonomous incident response.
- Automated remediation.
- A production-scale Security Operations Center.
- Comprehensive zero-day threat detection.
- Advanced AI or machine-learning detection.
- Production-scale performance benchmarking.
- Malware development.
- Complete representation of real-world adversarial behavior.
- Claims that controlled laboratory abnormalities represent all real-world attacks.

## Preliminary Success Criteria

RC-002 will not be considered successful solely because telemetry is displayed by a monitoring platform.

Successful validation should demonstrate that:

1. Required telemetry sources successfully provide observable data.
2. Activity from required network segments and sites can be centrally observed.
3. A reproducible normal-operation baseline can be established.
4. Controlled abnormal events produce measurable telemetry observations.
5. Selected normal and abnormal behaviors can be compared using defined metrics.
6. Legitimate RC-001 network connectivity and infrastructure services remain operational.
7. Failed, partially satisfied, or technically untestable requirements are explicitly reported.

## Research Principle

> **Implementation success does not automatically imply requirement satisfaction.**

Experimental conclusions will therefore be based on collected evidence and predefined acceptance criteria rather than the successful installation or configuration of individual technologies.