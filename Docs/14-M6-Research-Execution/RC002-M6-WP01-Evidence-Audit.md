# RC-002 — M6 WP-01 Evidence Audit and Integrity Report

**Project:** Project Aegis — RC-002 Enterprise Network Security Observability and Telemetry  
**Milestone:** M6 — Research Execution  
**Work Package:** WP-01 — Evidence Audit and Integrity  
**Audit Scope:** M3, M4, and M5  
**Status:** IN PROGRESS  
**Report Date:** 2026-10-12

---

## 1. Purpose and Scope

This report documents the systematic review of experimental evidence generated during milestones M3–M5 of RC-002.

The audit evaluates:

- **Evidence completeness:** Whether expected experimental results, screenshots, packet captures, and supporting records are available.
- **Evidence consistency:** Whether reported findings agree with retained experimental datasets and observations.
- **Evidence integrity:** Whether retained artifacts can be identified, fingerprinted, and preserved for future verification.
- **Research reproducibility:** Whether the available records sufficiently describe the experiments and their outcomes.
- **Research limitations:** Whether unresolved gaps, measurement constraints, and exceptions are accurately documented.

The audit supports the RC-002 Requirements Traceability Matrix (RTM) and informs subsequent M6 research-execution work packages.

### 1.1 Audit Boundaries

This work package reviews previously collected evidence. It does not independently repeat M3–M5 experiments or establish new detection capabilities.

A successful audit confirms the quality and traceability of available evidence; it does not automatically establish that every RC-002 functional or non-functional requirement has been satisfied.

### 1.2 Evidence Integrity Principle

SHA-256 fingerprints are used to verify file consistency between observed copies and recorded reference fingerprints.

Matching fingerprints support byte-for-byte consistency at the time of verification. They do not independently establish when an artifact was originally created or whether it was modified before its first recorded fingerprint.

---

## 2. M3–M5 Evidence Inventory and Availability Assessment

### 2.1 M3 — Centralized Telemetry Evidence

The M3 evidence inventory includes six retained screenshots under `Images/M3-Evidence/`.

The previously audited evidence references include:

- `M3-02-SERVER1-Centralized-Telemetry.png`
- `M3-03-EDGER1-Centralized-Telemetry.png`
- `M3-04-Multi-Source-Telemetry-Attribution.png`
- `M3-05-Post-Reboot-Telemetry-Persistence.png`

These artifacts support the documented centralized telemetry configuration, source attribution, and post-reboot persistence observations.

**Audit limitation:** File existence and current SHA-256 fingerprints were checked for selected artifacts, but the visual contents of all six screenshots have not yet been independently verified during WP-01.

**Assessment:** AVAILABLE — VISUAL CONTENT VERIFICATION INCOMPLETE.

### 2.2 M4 — Normal Baseline Evidence

The M4 evidence inventory includes four baseline scenarios:

| Scenario | Measurement or Observation | Retained Evidence |
|---|---|---|
| N01 — ICMP Interzone | Five trials; mean RTT 3.388 ms; no reported packet loss | Trial CSV and five PCAP captures |
| N02 — HTTP Application | Five trials; mean completion time 0.0097632 seconds; HTTP 200 responses | Trial CSV and five PCAP captures |
| N03 — Internet ICMP | Five trials; mean RTT 116.3288 ms; no reported packet loss | Trial CSV and five PCAPNG captures |
| N04 — System Activity | Two source-attribution observations involving SERVER-1 and EDGE-R1 | Trial CSV and supporting text records |

Supporting summary documents include:

- `results/M4-Normal-Baseline/M4-Baseline-Summary.csv`
- `results/M4-Normal-Baseline/M4-Observations.md`

**Audit limitations:** N04 does not establish a quantitative normal Syslog event rate. Recorded collector observation times and source-event timestamps also exhibit a discrepancy requiring investigation before reliable cross-source temporal correlation can be claimed.

**Assessment:** AVAILABLE — BASELINE COVERAGE AND TIME-CORRELATION LIMITATIONS.

### 2.3 M5 — Controlled Abnormal Behavior Evidence

M5 contains four controlled experimental scenarios:

| Scenario | Trials | Evidence Assessment |
|---|---:|---|
| A01 — Connection Frequency | 5 | Result report, trial CSV, screenshots, and five independently inspected packet captures available |
| A02 — Destination and Service Diversity | 5 | Result report, trial CSV, and screenshots available; unresolved T02 UDP/514 marker observation |
| A03 — Controlled Denied Access | 5 | Result report, trial CSV, and screenshots available; operational recovery and comparator limitations remain |
| A04 — Infrastructure Event Burst | 5 | Result report, trial CSV, and screenshots available; trial exceptions, protocol documentation, and timestamp limitations remain |

