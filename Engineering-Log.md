# RC-002 Engineering Log

## Project Initiation

**Project:** RC-002 â€” Enterprise Network Security Observability and Telemetry
**Project Aegis Phase:** OBSERVE
**Stage:** Research Design / Requirements Development

### Activities Completed

- Defined the RC-002 research problem.
- Developed the primary research question.
- Defined four supporting research questions.
- Established the primary research objective.
- Defined six specific research objectives.
- Established preliminary project scope.
- Defined preliminary success criteria.
- Positioned RC-002 as the OBSERVE phase of Project Aegis.

### Key Engineering Decision

Technology selection was intentionally deferred until requirements engineering and telemetry requirements are sufficiently defined.

### Rationale

The project will select technologies based on research and engineering requirements rather than designing the research around predetermined monitoring, telemetry, or security tools.

This approach is intended to improve requirements traceability and reduce technology-driven bias in the experimental design.

### Relationship to RC-001

RC-001 established and experimentally validated the secure multi-site enterprise infrastructure that provides the conceptual foundation for RC-002.

RC-002 extends this work by investigating how behavior within such an environment can be systematically observed and measured.

### Current Status

**Research foundation established.**

### Next Step

Develop:

- Functional requirements.
- Non-functional requirements.
- Research constraints.
- Acceptance criteria.
- Initial requirements traceability.

---

## Requirements Engineering â€” Initial Baseline

### Activities

- Developed the initial RC-002 functional requirements.
- Defined non-functional requirements.
- Defined laboratory and measurement constraints.
- Established measurable acceptance criteria.
- Defined requirement evaluation statuses.
- Created preliminary objective-to-requirement traceability.

### Key Methodological Decision

Requirements were defined independently of specific telemetry
technologies.

### Rationale

Separating capability requirements from technology selection reduces
tool-driven design bias and allows candidate technologies to be
evaluated against predefined research needs.

### Next Step

Define the RC-002 telemetry model, measurement metrics, hypotheses,
and technology-selection criteria.

---

## Telemetry and Measurement Design

### Activities

- Defined the initial RC-002 telemetry model.
- Classified telemetry into infrastructure, communication,
  security-control, host/service, and experimental-context categories.
- Defined the telemetry lifecycle.
- Established six preliminary measurement dimensions.
- Defined three research hypotheses and corresponding null hypotheses.
- Identified independent, dependent, controlled, and potential
  confounding variables.

### Key Methodological Decision

Behavioral differentiation will be evaluated using predefined
measurements rather than subjective dashboard interpretation.

### Research Boundary

Observable differences between normal and controlled abnormal
conditions will not be treated as proof of automated threat detection.

### Next Step

Develop technology-selection criteria and evaluate candidate telemetry
technologies against RC-002 requirements.

---

## Technology and Laboratory Platform Evaluation

### Activities

- Evaluated Packet Tracer, GNS3, GNS3 with a lightweight telemetry node,
  and a full multi-tool virtual laboratory.
- Considered available local computing resources during platform
  evaluation.
- Selected a lightweight experimental architecture as the preferred
  initial direction.
- Defined a proposed minimum implementation stack.

### Preliminary Architecture

GNS3 will provide the network experimentation environment.

A lightweight Linux telemetry node will provide centralized collection
and preservation of selected experimental telemetry.

### Key Design Decision

RC-002 will prioritize minimum sufficient observability capability over
maximum technology quantity.

### Next Step

Formally record the laboratory platform decision using an Architectural
Decision Record before beginning environment installation.

---

## M1 â€” GNS3 Laboratory Environment Deployment

### Status

**VERIFIED**

### Activities

- Installed GNS3 Desktop 2.2.61.
- Imported the corresponding GNS3 VM into Oracle VirtualBox.
- Configured the VM with 2048 MB RAM and 1 vCPU.
- Configured host-only and NAT networking.
- Integrated the GNS3 VM with GNS3 Desktop.
- Verified local and VM-based GNS3 servers.

### Issue Encountered

The GNS3 VM initially failed to start because VirtualBox could not attach
the VM to the host-only network interface.

Error:

`VERR_INTNET_FLT_IF_NOT_FOUND`

### Resolution

