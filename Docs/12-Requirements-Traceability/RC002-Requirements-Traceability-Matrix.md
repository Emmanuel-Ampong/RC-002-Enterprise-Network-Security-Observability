# RC-002 Requirements Traceability Matrix

## Document Information

**Project:** RC-002 — Enterprise Network Security Observability and Telemetry
**Project Aegis Phase:** OBSERVE
**Document Version:** 0.1
**Status:** Draft — Evidence Mapping in Progress

---

## 1. Purpose

This document establishes traceability between RC-002 research
objectives, requirements, experimental milestones, retained evidence,
and verification outcomes.

The matrix supports evidence-based evaluation of requirement
satisfaction and identification of outstanding research gaps.

---

## 2. Evaluation Principles

1. Milestone completion does not automatically establish requirement
   satisfaction.

2. Requirement status must be supported by retained experimental
   evidence and the applicable acceptance criteria.

3. Evidence from M1–M5 shall be reviewed before assigning formal
   requirement statuses.

4. Partial, failed, untested, and technically untestable requirements
   shall be reported explicitly.

5. Multi-source, multi-segment, and multi-site observability shall
   be evaluated as distinct capabilities.

6. Experimental observations shall be distinguished from
   interpretations and broader research claims.

---

## 3. Requirement Status Definitions

- **PASS:** All applicable acceptance criteria are satisfied.
- **PASS WITH LIMITATION:** The primary requirement is satisfied,
  but documented limitations restrict the conclusion.
- **PARTIAL:** Only some acceptance criteria have been validated.
- **FAIL:** Tested acceptance criteria were not satisfied.
- **NOT TESTED:** The requirement has not been experimentally evaluated.
- **NOT TESTABLE:** Laboratory or technology limitations prevent
  meaningful evaluation.

---

## 4. Functional Requirements Traceability

### FR-01 — Centralized Telemetry Collection

**Related Objective:** OBJ-01, OBJ-02

**Requirement:** Collect selected security-relevant telemetry
from enterprise infrastructure at a centralized analysis point.

**Supporting Milestone:** M3 — Telemetry Collection Foundation

**Verification Evidence:**
- Docs/06-Verification/RC002-M3-Telemetry-Collection-Foundation.md
- Images/M3-Evidence/M3-02-SERVER1-Centralized-Telemetry.png
- Images/M3-Evidence/M3-03-EDGER1-Centralized-Telemetry.png
- Images/M3-Evidence/M3-04-Multi-Source-Telemetry-Attribution.png
- Images/M3-Evidence/M3-05-Post-Reboot-Telemetry-Persistence.png

**Verified Observations:**
- TELEMETRY-1 received Syslog events from SERVER-1.
- TELEMETRY-1 received Syslog events from EDGE-R1.
- Source-specific logs preserved observed source identities.
- Controlled test events were successfully collected.
- Telemetry forwarding persisted across the tested restarts.

**Acceptance Criteria Assessment:**
- Successful telemetry reception: PASS
- Source identification: PASS
- Reproducible collection evidence: PASS

**Requirement Status:** PASS

**Scope and Limitations:**
This result establishes centralized telemetry collection for
the selected sources verified during M3.

It does not establish complete multi-site or multi-segment
telemetry coverage. Those requirements are evaluated
separately under FR-02 and FR-03.

---

### FR-02 — Multi-Site Telemetry Visibility

**Related Objectives:** OBJ-01, OBJ-03

**Requirement:** Provide centralized visibility into telemetry
originating from multiple network sites.

**Relevant Milestones:** M2, M3

**Supporting Documentation:**
- Docs/06-Verification/RC002-M2-Minimum-Observability-Topology.md
- Docs/06-Verification/RC002-M3-Telemetry-Collection-Foundation.md
- Docs/01-Requirements/RC002-Research-Problem.md

**Verified Observations:**
- M2 established a segmented, single-site GNS3 laboratory.
- M3 demonstrated centralized telemetry collection from
  SERVER-1 and EDGE-R1.
- Multi-source collection was demonstrated.
- Telemetry from multiple distinct enterprise sites was
  not demonstrated.

**Acceptance Criteria Assessment:**
- Required sites generate observable telemetry: NOT TESTED
- Telemetry attributable to originating site: NOT TESTED
- Evidence of successful multi-site visibility: NOT TESTED

**Requirement Status:** NOT TESTED

**Scope and Limitations:**
Multi-source and multi-segment observations do not establish
multi-site telemetry visibility.

The current laboratory does not reproduce the complete
multi-site RC-001 architecture.

**Outstanding Verification Work:**
- Define the required site population.
- Determine whether controlled multi-site expansion is feasible.
- Generate attributable telemetry from the required sites.
- Verify centralized reception and preserve experimental evidence.

**M6 Planning Implication:**
Evaluate the feasibility and necessity of extending RC-002
to support multi-site telemetry verification.

---

### FR-03 — Multi-Segment Visibility

**Related Objectives:** OBJ-01, OBJ-03

**Requirement:** Provide centralized visibility into selected
activity occurring across multiple logical network segments.

**Relevant Milestones:** M2, M3, M4, M5

**Supporting Documentation:**
- Docs/06-Verification/RC002-M2-Minimum-Observability-Topology.md
- Docs/06-Verification/RC002-M3-Telemetry-Collection-Foundation.md
- Docs/06-Verification/RC002-M4-Normal-Traffic-Baseline.md
- Docs/06-Verification/RC002-M5-Controlled-Abnormal-Behavior.md

**Verified Observations:**
- M2 established USER, SERVER, and OBSERVABILITY zones.
- M2 verified inter-zone connectivity.
- M3 demonstrated centralized Syslog collection from SERVER-1.
- M3 demonstrated centralized Syslog collection from EDGE-R1.
- M4 and M5 recorded selected network-communication activity.
- Complete telemetry coverage across the required segments
  has not been formally established.

**Acceptance Criteria Assessment:**
- Required segments represented in collected data: PARTIAL
- Activity attributable to source or network context: PARTIAL

**Requirement Status:** PARTIAL

**Scope and Limitations:**
Inter-zone connectivity does not independently establish
centralized telemetry visibility.

EDGE-R1 provides routing between segments, but router-generated
telemetry does not automatically demonstrate complete visibility
into endpoint activity within every segment.

The required segment population and telemetry coverage
remain to be formally evaluated.

**Outstanding Verification Work:**
- Define the required segment population.
- Inventory available telemetry sources by segment.
- Generate controlled observable events in selected segments.
- Verify centralized reception and source attribution.
- Calculate coverage using the M-01 measurement framework.

**M6 Planning Implication:**
Prioritize measurable multi-segment telemetry coverage
before making broader enterprise observability claims.

---

### FR-04 — Network Infrastructure Telemetry

**Related Objectives:** OBJ-01, OBJ-02, OBJ-03

**Requirement:** Collect selected security-relevant telemetry
from network infrastructure devices at the centralized
telemetry collection point.

**Supporting Milestone:** M3 — Telemetry Collection Foundation

**Verification Evidence:**
- Docs/06-Verification/RC002-M3-Telemetry-Collection-Foundation.md
- Images/M3-Evidence/M3-03-EDGER1-Centralized-Telemetry.png
- Images/M3-Evidence/M3-04-Multi-Source-Telemetry-Attribution.png
- Images/M3-Evidence/M3-05-Post-Reboot-Telemetry-Persistence.png

**Verified Observations:**
- EDGE-R1 successfully forwarded Syslog messages to TELEMETRY-1.
- The collector stored received messages under source 192.168.30.1.
- Controlled messages containing the RC002-M3-EDGER1 marker
  were successfully received.
- Telemetry forwarding was verified after the tested restart.

**Acceptance Criteria Assessment:**
- Selected infrastructure source generates telemetry: PASS
- Generated events centrally observable: PASS
- Source and controlled event identification: PASS
- Complete required infrastructure inventory coverage: NOT TESTED

**Requirement Status:** PASS WITH LIMITATION

**Scope and Limitations:**
The result establishes centralized Syslog collection
from the selected EDGE-R1 infrastructure device.

It does not establish telemetry coverage across all
potential infrastructure devices or demonstrate visibility
into every category of security-relevant infrastructure event.

**Outstanding Verification Work:**
- Establish the required infrastructure source inventory.
- Identify additional infrastructure event categories.
- Verify collection and attribution for selected events.
- Calculate infrastructure source coverage where applicable.

**M6 Planning Implication:**
Determine whether additional infrastructure sources or
event categories are necessary to satisfy the final
observability acceptance criteria.

---

### FR-05 — Host and Service Telemetry

**Related Objectives:** OBJ-01, OBJ-02, OBJ-03

**Requirement:** Collect selected telemetry from designated
hosts and services at the centralized collection point.

**Supporting Milestone:** M3 — Telemetry Collection Foundation

**Verification Evidence:**
- Docs/06-Verification/RC002-M3-Telemetry-Collection-Foundation.md
- Images/M3-Evidence/M3-02-SERVER1-Centralized-Telemetry.png
- Images/M3-Evidence/M3-04-Multi-Source-Telemetry-Attribution.png
- Images/M3-Evidence/M3-05-Post-Reboot-Telemetry-Persistence.png

**Verified Observations:**
- SERVER-1 forwarded Syslog messages to TELEMETRY-1.
- Received messages were stored under source 192.168.20.10.
- Controlled messages containing RC002-M3-SERVER1
  were successfully collected.
- Host telemetry forwarding persisted after the tested restart.

**Acceptance Criteria Assessment:**
- Selected host telemetry centrally observable: PASS
- Host source attribution: PASS
- Reproducible host event: PASS
- Comprehensive service-specific event coverage: NOT TESTED

**Requirement Status:** PASS WITH LIMITATION

**Scope and Limitations:**
The result establishes centralized Syslog collection
from the selected SERVER-1 host.