The four M5 result reports were reviewed against their corresponding trial-summary CSV records, with no numerical inconsistencies identified in the reviewed fields.

The evidence inventory identified 87 M5 screenshots: four associated with A01, twenty with A02, thirty-three with A03, and thirty with A04.

Screenshot inventory counts establish file availability, not independent verification of every image's contents.

**Assessment:** AVAILABLE WITH LIMITATIONS — EVIDENCE COMPLETENESS AND MEASUREMENT GAPS REMAIN.

### 2.4 A01 Packet-Capture Recovery

Five A01 packet captures were discovered under the M4 N03 dataset directory rather than the intended M5 A01 directory.

All five captures were copied to:

`datasets/M5-Controlled-Abnormal-Behavior/A01-Connection-Frequency/`

Each copied file passed SHA-256 comparison against its previously recorded original fingerprint.

The five verified captures were committed to Git as commit `46f2183` and successfully pushed to `origin/main`.

The original captures were preserved.

**Assessment:** RECOVERY COMPLETE — COPY INTEGRITY VERIFIED AND GIT PRESERVATION CONFIRMED.

The recorded fingerprints establish consistency between the audited copies and reference files at verification time, not an uninterrupted chain of custody from initial experiment execution.

---

## 3. Evidence Consistency, Integrity, and Research Limitations

### 3.1 M3 — Centralized Telemetry

The retained M3 evidence supports the documented implementation of centralized Syslog collection and source attribution.

The audit identified the following limitations:

- The visual contents of all six M3 screenshots have not yet been independently reviewed during WP-01.
- Availability of screenshots does not, by itself, establish complete telemetry coverage.
- Cross-source timestamp synchronization has not been demonstrated.

**Audit disposition:** Evidence available; visual verification and time-correlation assurance remain incomplete.

### 3.2 M4 — Normal Baseline

The reviewed M4 baseline summaries and trial datasets are consistent with the reported measurements for N01, N02, and N03.

N04 provides categorical evidence of Syslog source attribution but does not establish a quantitative normal event-generation or collection rate.

A timestamp discrepancy was identified between the recorded collector observation window and the source-event timestamps.

This discrepancy requires investigation. It must not be treated as conclusive proof of clock misconfiguration without additional verification.

**Audit disposition:** Baseline measurements retained; quantitative Syslog baseline and timestamp-correlation limitations remain.

### 3.3 M5 A01 — Connection Frequency

Independent packet-level inspection of the five recovered A01 captures supports the following aggregate observations:

| Metric | Verified Observation |
|---|---:|
| Trials | 5 |
| HTTP GET requests | 100 |
| HTTP 200 responses | 100 |
| TCP conversations | 100 |
| Filtered TCP packets | 1,000 |
| Filtered TCP bytes | 132,800 |

Each trial contains 20 HTTP transactions and 20 corresponding TCP conversations.

The captures support the reported increase in transaction count relative to the M4 N02 single-transaction trial structure.

However, the experiment does not independently establish malicious behavior, automated detection, or a statistically validated detection threshold.

The five PCAPNG files contain no recorded capture-interface statistics entries. Consequently, the reported zero capture drops cannot be independently verified from retained PCAPNG interface statistics.

All five recovered copies passed SHA-256 comparison against previously recorded reference fingerprints and were preserved in Git commit `46f2183`.

**Audit disposition:** Packet-level findings verified; capture-drop verification and detection claims remain limited.

### 3.4 M5 A02 — Destination and Service Diversity

The A02 results describe five trials involving internal and external destinations and multiple protocols or services.

Collector-side confirmation of the UDP/514 marker was documented in four of the five trials.

For T02, the expected UDP/514 marker was absent from the retained filtered capture and collector observation. The underlying cause remains unresolved.

The available evidence does not justify classifying this absence as a confirmed transmission failure.

The retained trial-summary records also lack quantitative packet-count, byte-count, and duration measurements.

**Audit disposition:** Partial evidence consistency; unresolved T02 observation and quantitative measurement gaps.

### 3.5 M5 A03 — Controlled Denied Access

The A03 results document five controlled trials using a narrowly scoped temporary firewall DROP rule.

Across the five trials, the reports record:

- 25 connection attempts affected by the rule.
- 25 corresponding connection timeouts.
- 50 dropped packets in the firewall counters.
- 3,000 dropped bytes in the firewall counters.

The evidence supports controlled denial behavior and documented restoration testing.

