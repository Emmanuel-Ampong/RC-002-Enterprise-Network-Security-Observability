# RC-002 — Enterprise Network Security Observability and Telemetry

**Project Aegis — OBSERVE Phase**

## Project Status

🟢 **Active Development — OBSERVE Phase**

### Milestone Progress

| Milestone | Description | Status |
|---|---|---|
| M1 | GNS3 Laboratory Environment | ✅ Complete |
| M2 | Minimum Observability Topology | ✅ Complete |
| M3 | Telemetry Collection Foundation | ✅ Complete |

RC-002 has completed its laboratory, network, and centralized telemetry
foundation. The segmented GNS3 environment now provides USER, SERVER,
OBSERVABILITY, and WAN zones with validated Layer-3 routing, Internet/NAT
connectivity, a reproducible HTTP application service, and centralized
source-attributable telemetry.

M3 established centralized Syslog collection on TELEMETRY-1 from SERVER-1 and
EDGE-R1, demonstrated source-specific telemetry storage and multi-source
attribution, and verified persistence after restart. Legitimate routed,
Internet/NAT, and HTTP operations remained functional during the tested
telemetry configuration.

The project is now prepared to progress from telemetry collection toward
controlled observability experiments involving normal and abnormal network
conditions. The scope and acceptance criteria for the next milestone will be
defined before implementation begins.

## Research Question

> **To what extent can centralized network telemetry improve the observability of normal and abnormal behavior within a segmented multi-site enterprise network while preserving legitimate network operations?**

## Primary Objective

To design, implement, and experimentally evaluate a centralized network telemetry architecture that improves visibility into normal and abnormal behavior across a segmented multi-site enterprise network while preserving legitimate network operations.

## Project Aegis Progression

**BUILD → HARDEN → OBSERVE → DETECT → INVESTIGATE → RESPOND → ADAPT**

RC-001 established the BUILD and HARDEN foundation through secure enterprise network architecture, segmentation, routing, management hardening, infrastructure services, and experimental validation.

RC-002 begins the **OBSERVE** phase by investigating how security-relevant network behavior can be systematically collected, centralized, measured, and analyzed.

## Research Approach

RC-002 will follow a requirements-driven and evidence-based engineering methodology:

1. Define the research problem and objectives.
2. Develop functional and non-functional requirements.
3. Define telemetry and observability requirements.
4. Establish hypotheses, variables, and measurable success criteria.
5. Evaluate and select technologies based on project requirements.
6. Design the observability architecture.
7. Implement the selected telemetry infrastructure.
8. Establish a normal-operation baseline.
9. Conduct controlled experimental scenarios.
10. Analyze results and operational impact.
11. Document limitations and threats to validity.
12. Report findings and future research directions.

## Current Stage

**Research problem and objectives defined.**

Next stage: **Requirements Engineering**

## Relationship to RC-001

RC-001 asked:

> How can a multi-site enterprise network be designed, progressively hardened, and systematically validated while preserving required business connectivity?

RC-002 extends that foundation by asking:

> To what extent can centralized network telemetry improve the observability of normal and abnormal behavior within that environment while preserving legitimate network operations?

---

**Project Aegis**  
*Securing Tomorrow's Digital Infrastructure Through Research*