It does not demonstrate comprehensive telemetry collection
from all services running on SERVER-1 or from every
potential enterprise host.

Successful HTTP transactions observed in later milestones
do not independently establish centralized HTTP service
event logging.

**Outstanding Verification Work:**
- Define the required host and service source inventory.
- Identify relevant service-specific event categories.
- Verify centralized collection of selected service events.
- Assess host and service telemetry coverage.

**M6 Planning Implication:**
Evaluate whether additional host or service telemetry
is required to support the remaining research objectives.

---

### FR-06 — Normal-Operation Baseline

**Related Objectives:** OBJ-04, OBJ-05

**Requirement:** Establish documented and reproducible
normal-operation baselines for selected enterprise
network behaviors.

**Supporting Milestone:** M4 — Normal Traffic Baseline

**Supporting Experiments:**
- N01 — Internal ICMP Connectivity
- N02 — HTTP Service Access
- N03 — External ICMP Connectivity
- N04 — Routine Syslog Observation

**Verified Observations:**
- N01 completed five trials with 0% packet loss.
- N01 recorded a mean trial-average RTT of 3.388 ms.
- N02 completed five successful HTTP transactions.
- N02 recorded HTTP 200 responses in all trials.
- N02 response times ranged from 0.007903 to 0.011616 seconds.
- N03 completed five trials with 0% packet loss.
- N03 trial-average RTTs ranged from 55.676 to 277.110 ms.
- N04 demonstrated routine centralized Syslog observation
  with identifiable sources.
- Routine Syslog event rates were not quantified.

**Acceptance Criteria Assessment:**
- Documented normal operating conditions: PASS
- Defined experimental observation periods: PASS WITH LIMITATION
- Repeated baseline measurements: PASS WITH LIMITATION
- Quantitative coverage of all relevant behaviors: PARTIAL

**Requirement Status:** PASS WITH LIMITATION

**Scope and Limitations:**
The M4 experiments establish normal-operation reference
measurements for selected network behaviors.

N02 measured individual HTTP transactions rather than
sustained HTTP request rates.

N04 did not establish a quantitative routine Syslog
event-rate baseline.

Consequently, the M4 results do not provide directly
comparable quantitative baselines for every abnormal
behavior scenario evaluated during M5.

**Outstanding Verification Work:**
- Define consistent observation windows where required.
- Establish quantitative routine Syslog event rates.
- Establish comparable normal traffic-volume measurements.
- Document baseline variability across repeated trials.

**M6 Planning Implication:**
Prioritize quantitative baseline gaps that directly
affect normal-versus-abnormal behavioral comparisons.

---

### FR-07 — Controlled Abnormal-Event Observation

**Related Objectives:** OBJ-03, OBJ-05

**Requirement:** Observe selected abnormal network behaviors
under controlled experimental conditions and evaluate
their visibility relative to normal operations.

**Supporting Milestone:** M5 — Controlled Abnormal Behavior

**Supporting Experiments:**
- A01 — Elevated HTTP Activity
- A02 — Multi-Protocol Activity
- A03 — Firewall Denial
- A04 — Syslog Burst

**Verification Evidence:**
- Docs/06-Verification/RC002-M5-Controlled-Abnormal-Behavior.md
- Docs/07-Results-Analysis/RC002-M5-Consolidated-Results-Analysis.md
- results/M5-Controlled-Abnormal-Behavior/A01-Results.md
- results/M5-Controlled-Abnormal-Behavior/A02-Results.md
- results/M5-Controlled-Abnormal-Behavior/A03-Results.md
- results/M5-Controlled-Abnormal-Behavior/A04-Results.md

**Verified Observations:**
- Four predefined abnormal-behavior scenarios were executed.
- Each scenario was evaluated across five trials.
- A01 produced 100 successful HTTP responses.
- A02 demonstrated selected multi-protocol observations,
  with one missing UDP collector marker.
- A03 produced 25 TCP/22 timeouts and 50 observed
  firewall DROP packets.
- A04 successfully collected 100 unique Syslog markers
  without missing or duplicate markers.
- Visibility gaps and experimental limitations were documented.

**Acceptance Criteria Assessment:**
- Predefined controlled abnormal scenarios: PASS
- Observable corresponding telemetry: PASS WITH LIMITATION
- Comparison against normal-operation baselines: PARTIAL
- Invisible or unexplained activity documented: PASS WITH LIMITATION

**Requirement Status:** PASS WITH LIMITATION

**Scope and Limitations:**
The results establish observability of selected abnormal
behaviors under controlled laboratory conditions.

A01 lacked a directly comparable sustained normal HTTP
request-rate baseline.

A02 contained one missing UDP collector marker and
an unexplained ICMP unreachable observation.

A03 included transient external connectivity issues.

A04 lacked a quantitative routine Syslog event-rate baseline.
Clock synchronization limitations also restricted timing analysis.

The experiments do not establish automated anomaly detection
or comprehensive coverage of abnormal network behavior.

**WP01-F05 Capture-Quality Qualification:**
Independent inspection of all five M4 N02 baseline PCAP files and all five M5 A01 PCAPNG files found zero retained capture-interface statistics entries in each file. Consequently, historical claims of zero kernel packet-capture drops cannot be independently verified from the retained captures. This limitation does not invalidate the observed HTTP transactions or establish that capture drops occurred. Future trials require retained capture-tool drop counters, relevant interface statistics, and supporting measurement logs. See `Docs/14-M6-Research-Execution/RC002-M6-WP01-Evidence-Audit.md`, Section 4.2.2.

**Outstanding Verification Work:**
- Strengthen comparable normal-versus-abnormal baselines.
- Investigate unexplained telemetry gaps.
- Improve evidence consistency where needed.
- Evaluate repeatability of selected behavioral distinctions.

**M6 Planning Implication:**
Prioritize evidence gaps that limit defensible
normal-versus-abnormal comparisons.

---

### FR-08 — Telemetry Search and Analysis

**Related Objectives:** OBJ-02, OBJ-03, OBJ-05

**Requirement:** Support reproducible searching, filtering,
and analysis of centrally collected telemetry.

**Relevant Milestones:** M3, M5

**Supporting Documentation:**
- Docs/06-Verification/RC002-M3-Telemetry-Collection-Foundation.md
- Docs/06-Verification/RC002-M5-Controlled-Abnormal-Behavior.md
- Docs/07-Results-Analysis/RC002-M5-Consolidated-Results-Analysis.md

**Verified Observations:**
- M3 established source-specific Syslog storage on TELEMETRY-1.
- Controlled event markers were identifiable in collected logs.
- M5 demonstrated retrieval and examination of selected
  experimental telemetry.
- Basic source identification and event inspection were possible.
- A comprehensive, formally verified search and filtering
  procedure has not been established.

**Acceptance Criteria Assessment:**
- Collected telemetry can be searched: PASS
- Source-based event identification: PASS
- Time-based filtering and analysis: NOT TESTED
- Fully documented reproducible search procedures: PARTIAL

**Requirement Status:** PARTIAL

**Scope and Limitations:**
Existing evidence supports basic telemetry retrieval
and examination using source-specific logs and
controlled experimental markers.

It does not establish a comprehensive telemetry
analysis capability or validated time-based filtering.

No automated detection, advanced correlation,
or dedicated security analytics platform has
been demonstrated.

**Outstanding Verification Work:**
- Define reproducible telemetry search procedures.
- Verify source-based filtering.
- Verify controlled event-marker filtering.
- Verify timestamp-based filtering.
- Preserve representative search outputs.
- Document search limitations.

**M6 Planning Implication:**
Evaluate whether lightweight command-line analysis
is sufficient to satisfy FR-08 before introducing
additional telemetry analysis tools.

---

### FR-09 — Time-Based Event Correlation

**Related Objectives:** OBJ-03, OBJ-05

**Requirement:** Support time-based association of collected
telemetry with controlled experimental activities using
usable timestamps and documented observation windows.

**Relevant Milestones:** M3, M4, M5

**Supporting Documentation:**
- Docs/06-Verification/RC002-M3-Telemetry-Collection-Foundation.md
- Docs/06-Verification/RC002-M5-Controlled-Abnormal-Behavior.md
- Docs/07-Results-Analysis/RC002-M5-Consolidated-Results-Analysis.md
- Docs/04-Telemetry-Design/RC002-Measurement-Framework.md

**Verified Observations:**
- Centralized Syslog records contained timestamps.
- Controlled experimental activities were documented
  using identifiable trial markers and observation windows.
- Selected telemetry records could be associated with
  their corresponding experimental trials.
- Clock synchronization across laboratory devices
  was not formally verified.
- Reliable cross-device event ordering was not established.
- Precise event-to-collection latency was not validated.

**Acceptance Criteria Assessment:**
- Usable event timestamps: PASS WITH LIMITATION
- Association with experimental observation windows: PARTIAL
- Clock-related limitations documented: PASS
- Reliable cross-device temporal correlation: NOT TESTED

**Requirement Status:** PARTIAL

**Scope and Limitations:**
The available evidence supports basic association
of selected telemetry records with controlled
experimental activities.

However, event timestamps cannot yet be treated
as a verified common time reference across devices.

Differences between device clocks may affect
event ordering and timing interpretations.

The M-03 telemetry latency metric cannot be
reliably evaluated without validating the
relevant clock relationships.

**Outstanding Verification Work:**
- Inspect time settings on participating devices.
- Determine whether clock synchronization is feasible.
- Verify timestamp consistency across selected sources.
- Demonstrate event filtering by observation window.
- Document any remaining timing uncertainty.
- Evaluate M-03 only if the required timing conditions
  can be established.

**M6 Planning Implication:**
Prioritize timestamp validation before making
cross-device correlation or collection-latency claims.

---

### FR-10 — Evidence Preservation

