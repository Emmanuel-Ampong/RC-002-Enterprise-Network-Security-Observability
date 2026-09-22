# RC-002 â€” Enterprise Network Security Observability and Telemetry

**Project Aegis â€” OBSERVE Phase**

## Project Status

ðŸŸ¢ **Active Development â€” OBSERVE Phase**

### Milestone Progress

| Milestone | Description | Status |
|---|---|---|
| M1 | GNS3 Laboratory Environment | Complete |
| M2 | Minimum Observability Topology | Complete |
| M3 | Telemetry Collection Foundation | Complete |
| M4 | Normal Traffic Baseline and Telemetry Characterization | Complete |

RC-002 has completed its laboratory, network, centralized telemetry, and normal-baseline foundation. The segmented GNS3 environment provides USER, SERVER, OBSERVABILITY, and WAN zones with validated Layer-3 routing, Internet/NAT connectivity, a reproducible HTTP application service, and centralized source-attributable telemetry.

M3 established centralized Syslog collection on TELEMETRY-1 from SERVER-1 and EDGE-R1, demonstrated source-specific telemetry storage and multi-source attribution, and verified persistence after restart.

M4 established a measured reference condition for legitimate behavior using inter-zone ICMP, HTTP application access, Internet ICMP, and normal system activity. Repeated trials characterized packet behavior, latency, application completion time, and source-attributed Syslog observations while legitimate operations remained functional.

The project is now prepared for controlled abnormal-behavior experiments in which observations can be compared against the M4 normal reference condition. M4 does not itself test H2 or establish that deviation from the baseline constitutes abnormal or malicious behavior.

## Research Question

> **To what extent can centralized network telemetry improve the observability of normal and abnormal behavior within a segmented multi-site enterprise network while preserving legitimate network operations?**

## Primary Objective

To design, implement, and experimentally evaluate a centralized network telemetry architecture that improves visibility into normal and abnormal behavior across a segmented multi-site enterprise network while preserving legitimate network operations.

## Project Aegis Progression

**BUILD â†’ HARDEN â†’ OBSERVE â†’ DETECT â†’ INVESTIGATE â†’ RESPOND â†’ ADAPT**

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

**M4 - Normal Traffic Baseline and Telemetry Characterization complete.**

A measured normal reference condition has been established for subsequent controlled abnormal-behavior comparison.

H2 has not yet been tested. The next experimental stage will introduce controlled abnormal conditions and compare their observable characteristics against the M4 baseline.

## Relationship to RC-001

RC-001 asked:

> How can a multi-site enterprise network be designed, progressively hardened, and systematically validated while preserving required business connectivity?

RC-002 extends that foundation by asking:

> To what extent can centralized network telemetry improve the observability of normal and abnormal behavior within that environment while preserving legitimate network operations?

---

**Project Aegis**
*Securing Tomorrow's Digital Infrastructure Through Research*