The problem was isolated to the VirtualBox networking layer by testing
the VM independently of GNS3.

VirtualBox networking components were repaired/reinstalled and the host
system was restarted.

Following repair, the VM successfully booted and GNS3 successfully
managed the VM.

### Verification

Both the local GNS3 server and GNS3 VM server displayed healthy green
status indicators.

Normal non-administrator launch of GNS3 was also verified.

### Result

**PASS**

### Next Step

Develop the minimum experimental topology for RC-002 and select the
network and Linux node types required for telemetry experimentation.

## 2026-09-10 â€” M2 Minimum Observability Topology

### Objective

Establish and validate the minimum network infrastructure required to support
subsequent security-observability and telemetry experiments in RC-002.

### Implementation

- Built the RC-002 experimental topology in GNS3.
- Deployed FRRouting as `EDGE-R1`.
- Created three internal network zones:
  - USER â€” `192.168.10.0/24`
  - SERVER â€” `192.168.20.0/24`
  - OBSERVABILITY â€” `192.168.30.0/24`
- Added a GNS3 NAT-based WAN segment on `192.168.42.0/24`.
- Configured EDGE-R1 as the Layer-3 gateway between the internal zones.
- Deployed two VPCS user endpoints: `CLIENT-1` and `CLIENT-2`.
- Deployed lightweight Alpine Linux `SERVER-1`.
- Deployed Ubuntu `TELEMETRY-1`.
- Configured persistent addressing for the Linux endpoints.
- Enabled external connectivity through EDGE-R1 using IPv4 forwarding and
  iptables source NAT/MASQUERADE.
- Configured DNS resolution on SERVER-1.
- Installed and configured Lighttpd on SERVER-1 to provide reproducible HTTP
  application traffic.
- Configured Lighttpd for automatic startup.

### Verification

Verified:

- same-zone USER connectivity;
- bidirectional inter-zone routing;
- connectivity between USER, SERVER, and OBSERVABILITY zones;
- EDGE-R1 external connectivity;
- internal endpoint Internet connectivity through source NAT;
- DNS resolution;
- HTTP service operation on SERVER-1;
- cross-zone HTTP access from TELEMETRY-1 to SERVER-1;
- SERVER-1 addressing and HTTP-service persistence after reboot.

All required M2 functional tests passed.

### Engineering Observations

Initial routed ICMP tests involving Linux endpoints exhibited elevated latency
on several first replies before stabilizing to low-millisecond response times.
No packet loss was observed.

The cause was not established during M2 and is therefore recorded without
attributing it to a specific mechanism. It may be investigated later using
packet capture and telemetry evidence.

### Outcome

M2 established a reproducible network and application baseline for the
Project Aegis OBSERVE phase.

Centralized telemetry collection, traffic inspection, anomaly generation,
detection, and event correlation remain intentionally outside the M2 scope.

**Milestone Status: VERIFIED**

## M3 â€” Telemetry Collection Foundation

**Status:** Complete
**Verification:** `Docs/06-Verification/RC002-M3-Telemetry-Collection-Foundation.md`

### Engineering Summary

Established centralized security telemetry collection on TELEMETRY-1 using
rsyslog. Remote Syslog reception was enabled over UDP/TCP port 514 with
source-specific storage based on the observed sender IP address.

SERVER-1 and EDGE-R1 were configured as persistent remote Syslog sources.
Controlled events from both systems were successfully received, attributed,
and stored separately by TELEMETRY-1.

### Key Results

- Verified rsyslog collector operation on TELEMETRY-1.
- Verified centralized telemetry from SERVER-1 (`192.168.20.10`).
- Verified centralized telemetry from EDGE-R1 (`192.168.30.1`).
- Demonstrated source-specific storage and multi-source attribution.
- Verified telemetry forwarding after restart.
- Confirmed legitimate routed connectivity, Internet/NAT access, and the
  SERVER-1 HTTP service remained operational.

### Engineering Finding

EDGE-R1 restart testing exposed a persistence limitation inherited from the M2
network baseline. The WAN DHCP configuration and manually configured iptables
MASQUERADE rule did not initially recover automatically after router restart.