**Related Objectives:** OBJ-02, OBJ-03, OBJ-04, OBJ-05, OBJ-06

**Requirement:** Preserve experimental evidence and maintain
traceability between research requirements, experiments,
observations, and verification outcomes.

**Relevant Milestones:** M3, M4, M5

**Supporting Documentation:**
- Docs/06-Verification/RC002-M3-Telemetry-Collection-Foundation.md
- Docs/06-Verification/RC002-M5-Controlled-Abnormal-Behavior.md
- Docs/07-Results-Analysis/RC002-M5-Consolidated-Results-Analysis.md
- results/M5-Controlled-Abnormal-Behavior/A01-Results.md
- results/M5-Controlled-Abnormal-Behavior/A02-Results.md
- results/M5-Controlled-Abnormal-Behavior/A03-Results.md
- results/M5-Controlled-Abnormal-Behavior/A04-Results.md

**Verified Observations:**
- M3 retained screenshots supporting centralized telemetry
  collection, source attribution, and persistence.
- M4 documented normal-operation baseline measurements.
- M5 retained experimental datasets and individual
  results for four controlled abnormal-behavior scenarios.
- M5 consolidated experimental observations, interpretations,
  and documented limitations.
- The Requirements Traceability Matrix is being developed
  to connect requirements with retained evidence.

**Acceptance Criteria Assessment:**
- Experimental evidence retained: PASS WITH LIMITATION
- Evidence linked to experiments: PASS
- Evidence linked to requirements: PARTIAL
- Observations distinguished from interpretations: PASS WITH LIMITATION

**Requirement Status:** PASS WITH LIMITATION

**Scope and Limitations:**
Evidence preservation has been demonstrated across
the completed experimental milestones.

However, some M5 evidence gaps remain, including
an incomplete screenshot record and inconsistencies
in selected experimental observations.

The requirements-to-evidence mapping is not yet complete.

The current assessment does not establish that every
experimental result is independently reproducible
from the retained artifacts alone.

**Outstanding Verification Work:**
- Complete the requirements traceability review.
- Audit retained evidence against experimental protocols.
- Identify missing or inconsistent evidence artifacts.
- Document unresolved evidence gaps explicitly.
- Verify that supporting files are correctly referenced.

**M6 Planning Implication:**
Prioritize evidence completeness and traceability
before extending experimental claims.

---

### FR-11 — Operational Regression Verification

**Related Objectives:** OBJ-01, OBJ-06

**Requirement:** Verify that telemetry collection and
controlled experiments preserve legitimate network
connectivity and essential service operations.

**Relevant Milestones:** M2, M3, M4, M5

**Supporting Documentation:**
- Docs/06-Verification/RC002-M2-Minimum-Observability-Topology.md
- Docs/06-Verification/RC002-M3-Telemetry-Collection-Foundation.md
- Docs/06-Verification/RC002-M5-Controlled-Abnormal-Behavior.md
- Docs/07-Results-Analysis/RC002-M5-Consolidated-Results-Analysis.md

**Verified Observations:**
- M2 demonstrated inter-zone connectivity and HTTP access.
- M3 verified basic network connectivity after telemetry
  forwarding was configured.
- M4 established successful normal-operation ICMP and
  HTTP baseline measurements.
- M5 documented operational observations during
  controlled abnormal-behavior experiments.
- A01 completed 100 HTTP requests successfully.
- A03 produced the expected TCP/22 denials under
  the temporary firewall rule.
- Transient external connectivity issues were
  observed during A03.
- The temporary firewall rule was removed after testing.

**Acceptance Criteria Assessment:**
- Selected connectivity and services tested: PASS
- Expected operational behavior documented: PASS
- Operational failures and limitations recorded: PASS
- Comprehensive regression coverage: PARTIAL
- Consistent before-and-after regression verification: PARTIAL

**Requirement Status:** PASS WITH LIMITATION

**Scope and Limitations:**
The available evidence demonstrates that selected
legitimate network functions remained operational
during the completed milestones.

However, the results do not establish uninterrupted
operation of every service throughout all experiments.

The transient external connectivity issues observed
during A03 remain a documented limitation.

A comprehensive regression suite with consistent
before-and-after verification has not yet been
formally demonstrated.

**Outstanding Verification Work:**
- Define the required operational regression test set.
- Establish expected results for each test.
- Execute baseline and post-change regression checks.
- Record unexpected failures and recovery actions.
- Compare operational results across selected experiments.
- Evaluate operational preservation using M-05.

**M6 Planning Implication:**
Formalize a repeatable operational regression suite
before introducing additional telemetry sources,
network changes, or experimental scenarios.

---

## 5. Non-Functional Requirements Traceability

### NFR-01 — Reproducibility

**Requirement:** Ensure that experimental procedures,
configurations, and observations are sufficiently documented
to support repeatable testing.

**Relevant Milestones:** M2, M3, M4, M5

**Supporting Documentation:**
- Docs/06-Verification/RC002-M2-Minimum-Observability-Topology.md
- Docs/06-Verification/RC002-M3-Telemetry-Collection-Foundation.md
- Docs/06-Verification/RC002-M5-Controlled-Abnormal-Behavior.md
- Docs/05-Experimental-Methodology/RC002-M5-Controlled-Abnormal-Behavior-Protocol.md
- Docs/07-Results-Analysis/RC002-M5-Consolidated-Results-Analysis.md

**Verified Observations:**
- M2 documented the minimum observability topology.
- M3 documented centralized telemetry configuration
  and verification procedures.
- M4 established repeated normal-operation measurements.
- M5 executed four controlled abnormal-behavior scenarios.
- Each M5 scenario was repeated across five trials.
- Experimental results and limitations were documented.

**Acceptance Criteria Assessment:**
- Experimental procedures documented: PASS
- Repeated experimental trials: PASS
- Supporting results retained: PASS WITH LIMITATION
- Independent reproduction of all results: NOT TESTED

**Requirement Status:** PASS WITH LIMITATION

**Scope and Limitations:**
The completed milestones demonstrate repeatable
experimental procedures under selected laboratory conditions.

However, the evidence does not establish that all
experiments can be independently reproduced without
additional clarification or environmental adjustments.

Some M5 evidence gaps, timing limitations, and unexplained
observations remain documented.

**Outstanding Verification Work:**
- Review experimental instructions for completeness.
- Confirm required configurations and dependencies.
- Address missing or inconsistent evidence.
- Repeat selected experiments where necessary.
- Evaluate independent reproducibility where feasible.

**M6 Planning Implication:**
Strengthen reproducibility documentation before
introducing additional experimental complexity.

---

### NFR-02 — Traceability

**Requirement:** Maintain traceable relationships between
research objectives, requirements, experimental procedures,
verification outcomes, results, and retained evidence.

**Relevant Milestones:** M1, M2, M3, M4, M5

**Supporting Documentation:**
- Docs/01-Requirements/RC002-Requirements-Specification.md
- Docs/06-Verification/RC002-M3-Telemetry-Collection-Foundation.md
- Docs/06-Verification/RC002-M5-Controlled-Abnormal-Behavior.md
- Docs/07-Results-Analysis/RC002-M5-Consolidated-Results-Analysis.md
- Docs/12-Requirements-Traceability/RC002-Requirements-Traceability-Matrix.md

**Verified Observations:**
- Research objectives and requirements were documented.
- Experimental milestones produced verification records.
- M5 results were organized by controlled experiment.
- The RTM links functional requirements to relevant
  milestones, observations, and supporting documentation.
- All 11 functional and 8 non-functional requirements
  have been mapped in the RTM.
- All 11 unique referenced Markdown documentation paths
  were checked and confirmed to exist.
- Screenshot, dataset, and claim-level evidence validation
  remains incomplete.

**Acceptance Criteria Assessment:**
- Research objectives and requirements documented: PASS
- All 19 requirements mapped to relevant milestones: PASS
- Referenced Markdown documentation paths validated: PASS
- Experimental outcomes linked to retained evidence:
  PASS WITH LIMITATION
- Screenshot and dataset evidence validation: PARTIAL
- Complete requirement-to-evidence audit: PARTIAL

**Requirement Status:** PARTIAL

**Scope and Limitations:**
The RTM now maps all 11 functional and 8 non-functional
requirements to relevant milestones, observations,
acceptance criteria, and supporting documentation.

All 11 unique referenced Markdown documentation paths
have been checked and confirmed to exist.

However, the existence of supporting documents does not
establish that every requirement is fully satisfied.

Screenshot, dataset, and claim-level evidence validation
remains incomplete. Some experimental evidence gaps
and limitations also remain unresolved.

The requirement-to-evidence audit is therefore partially
complete, and full traceability has not yet been established.

**Outstanding Verification Work:**
- Validate screenshot and dataset evidence references.
- Verify that supporting artifacts substantiate the
  observations and acceptance criteria assigned to
  each requirement.
- Identify requirements with missing, inconsistent,
  or insufficient supporting evidence.
- Confirm consistency between requirement statuses
  and milestone verification outcomes.
- Consolidate unresolved traceability gaps.

**M6 Planning Implication:**
Complete the evidence-reference audit and prioritize
requirements with insufficient supporting evidence.

---

### NFR-03 — Operational Preservation

**Requirement:** Ensure that telemetry collection,
monitoring activities, and controlled experiments
do not unnecessarily disrupt legitimate network
connectivity or essential service operations.

**Relevant Milestones:** M2, M3, M4, M5

**Related Functional Requirement:** FR-11

**Supporting Documentation:**
- Docs/06-Verification/RC002-M2-Minimum-Observability-Topology.md
- Docs/06-Verification/RC002-M3-Telemetry-Collection-Foundation.md
- Docs/06-Verification/RC002-M5-Controlled-Abnormal-Behavior.md
- Docs/07-Results-Analysis/RC002-M5-Consolidated-Results-Analysis.md

