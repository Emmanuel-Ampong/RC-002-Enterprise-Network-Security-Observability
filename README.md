# RC-002 - Enterprise Network Security Observability and Telemetry

**Project Aegis - OBSERVE Phase**

## Project Status

**Active Development - OBSERVE Phase**

### Milestone Progress

| Milestone | Description | Status |
|---|---|---|
| M1 | GNS3 Laboratory Environment | Complete |
| M2 | Minimum Observability Topology | Complete |
| M3 | Telemetry Collection Foundation | Complete |
| M4 | Normal Traffic Baseline and Telemetry Characterization | Complete |
| M5 | Controlled Abnormal-Behavior Experiments | In Progress - A01 Complete |

RC-002 has completed its laboratory, network, centralized telemetry, and normal-baseline foundation. The segmented GNS3 environment provides USER, SERVER, OBSERVABILITY, and WAN zones with validated Layer-3 routing, Internet/NAT connectivity, a reproducible HTTP application service, and centralized source-attributable telemetry.

M3 established centralized Syslog collection on TELEMETRY-1 from SERVER-1 and EDGE-R1, demonstrated source-specific telemetry storage and multi-source attribution, and verified persistence after restart.

M4 established a measured reference condition for legitimate behavior using inter-zone ICMP, HTTP application access, Internet ICMP, and normal system activity. Repeated trials characterized packet behavior, latency, application completion time, and source-attributed Syslog observations while legitimate operations remained functional.

M5 controlled abnormal-behavior experimentation is now in progress. A01 - Elevated Connection Frequency has been completed using five controlled trials and compared against the corresponding M4 normal reference condition. A01 produced a reproducible measurable difference in connection frequency and packet volume while legitimate application operation was preserved. This result contributes evidence toward evaluation of H2 for the tested condition, but does not by itself establish malicious activity, automated detection, or the overall H2 conclusion.

## Research Question

> **To what extent can centralized network telemetry improve the observability of normal and abnormal behavior within a segmented multi-site enterprise network while preserving legitimate network operations?**

## Primary Objective

To design, implement, and experimentally evaluate a centralized network telemetry architecture that improves visibility into normal and abnormal behavior across a segmented multi-site enterprise network while preserving legitimate network operations.

## Project Aegis Progression

**BUILD -> HARDEN -> OBSERVE -> DETECT -> INVESTIGATE -> RESPOND -> ADAPT**

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

**M5 - Controlled Abnormal-Behavior Experiments in progress.**

A01 - Elevated Connection Frequency is complete. Across five controlled trials, 100 HTTP requests produced 100 successful HTTP 200 responses, 100 TCP conversations, and 1,000 displayed HTTP/TCP packets while legitimate application operation remained functional.

A01 demonstrated measurable and reproducible behavioral differentiation from the corresponding M4 normal reference condition. This provides scenario-specific evidence relevant to H2; however, overall H2 evaluation remains in progress pending the remaining M5 controlled abnormal-behavior scenarios.

## Relationship to RC-001

RC-001 asked:

> How can a multi-site enterprise network be designed, progressively hardened, and systematically validated while preserving required business connectivity?

RC-002 extends that foundation by asking:

> To what extent can centralized network telemetry improve the observability of normal and abnormal behavior within that environment while preserving legitimate network operations?

---

**Project Aegis**
*Securing Tomorrow's Digital Infrastructure Through Research*