Persistent WAN DHCP configuration was subsequently implemented through
`/etc/network/interfaces`, while the verified MASQUERADE rule was stored using
the appliance's native OpenRC iptables save/restore mechanism.

A subsequent restart confirmed automatic recovery of WAN addressing, the
default route, NAT, Syslog forwarding, and external connectivity.

### Research Implication

M3 provides evidence toward H1 by demonstrating centralized, source-attributable
telemetry from multiple network sources. It also provides preliminary evidence
toward H3 because the legitimate network operations evaluated remained
functional alongside centralized telemetry.

H2 was not tested during M3; controlled abnormal-behavior experiments remain
outside this milestone's scope.

### Evidence

Evidence is stored under:

`Images/M3-Evidence/`

Six curated screenshots document collector operation, SERVER-1 and EDGE-R1
telemetry, multi-source attribution, post-restart persistence, and preservation
of normal network operation.


---

## M4 — Normal Traffic Baseline and Telemetry Characterization

**Status:** VERIFIED / PASS
**Project Aegis Phase:** OBSERVE

### Objective

Establish a measured reference condition for legitimate network and system behavior in the RC-002 environment before introducing controlled abnormal-behavior experiments.

Four normal-operation scenarios were evaluated:

- N01 — Inter-zone ICMP
- N02 — HTTP application access
- N03 — Internet ICMP
- N04 — Normal system activity

N01, N02, and N03 were each executed across five controlled trials. N04 evaluated centralized Syslog behavior from SERVER-1 and EDGE-R1.

### Key Results

- N01 completed all five trials with 0% packet loss, six captured ICMP packets per trial, and zero kernel drops.
- N01 mean trial-average RTT was 3.388 ms, with observed trial averages ranging from 2.664 ms to 5.450 ms.
- N02 returned HTTP 200 in all five trials.
- N02 captured 10 TCP packets per trial with SYN and SYN-ACK behavior observed and zero kernel drops.
- N02 mean completion time was 0.0097632 seconds, ranging from 0.007903 to 0.011616 seconds.
- N03 completed all five trials with three requests, three replies, and 0% packet loss.
- N03 mean trial-average RTT was 116.3288 ms, with trial averages ranging from 55.676 ms to 277.110 ms.
- N04 confirmed controlled and routine Syslog events from SERVER-1 and EDGE-R1 with source attribution preserved.
- All ten defined M4 acceptance criteria were satisfied within the milestone scope.

### Engineering Finding

The strongest baseline finding was the difference between internal and Internet-facing latency behavior.

N01 produced comparatively low and consistent trial-average RTT values, while N03 exhibited substantially greater RTT variability despite successful delivery and 0% packet loss in every trial.

This demonstrates that legitimate behavior in the laboratory is not necessarily low-variance behavior. A large latency value or latency increase cannot therefore be treated independently as evidence of abnormal activity.

Packet-level evidence and centralized Syslog also provided complementary forms of observability: packet captures described network interactions, while centralized logs preserved source-attributed system context.

### Research Implication

M4 establishes the normal reference condition required before controlled abnormal-behavior comparison.

H2 was not tested during M4. No abnormal condition was introduced, and no normal-versus-abnormal classification claim is made.

The M4 results indicate that later comparisons should use multiple observable characteristics rather than relying on a single fixed latency threshold.

M4 also provides additional evidence relevant to H1 through source-attributed telemetry and additional evidence consistent with H3 because the tested legitimate operations remained functional while observability mechanisms were active.

### Limitations

The baseline represents the implemented RC-002 laboratory environment and the specific scenarios tested.

N01, N02, and N03 used five trials each and should not be generalized to production enterprise traffic distributions.

N03 traversed an external path through the GNS3 NAT environment, so its RTT variability may include conditions outside the RC-002 topology.

N04 was primarily categorical rather than quantitative.

Deviation from the M4 baseline is not, by itself, evidence of malicious or abnormal behavior.

### Evidence

Raw experimental evidence is stored under:

`datasets/M4-Normal-Baseline/`

Derived baseline analysis is stored under:

`results/M4-Normal-Baseline/`

Formal milestone verification:

`Docs/06-Verification/RC002-M4-Normal-Traffic-Baseline.md`