**Verified Observations:**
- M2 demonstrated functioning inter-zone connectivity.
- M3 verified basic network operation after centralized
  telemetry collection was configured.
- M4 established successful normal-operation ICMP
  and HTTP measurements.
- M5 executed controlled abnormal-behavior scenarios
  while documenting operational observations.
- A01 completed 100 successful HTTP requests.
- A03 demonstrated the expected TCP/22 denial behavior.
- The temporary A03 firewall rule was removed after testing.
- Transient external connectivity issues were observed
  during A03.

**Acceptance Criteria Assessment:**
- Selected legitimate connectivity preserved:
  PASS WITH LIMITATION
- Selected essential services remained accessible:
  PASS WITH LIMITATION
- Operational disruptions documented: PASS
- Comprehensive operational preservation verified: PARTIAL

**Requirement Status:** PASS WITH LIMITATION

**Scope and Limitations:**
The available evidence supports preservation of selected
legitimate network functions during completed milestones.

However, uninterrupted connectivity and service availability
were not demonstrated across every experimental condition.

The transient external connectivity issues observed
during A03 remain an unresolved operational limitation.

The existing results do not establish that telemetry
collection has zero performance or availability impact.

**Outstanding Verification Work:**
- Define essential connectivity and service requirements.
- Establish a repeatable operational regression suite.
- Verify network operation before and after changes.
- Investigate unexplained connectivity disruptions.
- Measure operational impact where technically feasible.
- Evaluate operational preservation using M-05.

**M6 Planning Implication:**
Formalize operational regression verification before
expanding telemetry collection or introducing additional
experimental complexity.

---

### NFR-04 — Security

**Requirement:** Ensure that the telemetry architecture
does not introduce unnecessary security weaknesses
and that collected telemetry is appropriately protected.

**Relevant Milestones:** M2, M3, M5

**Supporting Documentation:**
- Docs/01-Requirements/RC002-Requirements-Specification.md
- Docs/06-Verification/RC002-M2-Minimum-Observability-Topology.md
- Docs/06-Verification/RC002-M3-Telemetry-Collection-Foundation.md
- Docs/06-Verification/RC002-M5-Controlled-Abnormal-Behavior.md

**Verified Observations:**
- M2 established a segmented laboratory topology.
- M3 configured centralized Syslog collection.
- TELEMETRY-1 received messages over UDP/TCP port 514.
- Source-specific telemetry storage was demonstrated.
- M5 tested selected firewall denial behavior.
- Telemetry transport encryption was not demonstrated.
- Collector access controls and log file permissions
  have not been formally audited.
- Protection against unauthorized log modification
  has not been experimentally verified.

**Acceptance Criteria Assessment:**
- Segmented laboratory architecture: PASS
- Centralized telemetry collection: PASS
- Telemetry transport confidentiality: NOT TESTED
- Collector access restrictions: NOT TESTED
- Stored telemetry access protection: NOT TESTED
- Resistance to unauthorized modification: NOT TESTED

**Requirement Status:** PARTIAL

**Scope and Limitations:**
The existing architecture provides a foundation
for controlled telemetry collection.

However, successful collection does not independently
establish secure telemetry transport or protected storage.

Traditional UDP/TCP Syslog on port 514 does not
inherently provide transport encryption or
cryptographic source authentication.

Source attribution based on observed network addresses
should not be interpreted as verified sender identity.

The current evidence does not establish that the
collector or stored telemetry is protected against
unauthorized access or modification.

**Outstanding Verification Work:**
- Review telemetry collector network exposure.
- Inspect collector and log file permissions.
- Evaluate authentication and access restrictions.
- Document transport confidentiality limitations.
- Assess practical security controls appropriate
  for the lightweight laboratory architecture.
- Verify selected controls experimentally.

**M6 Planning Implication:**
Perform a focused telemetry security assessment
before expanding source coverage or introducing
additional telemetry services.

---

### NFR-05 — Data Integrity

**Requirement:** Preserve the accuracy, completeness,
and integrity of collected telemetry and retained
experimental evidence.

**Relevant Milestones:** M3, M4, M5

**Supporting Documentation:**
- Docs/06-Verification/RC002-M3-Telemetry-Collection-Foundation.md
- Docs/06-Verification/RC002-M5-Controlled-Abnormal-Behavior.md
- Docs/07-Results-Analysis/RC002-M5-Consolidated-Results-Analysis.md
- results/M5-Controlled-Abnormal-Behavior/A02-Results.md
- results/M5-Controlled-Abnormal-Behavior/A04-Results.md

**Verified Observations:**
- M3 demonstrated centralized storage of source-attributed
  Syslog messages.
- M5 A04 generated 100 unique controlled Syslog markers.
- All 100 A04 markers were observed at the collector.
- No missing or duplicate A04 markers were reported.
- M5 A02 recorded one missing UDP collector marker.
- Some experimental evidence gaps remain documented.
- Cryptographic verification of retained evidence
  has not been formally demonstrated.

**Acceptance Criteria Assessment:**
- Selected telemetry successfully retained: PASS
- Completeness of controlled A04 marker collection: PASS
- Completeness across all telemetry scenarios: PARTIAL
- Protection against unauthorized alteration: NOT TESTED
- Cryptographic evidence integrity verification: NOT TESTED

**Requirement Status:** PARTIAL

**Scope and Limitations:**
The available evidence demonstrates successful
collection and retention of selected telemetry.

A04 established complete receipt of its controlled
Syslog markers under the tested conditions.

However, this result does not establish universal
telemetry delivery reliability or protection
against unauthorized modification.

The missing A02 UDP collector marker remains
a documented completeness limitation.

No formal file-hashing, integrity-monitoring,
or chain-of-custody procedure has been verified.

**Outstanding Verification Work:**
- Define integrity criteria for retained telemetry.
- Investigate the missing A02 collector marker.
- Establish file-hashing procedures for evidence.
- Verify hashes before and after evidence handling.
- Review log storage permissions and modification risks.
- Document evidence retention and integrity procedures.

**M6 Planning Implication:**
Introduce lightweight evidence integrity controls
and verify selected telemetry completeness claims
before expanding experimental scope.

---

### NFR-06 — Modularity

**Requirement:** Support the addition, modification, or
replacement of selected telemetry sources and collection
components without unnecessary redesign or disruption
of the existing observability architecture.

**Relevant Milestones:** M2, M3

**Supporting Documentation:**
- Docs/01-Requirements/RC002-Requirements-Specification.md
- Docs/06-Verification/RC002-M2-Minimum-Observability-Topology.md
- Docs/06-Verification/RC002-M3-Telemetry-Collection-Foundation.md
- Docs/10-ADR/ADR-001-Laboratory-Platform-Selection.md

**Verified Observations:**
- M2 established distinct USER, SERVER, and
  OBSERVABILITY network zones.
- M3 deployed TELEMETRY-1 as a centralized
  Syslog collection component.
- SERVER-1 and EDGE-R1 independently forwarded
  telemetry to the centralized collector.
- Source-specific storage separated received
  telemetry by observed source address.
- The architecture supports multiple configured
  telemetry sources.
- Controlled addition or replacement of a telemetry
  source has not been formally evaluated.

**Acceptance Criteria Assessment:**
- Separation of network and telemetry components: PASS
- Multiple configured telemetry sources: PASS
- Independent source configuration: PASS WITH LIMITATION
- Addition of new sources without redesign: NOT TESTED
- Modification without operational disruption: NOT TESTED

**Requirement Status:** PARTIAL

**Scope and Limitations:**
The existing laboratory demonstrates architectural
separation and centralized collection from two
independently configured telemetry sources.

However, successful operation with two sources does
not establish that additional sources can be introduced
without configuration changes, service disruption,
or resource constraints.

Component replacement and collector extensibility
have not been experimentally validated.

**Outstanding Verification Work:**
- Define a controlled telemetry-source addition test.
- Document required configuration changes.
- Verify existing source collection before and after
  introducing a new source.
- Evaluate operational impact using regression tests.
- Record resource and configuration limitations.
- Assess whether collector changes are necessary.

**M6 Planning Implication:**
Evaluate modular source expansion only where it
directly supports outstanding telemetry coverage
requirements and available laboratory resources.

---

### NFR-07 — Scalability

**Requirement:** Support reasonable increases in telemetry
sources and event volumes without unacceptable degradation
of collection reliability or laboratory operations.

**Relevant Milestones:** M3, M5

**Supporting Documentation:**
- Docs/01-Requirements/RC002-Requirements-Specification.md
- Docs/06-Verification/RC002-M3-Telemetry-Collection-Foundation.md
- Docs/06-Verification/RC002-M5-Controlled-Abnormal-Behavior.md
- Docs/07-Results-Analysis/RC002-M5-Consolidated-Results-Analysis.md
- Docs/10-ADR/ADR-001-Laboratory-Platform-Selection.md

**Verified Observations:**
- M3 demonstrated centralized collection from two
  configured telemetry sources.
- M5 A04 generated 100 unique controlled Syslog markers.
- All 100 A04 markers were received without missing
  or duplicate markers under the tested conditions.
- A04 did not constitute sustained high-volume load testing.
- Collector resource consumption was not systematically measured.
- Performance under increasing source counts was not evaluated.
- Collector capacity limits remain unknown.

**Acceptance Criteria Assessment:**
- Basic multi-source collection demonstrated: PASS
- Increasing source-count capacity: NOT TESTED
- Sustained event-volume capacity: NOT TESTED
- Resource utilization under increasing load: NOT TESTED
- Acceptable performance degradation thresholds: NOT TESTED

**Requirement Status:** NOT TESTED

**Scope and Limitations:**
Existing experiments demonstrate successful telemetry
collection under selected laboratory workloads.