Limitations include transient external connectivity variation, incomplete normal-versus-denied comparator coverage, and the need to distinguish firewall counter observations from broader network-security detection capability.

**Audit disposition:** Controlled denial observations supported; comparator and operational-regression limitations remain.

### 3.6 M5 A04 — Infrastructure Event Burst

The A04 results document five trials involving 20 unique benign Syslog markers per trial.

The retained records report:

- 100 intended markers.
- 100 received markers.
- 100 unique received markers.
- Zero missing or duplicate markers in the reported trial observations.

These results support successful marker collection within the documented trial conditions.

However, T01 and T04 contain recorded operational exceptions. Some supporting screenshots are missing or insufficiently clear, and the frozen trial protocol does not fully capture the reconstructed execution procedure.

Clock synchronization was not verified, so precise event-delivery latency cannot be established.

The experiment also lacks a quantitative M4 normal Syslog event-rate comparator.

**Audit disposition:** Reported marker counts internally consistent; operational exceptions, documentation gaps, and timing limitations remain.

### 3.7 Integrity and Reproducibility Conclusions

The audit establishes that substantial M3–M5 evidence is retained and that the recovered A01 captures are consistent with their recorded reference fingerprints.

It does not establish a complete historical chain of custody for every artifact.

The following matters remain open:

- Independent visual inspection of selected screenshots.
- Investigation of source and collector timestamp discrepancies.
- Resolution or explicit limitation of the A02 T02 UDP/514 observation.
- Quantitative normal-versus-abnormal comparison where currently unsupported.
- Verification of operational recovery and telemetry security requirements.
- Continued preservation of evidence references and experimental exceptions.

These findings inform the remaining M6 work packages and must be reflected accurately in subsequent requirements-traceability updates.

---

## 4. Audit Findings and Corrective-Action Register

### 4.1 Findings Classification

The WP-01 audit classifies outstanding issues according to their effect on research validity, evidence completeness, reproducibility, and requirements traceability.

**HIGH:** A gap that materially limits the strength of a research conclusion, requirement verification, or reproducibility claim.

**MEDIUM:** A gap that reduces evidence quality or completeness but does not invalidate the principal observations already supported by retained records.

A finding is marked **CLOSED** only when its corrective action has been completed and independently verified.

### 4.2 Corrective-Action Register

| Finding ID | Finding | Priority | Required Corrective Action | Status |
|---|---|---|---|---|
| WP01-F01 | A01 captures were stored under the M4 N03 directory | MEDIUM | Copy captures into M5 A01, verify SHA-256 fingerprints, and preserve them in Git | CLOSED |
| WP01-F02 | M3 screenshot contents have not all been independently inspected | MEDIUM | Inspect selected screenshots and document whether they support their stated claims | OPEN |
| WP01-F03 | M4 N04 lacks a quantitative normal Syslog event-rate baseline | HIGH | Establish a controlled, measurable normal event-rate comparator | OPEN |
| WP01-F04 | Source and collector timestamps show an unresolved discrepancy | HIGH | Verify clocks, time zones, and timestamp interpretation before correlation analysis | OPEN |
| WP01-F05 | A01 PCAPNG files lack capture-interface statistics entries | MEDIUM | Qualify capture-drop claims and define verifiable capture-quality measurements | OPEN |
| WP01-F06 | A02 T02 UDP/514 marker was not confirmed in retained evidence | HIGH | Investigate the observation gap or formally document it as unresolved | OPEN |
| WP01-F07 | A02 trial records lack quantitative packet, byte, and duration metrics | MEDIUM | Define and collect measurable traffic-diversity observations | OPEN |
| WP01-F08 | A03 lacks a complete normal-versus-denied comparator | HIGH | Establish controlled comparator evidence and verify operational recovery | OPEN |
| WP01-F09 | A04 contains trial exceptions and incomplete supporting documentation | MEDIUM | Reconcile trial records, identify missing evidence, and preserve explicit limitations | OPEN |
| WP01-F10 | A04 event-delivery latency cannot be established without verified clock synchronization | HIGH | Validate time synchronization before making quantitative latency claims | OPEN |
| WP01-F11 | Normal-versus-abnormal detection thresholds have not been validated | HIGH | Define and test measurable differentiation criteria without assuming malicious intent | OPEN |
| WP01-F12 | Telemetry security and access protections remain incompletely verified | HIGH | Conduct authorized configuration and access-control verification | OPEN |

### 4.3 Closed Finding — WP01-F01

The five A01 packet captures were recovered from their previous M4 N03 location and copied into the intended M5 A01 directory.

The copied files were checked against previously recorded SHA-256 fingerprints, and all five comparisons passed.