They do not establish the collector's maximum capacity,
sustained throughput, or ability to accommodate additional
sources without unacceptable degradation.

The successful A04 marker collection result must not
be interpreted as proof of scalability.

No formal capacity thresholds or resource-based
acceptance criteria have been established.

**Outstanding Verification Work:**
- Define realistic scalability acceptance criteria.
- Establish baseline collector resource utilization.
- Measure CPU, memory, and storage behavior.
- Evaluate controlled increases in event volume.
- Evaluate additional telemetry sources where feasible.
- Measure event loss and operational impact.
- Document laboratory resource limitations.

**M6 Planning Implication:**
Determine whether limited scalability testing is
necessary for the research objectives.

Avoid introducing resource-intensive components
without a demonstrated requirement and a realistic
capacity assessment.

---

### NFR-08 — Documentation Quality

**Requirement:** Maintain clear, accurate, organized,
and sufficiently detailed technical and research
documentation throughout the project.

**Relevant Milestones:** M1, M2, M3, M4, M5

**Supporting Documentation:**
- Docs/01-Requirements/RC002-Requirements-Specification.md
- Docs/04-Telemetry-Design/RC002-Measurement-Framework.md
- Docs/05-Experimental-Methodology/RC002-M5-Controlled-Abnormal-Behavior-Protocol.md
- Docs/06-Verification/RC002-M3-Telemetry-Collection-Foundation.md
- Docs/06-Verification/RC002-M5-Controlled-Abnormal-Behavior.md
- Docs/07-Results-Analysis/RC002-M5-Consolidated-Results-Analysis.md
- Docs/12-Requirements-Traceability/RC002-Requirements-Traceability-Matrix.md

**Verified Observations:**
- Research requirements were formally documented.
- Architecture and telemetry design records were created.
- Experimental procedures and verification outcomes
  were documented across completed milestones.
- M5 results were consolidated into a research analysis.
- Limitations and unresolved observations were recorded.
- The RTM now contains all 19 requirements in the
  correct functional and non-functional section order.
- All 11 unique referenced Markdown documentation paths
  have been checked and confirmed to exist.
- Requirement-status consistency review has progressed
  across all functional and non-functional requirements.
- Screenshot, dataset, and claim-level evidence validation
  remains incomplete.
- Final documentation consistency review remains incomplete.

**Acceptance Criteria Assessment:**
- Organized technical documentation: PASS
- Documented experimental procedures: PASS
- Documented verification outcomes: PASS
- Explicit reporting of limitations: PASS
- Referenced Markdown documentation paths validated: PASS
- Complete evidence-reference validation: PARTIAL
- Final document consistency review: PARTIAL

**Requirement Status:** PASS WITH LIMITATION

**Scope and Limitations:**
The project maintains organized technical and research
documentation supporting milestones M1 through M5.

The RTM now contains all 19 requirements in the correct
functional and non-functional section order.

All 11 unique referenced Markdown documentation paths
have been checked and confirmed to exist.

However, documentation-path validation does not establish
that every supporting artifact is complete or that every
documented claim is substantiated by retained evidence.

Screenshot, dataset, and claim-level evidence validation
remains incomplete.

The final documentation consistency review has also
not been completed.

These limitations prevent an unrestricted assessment
of documentation quality.

**Outstanding Verification Work:**
- Validate screenshot and dataset evidence references.
- Verify that documented findings are supported by
  the corresponding retained evidence.
- Review terminology and requirement-status consistency
  across project documentation.
- Identify missing, incomplete, or inconsistent artifacts.
- Resolve or explicitly document outstanding evidence gaps.
- Complete the final RTM and documentation quality audit.

**M6 Planning Implication:**
Complete a documentation quality and traceability
audit before finalizing M6 scope and acceptance criteria.

---

## 6. Outstanding Research Gaps

The following research gaps were identified through
the requirements traceability review of milestones
M1 through M5.

These gaps represent limitations in demonstrated
capabilities, incomplete verification, or unresolved
experimental observations.

They do not necessarily indicate implementation
failures. Each gap requires further verification
before stronger research claims can be made.

### RG-01 — Incomplete Telemetry Coverage

**Related Requirements:** FR-02, FR-03, FR-04, FR-05, NFR-06

**Current Evidence:**
- M2 established USER, SERVER, and OBSERVABILITY zones.
- M3 demonstrated centralized Syslog collection from
  SERVER-1 and EDGE-R1.
- Source-specific storage and attribution were verified.

**Identified Gap:**
- Multi-site telemetry visibility has not been tested.
- Comprehensive telemetry coverage across all network
  segments has not been demonstrated.
- Additional infrastructure and endpoint sources
  remain outside the verified collection scope.
- Service-specific HTTP telemetry has not been
  independently verified.
- Controlled source expansion has not been tested.

**Research Significance:**
The existing results demonstrate selected centralized
telemetry collection but do not establish comprehensive
enterprise-wide observability.

**Recommended Follow-Up:**
- Define a source and network-segment coverage inventory.
- Identify the minimum additional sources required.
- Evaluate controlled source expansion where feasible.
- Measure source coverage using M-01.
- Preserve the single-site limitation unless a
  multi-site experiment is actually implemented.

**Priority:** HIGH

---

### RG-02 — Incomplete Baseline and Abnormal-Event Differentiation

**Related Requirements:** FR-06, FR-07, FR-08

**Current Evidence:**
- M4 established normal-operation ICMP and HTTP
  measurements through repeated trials.
- M4 documented routine Syslog observations.
- M5 executed four controlled abnormal-behavior
  scenarios across five trials each.
- A01 generated 100 successful HTTP requests.
- A03 demonstrated controlled TCP/22 denial behavior.
- A04 generated 100 unique Syslog markers, all of
  which were observed at the collector.

**Identified Gap:**
- Normal HTTP request rates were not measured using
  a directly comparable method to A01.
- Normal Syslog event rates were not quantified for
  comparison with A04.
- No validated thresholds distinguish normal from
  abnormal activity across the tested scenarios.
- Automated detection or classification performance
  has not been established.
- Event visibility does not independently demonstrate
  reliable abnormal-behavior detection.

**Research Significance:**
The experiments demonstrate that selected controlled
activities can be generated and observed.

However, the available measurements do not establish
how reliably the telemetry distinguishes abnormal
behavior from legitimate operational variation.

**Recommended Follow-Up:**
- Define comparable normal and abnormal measurements.
- Establish repeatable event-count and rate calculations.
- Evaluate differentiation using M-04.
- Document false positives and false negatives if
  a detection rule is subsequently implemented.
- Avoid claiming automated detection until it has
  been explicitly implemented and verified.

**Priority:** HIGH

---

### RG-03 — Time Synchronization and Event Correlation

**Related Requirements:** FR-08, FR-09, NFR-01

**Current Evidence:**
- M3 demonstrated centralized collection of Syslog
  messages from SERVER-1 and EDGE-R1.
- Collected messages contained timestamps and
  source-attribution information.
- M5 used controlled experimental markers to
  support identification of selected events.
- Experimental observations were documented
  across repeated trials.

**Identified Gap:**
- Clock synchronization across telemetry sources
  and the collector has not been verified.
- Timestamp accuracy and consistency have not
  been systematically evaluated.
- Reliable chronological ordering of events
  originating from different devices has not
  been experimentally demonstrated.
- Event delivery latency has not been measured
  using synchronized timestamps.
- A reproducible cross-source event-correlation
  procedure has not been validated.

**Research Significance:**
Centralized event collection does not automatically
establish reliable chronological correlation.

Unsynchronized clocks may produce misleading event
sequences and undermine investigations involving
multiple network devices.

**Recommended Follow-Up:**
- Inspect time configuration on telemetry sources
  and the collector.
- Establish and verify a common time reference
  where technically feasible.
- Record clock offsets and synchronization limitations.
- Define a reproducible event-correlation procedure.
- Evaluate event delivery latency using M-03 only
  after timestamp comparability is established.
- Document any remaining timing uncertainty.

**Priority:** HIGH

---

### RG-04 — Experimental Evidence Completeness and Integrity

**Related Requirements:** FR-07, FR-10, NFR-01, NFR-02,
NFR-05, NFR-08

**Current Evidence:**
- M3 demonstrated centralized Syslog collection
  and persistence.
- M4 retained normal-operation measurements.
- M5 documented four controlled experimental
  scenarios across five trials each.
- A04 generated 100 unique Syslog markers,
  all of which were observed at the collector.
- A02 recorded one missing expected UDP
  collector marker.
- M5 retained individual results and a
  consolidated experimental analysis.
- The RTM maps all 19 requirements to
  supporting documentation.

**Identified Gap:**
- The cause of the missing A02 collector
  marker remains unresolved.
- Selected M5 screenshot and evidence
  records remain incomplete.
- Screenshot, dataset, and claim-level
  evidence validation is unfinished.
- Cryptographic integrity verification
  of retained evidence has not been
  formally demonstrated.
- Independent reproduction of all
  experimental findings remains unverified.

**Research Significance:**
Successful collection in one experiment
does not establish complete or reliable
collection across all experimental conditions.

Similarly, preserving experimental files
does not independently establish their
integrity, completeness, or reproducibility.

**Recommended Follow-Up:**
- Audit retained artifacts against the
  corresponding experimental protocols.
- Investigate the missing A02 marker
  without assuming its cause.
- Identify and document missing or
  inconsistent evidence.
- Establish a lightweight SHA-256
  evidence-hashing procedure.
- Record evidence integrity checks
  and retention procedures.
- Reconcile requirement claims with
  verified supporting artifacts.
- Repeat selected experiments only
  where necessary and feasible.

**Priority:** HIGH

---

### RG-05 — Operational Regression and Recovery Verification

**Related Requirements:** FR-11, NFR-03

**Current Evidence:**
- M2 demonstrated successful inter-zone connectivity
  and HTTP service accessibility.
- M3 verified basic connectivity following
  centralized telemetry configuration.
- M4 established normal-operation ICMP and HTTP
  measurements.
- M5 A01 completed 100 successful HTTP requests.
- M5 A03 demonstrated expected TCP/22 denial
  behavior under a temporary firewall rule.
- The temporary A03 firewall rule was removed
  after testing.
- Transient external connectivity issues were
  documented during A03.

**Identified Gap:**
- A standardized operational regression suite
  has not been formally established.
- Consistent before-and-after connectivity
  verification is incomplete.
- Essential service availability has not been
  verified across every experimental condition.
- The cause of the transient A03 external
  connectivity issue remains unresolved.
- Recovery behavior has not been systematically
  measured across controlled changes.
- The operational impact of telemetry collection
  has not been comprehensively evaluated.

**Research Significance:**
Successful execution of controlled experiments
does not establish that legitimate network
operations remain unaffected.

Without consistent regression measurements,
it may be difficult to distinguish expected
experimental effects from unintended failures.

**Recommended Follow-Up:**
- Define essential connectivity and service
  availability checks.
- Establish expected results for each check.
- Execute regression tests before and after
  selected configuration changes.
- Investigate unexplained connectivity failures.
- Document recovery actions and outcomes.
- Evaluate operational preservation using M-05.
- Retain unresolved limitations where their
  causes cannot be established.

**Priority:** HIGH

---

### RG-06 — Telemetry Security and Access Protection

**Related Requirements:** NFR-04, NFR-05

**Current Evidence:**
- M2 established separate USER, SERVER, and
  OBSERVABILITY network zones.
- M3 configured centralized Syslog collection
  on TELEMETRY-1.
- SERVER-1 and EDGE-R1 forwarded telemetry
  using UDP/TCP port 514.
- Source-specific log storage was demonstrated.
- M5 verified selected firewall denial behavior.

**Identified Gap:**
- Telemetry transport encryption has not been
  implemented or verified.
- Cryptographic sender authentication has not
  been demonstrated.
- Collector network exposure has not been
  formally assessed.
- Collector access controls and stored log
  permissions have not been audited.
- Protection against unauthorized modification
  of collected telemetry remains unverified.

**Research Significance:**
Centralized collection improves telemetry
availability but may introduce security risks
if the collector or stored logs are inadequately
protected.

Source IP attribution does not independently
establish authenticated sender identity.

**Recommended Follow-Up:**
- Review collector network exposure and
  permitted telemetry connections.
- Inspect collector user access and log-file
  permissions.
- Document the confidentiality limitations
  of traditional UDP/TCP Syslog.
- Evaluate practical access restrictions
  appropriate for the laboratory.
- Verify selected security controls
  experimentally.
- Document residual risks and limitations.

**Priority:** HIGH

---

### RG-07 — Modularity and Scalability Validation

**Related Requirements:** NFR-06, NFR-07, FR-03

**Current Evidence:**
- M2 established separate USER, SERVER, and
  OBSERVABILITY network zones.
- M3 demonstrated centralized Syslog collection
  from SERVER-1 and EDGE-R1.
- Source-specific log storage was verified.
- M5 A04 successfully collected all 100
  controlled Syslog markers.
- ADR-001 established a lightweight laboratory
  architecture appropriate for available resources.

**Identified Gap:**
- Controlled addition of a new telemetry source
  has not been experimentally verified.
- Modification or replacement of existing
  telemetry components has not been tested.
- Collector performance under increasing
  source counts remains unknown.
- Sustained event-volume capacity has not
  been measured.
- CPU, memory, and storage utilization under
  increasing telemetry loads remain unmeasured.
- Acceptable performance degradation thresholds
  have not been established.

**Research Significance:**
Successful telemetry collection from two sources
demonstrates basic multi-source functionality,
but does not establish modular expansion or
scalability.

The successful A04 marker collection result
does not demonstrate sustained collector capacity
or reliable operation under increasing workloads.

**Recommended Follow-Up:**
- Define a controlled source-addition experiment
  if required to improve telemetry coverage.
- Verify existing telemetry collection before
  and after introducing an additional source.
- Document configuration changes and operational
  effects.
- Establish baseline collector resource usage.
- Define measurable scalability acceptance
  criteria before conducting load experiments.
- Evaluate limited event-volume increases only
  where feasible within laboratory constraints.
- Document resource limitations and avoid
  unsupported capacity claims.

**Priority:** MEDIUM

---



## 7. M6 Planning Implications

The requirements traceability review identified seven
outstanding research gaps that inform the planning
of milestone M6.

M6 should prioritize measurable verification of
selected unresolved requirements rather than
introducing additional architectural complexity
without a demonstrated research need.

### 7.1 Planning Principles

The following principles will guide M6 planning:

1. **Evidence First:**
   Validate existing experimental evidence and
   document unresolved limitations before
   extending research claims.

2. **Operational Preservation:**
   Establish repeatable regression checks before
   introducing configuration changes or additional
   telemetry sources.

3. **Measurable Outcomes:**
   Define acceptance criteria, expected observations,
   and evidence requirements before executing tests.

4. **Controlled Experimental Scope:**
   Select experiments that directly address
   identified research gaps and requirements.

5. **Resource Awareness:**
   Preserve the lightweight laboratory approach
   established in ADR-001 and avoid unnecessary
   resource-intensive components.

6. **Research Integrity:**
   Distinguish verified observations from
   interpretations, assumptions, and limitations.

7. **Incremental Verification:**
   Implement, test, document, and verify each
   selected change before proceeding to the next.

### 7.2 M6 Scope Selection

The seven research gaps are candidates for
M6 planning, not automatic commitments to
complete every outstanding requirement.

Final M6 scope will be determined after the
remaining evidence audit and consideration of
technical dependencies, research significance,
and available laboratory resources.

### 7.3 Proposed M6 Work Packages

The following work packages provide a provisional
sequence for addressing selected research gaps.

They are planning candidates and do not represent
completed activities or final M6 scope commitments.

#### WP-01 — Evidence Audit and Integrity

**Related Research Gap:** RG-04

**Related Requirements:** FR-10, NFR-01, NFR-02,
NFR-05, NFR-08

**Objective:**
Evaluate the completeness, traceability, and integrity
of evidence retained during milestones M3 through M5.

**Proposed Activities:**
- Inventory retained experimental evidence.
- Compare evidence artifacts against experiment
  protocols and documented results.
- Verify screenshot, dataset, and file references.
- Investigate the missing A02 collector marker.
- Identify missing or inconsistent evidence.
- Establish a lightweight SHA-256 hashing procedure.
- Record unresolved limitations explicitly.

**Proposed Acceptance Criteria:**
- An evidence inventory is produced.
- Referenced artifacts are checked for existence.
- Evidence gaps are recorded and classified.
- Selected retained files have recorded SHA-256 hashes.
- The A02 missing-marker issue is investigated,
  with findings or unresolved uncertainty documented.
- Requirement-to-evidence mappings are updated
  where necessary.

**Expected Deliverables:**
- Evidence inventory and validation record.
- Evidence-integrity verification results.
- Updated traceability findings.
- Documented unresolved evidence limitations.

**Status:** PROPOSED

--- 

#### WP-02 — Operational Regression Baseline

**Related Research Gap:** RG-05

**Related Requirements:** FR-11, NFR-03

**Objective:**
Establish a repeatable operational regression
verification procedure to evaluate legitimate
network connectivity and essential service
availability before and after controlled changes.

**Proposed Activities:**
- Identify essential network connectivity paths.
- Define expected ICMP and HTTP service behavior.
- Verify connectivity between selected network zones.
- Verify HTTP service accessibility.
- Confirm continued Syslog collection from
  existing telemetry sources.
- Establish pre-change and post-change test procedures.
- Document unexpected failures and recovery actions.
- Review the transient external connectivity
  limitation observed during M5 A03.

**Proposed Acceptance Criteria:**
- A documented operational regression checklist exists.
- Each selected test has an expected outcome.
- Baseline test results are recorded.
- Post-change results are compared against
  the corresponding baseline.
- Unexpected failures are documented and investigated.
- Existing telemetry collection is verified
  following selected configuration changes.
- Operational preservation is evaluated using M-05.

**Expected Deliverables:**
- Operational regression test plan.
- Baseline connectivity and service results.
- Post-change regression verification records.
- Documented failures, recovery actions,
  and unresolved operational limitations.

**Dependencies:**
- Existing M2–M5 laboratory configuration.
- Availability of the selected network devices
  and essential services.
- Relevant findings from WP-01.

**Status:** PROPOSED

---

#### WP-03 — Time Synchronization and Event Correlation

**Related Research Gap:** RG-03

**Related Requirements:** FR-08, FR-09, NFR-01

**Objective:**
Establish and verify a common time reference
for selected telemetry sources and the collector,
then evaluate reproducible cross-source event
correlation.

**Proposed Activities:**
- Inspect system time, time zones, and clock
  configuration on SERVER-1, EDGE-R1,
  and TELEMETRY-1.
- Identify available clock synchronization
  mechanisms within the GNS3 laboratory.
- Configure a common time reference where feasible.
- Verify and document clock synchronization status.
- Record observed clock offsets and limitations.
- Generate identifiable controlled events from
  selected telemetry sources.
- Compare source and collector timestamps.
- Develop a reproducible procedure for searching
  and correlating related events.
- Evaluate event delivery latency using M-03
  only when timestamp comparability is established.

**Proposed Acceptance Criteria:**
- Time configuration is documented for all
  selected devices.
- Clock synchronization status is verified
  or its limitations explicitly recorded.
- A common time reference is established
  where technically feasible.
- Controlled events are identifiable in
  retained collector logs.
- A documented procedure supports searching
  and comparing events across selected sources.