The captures were committed as `46f2183` and pushed successfully to GitHub.

The original copies were retained.

**Closure basis:** Correct destination confirmed, five SHA-256 comparisons passed, local Git commit created, and remote push confirmed.

**Residual limitation:** Historical integrity before the first recorded fingerprints remains unproven.

### 4.4 Open Findings and Research Dependencies

The remaining findings require a combination of evidence inspection, controlled measurements, operational verification, and explicit documentation of unresolved limitations.

Their planned treatment is:

- **WP-01 — Evidence Audit and Integrity:** Review retained evidence, identify unsupported claims, and document unresolved gaps.
- **WP-02 — Operational Regression Baseline:** Address operational recovery and regression-verification gaps.
- **WP-03 — Time Synchronization and Event Correlation:** Investigate timestamp discrepancies and establish defensible correlation measurements.
- **WP-04 — Telemetry Coverage and Controlled Source Expansion:** Address coverage gaps if justified by the requirements review and available resources.
- **WP-05 — Normal-versus-Abnormal Event Differentiation:** Establish measurable comparator conditions and evaluate differentiation criteria.
- **WP-06 — Telemetry Security and Access Protection:** Verify authorized telemetry access and relevant protection controls.

Assignment to a work package is a proposed treatment, not evidence that a finding has been resolved.

### 4.5 Closure Criteria

A corrective-action finding may be closed only when:

1. The corrective action has been performed.
2. Supporting evidence is retained and identifiable.
3. The evidence supports the stated result without exceeding its measurement limitations.
4. Any remaining exceptions are explicitly documented.
5. Relevant requirements-traceability entries are updated where necessary.

Findings that cannot be resolved within M6 must remain open or be formally classified as accepted limitations with justification.

---

## 5. WP-01 Audit Summary and Next Actions

### 5.1 Audit Summary

The M6 WP-01 audit reviewed retained evidence from milestones M3, M4, and M5 to assess evidence availability, consistency, integrity, and research limitations.

The audit confirmed that:

- M3 centralized telemetry screenshots and supporting evidence references are retained.
- M4 baseline measurements and supporting experimental datasets are available.
- M5 A01–A04 result reports and trial-summary records are available.
- Five A01 packet captures were independently inspected at packet level.
- The five A01 captures were recovered into the correct M5 directory and passed SHA-256 copy-integrity verification.
- The recovered A01 captures were preserved in Git commit `46f2183` and pushed successfully to GitHub.
- Several measurement, operational, timestamp-correlation, and evidence-completeness limitations remain unresolved.

These observations support continued research execution but do not establish complete satisfaction of all RC-002 requirements.

### 5.2 Corrective-Action Status

| Classification | Number of Findings |
|---|---:|
| CLOSED | 1 |
| OPEN | 11 |
| TOTAL | 12 |

The closed finding concerns A01 packet-capture recovery and preservation.

The remaining findings require additional verification, controlled experimentation, documentation, or formal acceptance of limitations.

### 5.3 Research Implications

The audit provides a stronger evidence foundation for RC-002 by distinguishing experimentally supported observations from unverified assumptions.

The principal research implications are:

1. **Evidence preservation:** Retained artifacts should be organized, fingerprinted, and referenced consistently.
2. **Measurement validity:** Claims about capture drops, event latency, and detection effectiveness require appropriate supporting measurements.
3. **Temporal correlation:** Cross-source event timing should not be treated as reliable until clock and timestamp interpretation are verified.
4. **Experimental comparability:** Normal and controlled-abnormal conditions require clearly defined, measurable comparison criteria.
5. **Operational resilience:** Security-control experiments should include evidence of service restoration and post-test regression checks.
6. **Requirements traceability:** The RTM should distinguish verified requirements from partial results and unresolved limitations.

### 5.4 Recommended Next Actions

The next WP-01 activities are:

- Review selected retained screenshots to assess whether their contents support the associated experimental claims.
- Establish a consolidated SHA-256 evidence manifest for selected critical artifacts.
- Confirm evidence paths and references against the Requirements Traceability Matrix.
- Record unresolved evidence gaps without converting them into unsupported PASS claims.
- Update the corrective-action register as verification progresses.

After the remaining WP-01 checks, proceed with the selected M6 work packages according to their documented dependencies.

### 5.5 Audit Status

**WP-01 STATUS: IN PROGRESS**

The initial evidence inventory, consistency review, A01 capture recovery, and corrective-action register have been documented.

Final WP-01 closure remains dependent on outstanding evidence verification, integrity-manifest preparation, and requirements-traceability review.

No unresolved finding should be represented as closed solely because this report has been prepared.