- Timestamp uncertainty is recorded.
- Event latency measurements are reported
  only when the required timing conditions
  have been verified.

**Expected Deliverables:**
- Time synchronization configuration record.
- Clock verification and offset observations.
- Controlled event-correlation test results.
- Reproducible event-search procedure.
- Documented timing limitations.

**Dependencies:**
- Existing M3 telemetry collection configuration.
- WP-02 operational regression baseline.
- Availability of a suitable common time reference.
- Relevant evidence findings from WP-01.

**Status:** PROPOSED

---

#### WP-04 — Telemetry Coverage and Controlled Source Expansion

**Related Research Gaps:** RG-01, RG-07

**Related Requirements:** FR-02, FR-03, FR-04,
FR-05, NFR-06, NFR-07

**Objective:**
Assess telemetry coverage across the existing
laboratory and evaluate the feasibility of
incrementally expanding source visibility
without compromising operational stability.

**Proposed Activities:**
- Inventory existing telemetry sources and
  their associated network segments.
- Identify currently observable event categories.
- Map telemetry sources to USER, SERVER,
  and OBS network segments.
- Identify relevant services and devices
  that lack demonstrated telemetry coverage.
- Select one additional telemetry source
  for controlled onboarding, if feasible.
- Document source configuration and
  collector integration requirements.
- Generate identifiable test events from
  the selected additional source.
- Verify event receipt, source identification,
  and persistence at TELEMETRY-1.
- Evaluate the effect of source expansion
  on operational connectivity and stability.
- Record resource constraints and remaining
  telemetry coverage limitations.

**Proposed Acceptance Criteria:**
- A telemetry source and coverage inventory exists.
- Existing source-to-segment mappings are documented.
- Observable and unverified event categories
  are explicitly distinguished.
- At least one additional source is evaluated
  for onboarding feasibility.
- If source onboarding is approved, test events
  are generated and collector receipt verified.
- Any source-expansion configuration changes
  undergo WP-02 regression verification.
- Remaining coverage and scalability limitations
  are documented.
- Source coverage is evaluated using M-01.

**Expected Deliverables:**
- Telemetry coverage inventory and gap analysis.
- Source expansion feasibility assessment.
- Additional source configuration and test
  evidence, if onboarding is performed.
- Operational regression verification results.
- Updated coverage and scalability findings.

**Dependencies:**
- Existing M3 telemetry collection infrastructure.
- WP-01 evidence audit findings.
- WP-02 operational regression baseline.
- Available GNS3 host resources.
- Relevant WP-03 time synchronization findings
  where cross-source timing is evaluated.

**Status:** PROPOSED

---

#### WP-05 — Normal-versus-Abnormal Event Differentiation

**Related Research Gap:** RG-02

**Related Requirements:** FR-06, FR-07, FR-08

**Objective:**
Evaluate whether existing and newly collected
telemetry can support measurable, reproducible
distinctions between selected normal and
controlled abnormal network behaviors.

**Proposed Activities:**
- Review M4 normal-behavior experiments N01–N04.
- Review M5 abnormal-behavior experiments A01–A04.
- Identify comparable normal and abnormal
  activity categories.
- Identify measurable features such as event
  counts, event rates, connection outcomes,
  and repeated activity patterns.
- Identify missing baseline measurements
  that prevent valid comparisons.
- Design controlled comparison trials using
  consistent observation windows.
- Collect additional baseline evidence
  where necessary and feasible.
- Evaluate candidate differentiation rules
  using recorded experimental observations.
- Document false-positive and false-negative
  outcomes where suitable labeled trials exist.
- Record limitations affecting interpretation
  and reproducibility.

**Proposed Acceptance Criteria:**
- Selected normal and abnormal activity
  categories are explicitly defined.
- At least one comparable activity pair
  is identified for controlled evaluation.
- Comparison trials use documented observation
  windows and measurement procedures.
- Measured differences are supported by
  retained experimental evidence.
- Candidate differentiation rules are evaluated
  against available labeled observations.
- Unsupported detection claims are avoided.
- Differentiation findings are evaluated
  using M-04.
- Unresolved baseline and measurement
  limitations are documented.

**Expected Deliverables:**
- Normal-versus-abnormal comparison protocol.
- Selected telemetry feature definitions.
- Controlled comparison results.
- Candidate differentiation rule assessment.
- Documented detection limitations and
  recommendations for future research.

**Dependencies:**
- Existing M4 and M5 experimental records.
- WP-01 evidence audit findings.
- WP-02 operational regression baseline.
- Relevant WP-03 timestamp verification results.
- WP-04 telemetry coverage findings where
  additional sources are used.

**Status:** PROPOSED

---

#### WP-06 — Telemetry Security and Access Protection

**Related Research Gap:** RG-06

**Related Requirements:** NFR-04, NFR-05

**Objective:**
Assess the security of telemetry transmission,
collection, access, and storage within the
existing lightweight GNS3 laboratory.

**Proposed Activities:**
- Review existing rsyslog UDP/TCP 514
  configuration and listening interfaces.
- Identify devices and network segments
  permitted to reach TELEMETRY-1.
- Review applicable firewall rules and
  network access restrictions.
- Inspect permissions and ownership of
  retained telemetry files and directories.
- Identify accounts with access to
  collected telemetry.
- Evaluate whether source authentication
  and transport encryption are configured.
- Assess the feasibility of additional
  lightweight telemetry protection controls.
- Evaluate proposed controls against
  operational and resource constraints.
- Document residual security risks
  and limitations.

**Proposed Acceptance Criteria:**
- Telemetry collection interfaces and
  exposed ports are documented.
- Existing network access restrictions
  are inspected and recorded.
- Log directory and file permissions
  are verified.
- Accounts with telemetry access
  are identified where feasible.
- Transport encryption and source
  authentication status are explicitly
  documented.
- At least one feasible security
  improvement is evaluated.
- Any implemented security change
  undergoes WP-02 regression verification.
- Unresolved security limitations
  are recorded without unsupported
  protection claims.

**Expected Deliverables:**
- Telemetry security configuration assessment.
- Collector access and permissions review.
- Transport protection findings.
- Security improvement feasibility assessment.
- Regression verification evidence
  for any implemented changes.
- Documented residual risks
  and recommendations.

**Dependencies:**
- Existing M3 telemetry collection infrastructure.
- WP-01 evidence audit findings.
- WP-02 operational regression baseline.
- Available laboratory resources.
- Existing network segmentation
  and firewall configuration.

**Status:** PROPOSED

---

### 7.4 Proposed Execution Sequence and Dependencies

The proposed M6 work packages should be
executed according to their technical dependencies,
research significance, and available resources.

**Stage 1 — Establish Research Readiness**

- WP-01: Audit existing experimental evidence.
- WP-02: Establish repeatable operational
  regression verification procedures.

These activities provide the evidence and
operational foundation for subsequent testing.

**Stage 2 — Strengthen Telemetry Verification**

- WP-03: Verify time synchronization and
  cross-source event correlation.
- WP-04: Assess telemetry coverage and
  evaluate controlled source expansion.

WP-04 source onboarding remains conditional
on laboratory feasibility and approved scope.

**Stage 3 — Evaluate Research Capabilities**

- WP-05: Evaluate measurable differences
  between normal and controlled abnormal events.
- WP-06: Assess telemetry security and
  evaluate feasible protection improvements.

WP-05 and WP-06 may be scheduled independently
once their relevant prerequisites are satisfied.

**Execution Constraints**

- Each selected experiment must have a
  documented protocol and acceptance criteria.
- Configuration changes must undergo
  appropriate operational regression checks.
- Resource-intensive architectural expansion
  must not be assumed necessary.
- Unresolved evidence or synchronization
  limitations must be documented.
- Work packages may be deferred when their
  prerequisites cannot be satisfied.

**Planning Status:** PROPOSED

---

### 7.5 M6 Scope-Selection Criteria

The proposed work packages will be evaluated
before finalizing the M6 experimental scope.

Selection will consider the following criteria:

**1. Research Significance**
- Does the work package address a high-priority
  research gap?
- Does it strengthen the reliability or
  interpretability of RC-002 findings?

**2. Requirements Traceability**
- Are the relevant functional or non-functional
  requirements explicitly identified?
- Can the proposed work produce evidence
  relevant to those requirements?

**3. Technical Dependencies**
- Are the necessary configurations, services,
  and experimental prerequisites available?
- Can unresolved dependencies be addressed
  within the proposed milestone?

**4. Experimental Measurability**
- Can the objective be evaluated using
  observable and reproducible measurements?
- Can meaningful acceptance criteria
  be established before testing?

**5. Resource Feasibility**
- Can the work be performed using the
  existing lightweight GNS3 environment?
- Does it avoid unnecessary computational,
  storage, and architectural overhead?

**6. Operational Risk**
- Could the activity disrupt existing
  connectivity or telemetry collection?
- Are appropriate regression and recovery
  procedures available?

**7. Evidence Readiness**
- Are the required baseline records,
  protocols, and supporting artifacts available?
- Can identified evidence gaps be resolved
  or explicitly accounted for?

### Scope-Selection Decision Rules

Each proposed work package will receive
one of the following planning decisions:

- **SELECTED:** Included in the approved
  M6 experimental scope.
- **CONDITIONAL:** Included only if specified
  prerequisites or feasibility conditions
  are satisfied.
- **DEFERRED:** Reserved for a later milestone
  or future research.
- **REJECTED:** Excluded because it does not
  adequately support the research objectives
  or cannot be justified within constraints.

Selection decisions must include a documented
rationale and any relevant dependencies.

No work package will be considered completed
solely because it appears in this planning matrix.

**Planning Status:** PROPOSED

---

### 7.6 Preliminary Work-Package Selection

The following assessments apply the criteria
defined in Section 7.5.

Selection decisions remain preliminary until
the complete M6 scope is reviewed and approved.

#### WP-01 — Evidence Audit and Integrity

**Preliminary Decision:** SELECTED

**Selection Rationale:**
WP-01 directly addresses RG-04 and supports
the reliability of existing M3–M5 research
evidence.

The work is primarily documentation-based,
requires limited additional computational
resources, and introduces minimal operational
risk when performed using read-only checks.

**Relevant Requirements:**
FR-10, NFR-01, NFR-02, NFR-05, NFR-08

**Key Dependencies:**
- Availability of existing experimental records.
- Access to referenced evidence artifacts.
- Identification of missing or inconsistent files.

**Scope Boundary:**
WP-01 will evaluate evidence completeness,
traceability, and integrity.

It will not assume that missing evidence
can be reconstructed or that unexplained
experimental outcomes can be conclusively
resolved.

**Expected Decision Outcome:**
Establish an auditable evidence baseline
and identify limitations requiring further
investigation.

---

#### WP-02 — Operational Regression Baseline

**Preliminary Decision:** SELECTED

**Selection Rationale:**
WP-02 directly addresses RG-05 and provides
a repeatable method for evaluating operational
stability before and after experimental changes.

The proposed tests can use existing GNS3
devices, connectivity paths, HTTP services,
and Syslog collection infrastructure.

The work supports M-05 operational regression
measurement and reduces the risk of
misattributing unrelated network failures
to controlled experimental activities.

**Relevant Requirements:**
FR-11, NFR-03

**Key Dependencies:**
- Availability of the existing M2–M5 laboratory.
- Functional connectivity and essential services.
- Relevant evidence findings from WP-01.
- Defined expected outcomes for selected tests.

**Scope Boundary:**
WP-02 will establish and apply repeatable
operational regression checks for selected
network paths and services.

It will not claim comprehensive enterprise
availability testing, long-term reliability,
or guaranteed recovery from all failures.

**Expected Decision Outcome:**
Establish a documented operational baseline
that supports safe execution and verification
of subsequent M6 experiments.

---

#### WP-03 — Time Synchronization and Event Correlation

**Preliminary Decision:** SELECTED

**Selection Rationale:**
WP-03 directly addresses RG-03 and strengthens
the reliability of event chronology and
cross-source telemetry investigation.

Existing M3–M5 evidence demonstrates Syslog
collection, but synchronized device clocks
and reproducible cross-source correlation
have not yet been verified.

The proposed assessment can begin using
existing laboratory devices and lightweight
time-verification tools.

**Relevant Requirements:**
FR-08, FR-09, NFR-01

**Key Dependencies:**
- Availability of SERVER-1, EDGE-R1,
  and TELEMETRY-1.
- WP-02 operational regression baseline.
- Access to device time configuration.
- A feasible common time reference for
  synchronization-dependent experiments.

**Scope Boundary:**
WP-03 will inspect device clocks, evaluate
synchronization feasibility, and test
cross-source event correlation.

If reliable synchronization cannot be
established, the study will document
the limitation rather than report
unsupported event-latency measurements.

**Feasibility Condition:**
Quantitative cross-device event latency
measurement using M-03 requires verified
timestamp comparability.

**Expected Decision Outcome:**
Determine whether the laboratory supports
reproducible cross-source event chronology
and identify any remaining timing limitations.

---

#### WP-04 — Telemetry Coverage and Controlled Source Expansion

**Preliminary Decision:** CONDITIONAL

**Selection Rationale:**
WP-04 addresses RG-01 and RG-07 by evaluating
existing telemetry coverage and the feasibility
of expanding observable event sources.

The existing laboratory has demonstrated
telemetry collection from SERVER-1 and EDGE-R1,
but comprehensive coverage across network
segments and event categories remains unverified.

A coverage assessment is justified using
existing evidence. Additional source onboarding
requires a separate feasibility decision.

**Relevant Requirements:**
FR-02, FR-03, FR-04, FR-05, NFR-06, NFR-07

**Key Dependencies:**
- WP-01 evidence audit findings.
- WP-02 operational regression baseline.
- Existing M3 telemetry infrastructure.
- Available host memory, CPU, and storage.
- Identification of a suitable additional source.

**Scope Boundary:**
WP-04 will assess current telemetry coverage
and identify observable and unverified sources.

Controlled onboarding of an additional source
will occur only if it offers measurable
research value and can be supported without
unacceptable operational or resource impact.

The work will not claim comprehensive
enterprise-wide telemetry coverage or
production-scale scalability.

**Feasibility Conditions:**
- Existing laboratory stability is verified.
- A suitable additional source is identified.
- Expected research evidence is defined.
- Resource requirements are acceptable.
- Regression and recovery procedures are available.

**Expected Decision Outcome:**
Produce a defensible telemetry coverage
assessment and determine whether controlled
source expansion should proceed during M6
or be deferred to a later milestone.

---

#### WP-05 — Normal-versus-Abnormal Event Differentiation

**Preliminary Decision:** SELECTED

**Selection Rationale:**
WP-05 directly addresses RG-02 and advances
RC-002 from basic telemetry collection toward
evidence-based differentiation of selected
network behaviors.

M4 established initial normal-behavior
observations, while M5 introduced controlled
abnormal-behavior experiments.

However, comparable observation windows,
event-rate baselines, and validated
differentiation thresholds remain incomplete.

The proposed work can use existing laboratory
infrastructure and lightweight analysis tools.

**Relevant Requirements:**
FR-06, FR-07, FR-08

**Key Dependencies:**
- Availability of M4 and M5 experimental records.
- WP-01 evidence audit findings.
- WP-02 operational regression baseline.
- Comparable normal and abnormal activity data.
- WP-03 timing verification where required
  for time-dependent measurements.

**Scope Boundary:**
WP-05 will evaluate whether selected measurable
telemetry features distinguish controlled
normal and abnormal network behaviors.

It will not claim comprehensive intrusion
detection, real-world attack classification,
or production-ready detection capability.

**Feasibility Conditions:**
- At least one comparable normal-abnormal
  activity pair is identified.
- Consistent observation windows are defined.
- Measurable telemetry features are available.
- Relevant experimental evidence can be retained.
- Candidate differentiation rules can be
  evaluated against labeled observations.

**Expected Decision Outcome:**
Determine whether selected telemetry features
support reproducible differentiation and
identify limitations requiring additional
baseline research or future experiments.

---

#### WP-06 — Telemetry Security and Access Protection

**Preliminary Decision:** SELECTED

**Selection Rationale:**
WP-06 directly addresses RG-06 by evaluating
the security of the existing telemetry
collection infrastructure.

M3 demonstrated Syslog collection using
UDP/TCP port 514, but transport encryption,
source authentication, collector access
restrictions, and retained-log permissions
have not been comprehensively verified.

A configuration and access-control assessment
can be performed using existing laboratory
devices and lightweight administrative tools.

**Relevant Requirements:**
NFR-04, NFR-05

**Key Dependencies:**
- Existing M3 telemetry infrastructure.
- WP-01 evidence audit findings.
- WP-02 operational regression baseline.
- Access to relevant collector and
  source-device configurations.
- Availability of existing firewall
  and file-permission information.

**Scope Boundary:**
WP-06 will assess telemetry transmission,
collector exposure, access restrictions,
and retained-log protection.

The assessment will distinguish verified
security controls from absent, unverified,
or unsupported protections.

It will not claim end-to-end telemetry
confidentiality, authenticated transport,
or production-grade security without
appropriate experimental evidence.

**Implementation Condition:**
Additional security controls will be
implemented only when their feasibility,
research value, and operational impact
have been evaluated.

Any implemented changes must undergo
WP-02 regression verification.

**Expected Decision Outcome:**
Establish a documented telemetry security
baseline, identify residual risks, and
determine whether selected protection
improvements should be implemented during
M6 or deferred.

---

### 7.7 Consolidated Preliminary M6 Scope

The preliminary M6 scope is based on the
research gaps, requirements traceability,
technical dependencies, and feasibility
criteria documented in Sections 6 and 7.

| Work Package | Preliminary Decision | Scope Qualification |
|---|---|---|
| WP-01 — Evidence Audit and Integrity | SELECTED | Audit existing evidence and document unresolved limitations. |
| WP-02 — Operational Regression Baseline | SELECTED | Establish repeatable checks for selected network paths and services. |
| WP-03 — Time Synchronization and Event Correlation | SELECTED | Verify clocks and correlation; latency measurement requires comparable timestamps. |
| WP-04 — Telemetry Coverage and Source Expansion | CONDITIONAL | Assess coverage; additional source onboarding requires feasibility approval. |
| WP-05 — Normal-versus-Abnormal Event Differentiation | SELECTED | Evaluate selected comparable behaviors; dependent on suitable experimental data. |
| WP-06 — Telemetry Security and Access Protection | SELECTED | Assess existing protections; implementing new controls requires separate justification. |

### Scope Interpretation

A SELECTED decision indicates that the
work package is proposed for inclusion in M6.

It does not guarantee that every proposed
activity or acceptance criterion can be
completed successfully.

A CONDITIONAL decision indicates that
execution depends on documented feasibility,
technical prerequisites, and research value.

The preliminary scope prioritizes:

1. Evidence integrity and operational stability.
2. Reliable event chronology and correlation.
3. Measurable differentiation of selected behaviors.
4. Telemetry security assessment.
5. Coverage expansion only when justified.

### Final Scope Approval

Before M6 execution begins, the preliminary
scope must be reviewed against:

- Existing laboratory availability.
- Evidence audit findings.
- Operational regression readiness.
- Experimental measurability.
- Available computational resources.
- Research objectives and milestone boundaries.

The final approved scope will be recorded
in the M6 experimental planning documentation.

**Overall Planning Status:** PRELIMINARY

---