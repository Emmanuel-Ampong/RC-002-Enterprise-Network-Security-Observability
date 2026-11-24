
# RC002-M6-WP01-F08 — A03 Controlled Access Comparator Protocol

## 1. Research Identification

| Field | Details |
|---|---|
| Project | Project Aegis |
| Research Cycle | RC-002 — Enterprise Network Security Observability |
| Milestone | M6 — Research Execution |
| Work Package | WP01 — Evidence Audit and Integrity |
| Finding | WP01-F08 |
| Priority | HIGH |
| Original Experiment | M5-A03 — Controlled Denied-Access Activity |
| Protocol Date | 2026-11-24 |
| Protocol Status | DRAFT — NOT YET EXECUTED |
| Research Hypothesis | H2 — Behavioral Differentiation |

## 2. Background

The original A03 experiment successfully demonstrated observable TCP/22 denial under a temporary, narrowly scoped firewall DROP rule.

Five trials produced 25 timed-out connection attempts, 50 firewall DROP packets, and 25 observed TCP SYN retransmissions.

Historical evidence also confirmed successful TCP/22 reachability before rule installation and after firewall rollback.

However, the original M4 baseline did not contain directly comparable quantitative TCP/22 connection measurements.

WP01-F08 was opened to resolve this comparison gap and strengthen recovery verification.

The present experiment will produce new measurements. It will not replace or retrospectively modify the original A03 findings.

## 3. Objective

Establish a controlled, repeatable quantitative comparison of TCP/22 connection behavior across three conditions:

1. NORMAL — Permitted TCP/22 connectivity before applying the experimental restriction.
2. DENIED — TCP/22 connection attempts while the experimental DROP rule is active.
3. RECOVERY — Restored TCP/22 connectivity after removing the experimental DROP rule.

The experiment will compare connection outcomes, TCP packet behavior, firewall counters where applicable, and operational-preservation observations.

## 4. Laboratory Environment

| Component | Configuration |
|---|---|
| Source | SERVER-1 |
| Source IPv4 | 192.168.20.10 |
| Destination | TELEMETRY-1 |
| Destination IPv4 | 192.168.30.10 |
| Target service | TCP/22 |
| Firewall enforcement point | EDGE-R1 |
| Firewall mechanism | Linux iptables FORWARD chain |
| Packet observation | EDGE-R1 eth1 to SERVER-SW1 Ethernet0 |
| Collector | TELEMETRY-1 |
| UDP telemetry service | Port 514 |

### Verified Pre-Protocol Conditions

On 2026-11-24:

- EDGE-R1 FORWARD chain policy was ACCEPT.
- No explicit FORWARD rules were present.
- SERVER-1 successfully connected to TELEMETRY-1 on TCP/22.

These prechecks establish current laboratory readiness but are not counted as quantitative F08 trials.

The laboratory state must be rechecked immediately before trial execution.

## 5. Experimental Variables

### Independent Variable

Firewall access condition for SERVER-1 to TELEMETRY-1 on TCP/22:

- NORMAL — No experimental DROP rule.
- DENIED — Experimental DROP rule active.
- RECOVERY — Experimental DROP rule removed.

### Dependent Variables

- Total TCP/22 connection attempts.
- Successful connection attempts.
- Timed-out attempts.
- Other connection failures.
- Observed initial TCP SYN packets.
- Observed TCP SYN retransmissions.
- Matching firewall DROP packets, where applicable.
- Matching firewall DROP bytes, where applicable.
- Packet-capture duration and frame bytes, where reliably measurable.
- Operational-preservation results.
- TCP/22 restoration following rule removal.

## 6. Frozen Experimental Design

Each condition will contain five trials, with five TCP/22 connection attempts per trial.

| Condition | Trial IDs | Trials | Attempts per Trial | Planned Attempts |
|---|---|---:|---:|---:|
| NORMAL | F08-N01 to F08-N05 | 5 | 5 | 25 |
| DENIED | F08-D01 to F08-D05 | 5 | 5 | 25 |
| RECOVERY | F08-P01 to F08-P05 | 5 | 5 | 25 |
| TOTAL | — | 15 | 5 | 75 |

### Fixed Parameters

| Parameter | Value |
|---|---|
| Source | 192.168.20.10 |
| Destination | 192.168.30.10 |
| Target port | TCP/22 |
| Connection timeout | 3 seconds |
| Delay between attempts | 2 seconds |
| Connection attempts per trial | 5 |
| Trials per condition | 5 |
| Packet-capture point | EDGE-R1 eth1 to SERVER-SW1 Ethernet0 |
| Experimental firewall action | DROP |
| Experimental rule persistence | DENIED condition only |

The same connection-attempt procedure must be used for all 15 trials.

Any changes to these parameters must be documented before continuing.

## 7. Pre-Experiment Safety Checks

Before generating trial traffic:

1. Verify SERVER-1 and TELEMETRY-1 addresses.
2. Verify TCP/22 connectivity under normal conditions.
3. Record the EDGE-R1 FORWARD chain.
4. Record the EDGE-R1 NAT configuration.
5. Confirm that the previous A03 DROP rule is absent.
6. Confirm the packet-capture interface.
7. Confirm UDP/514 collector availability.
8. Confirm required HTTP and ICMP connectivity.
9. Record the intended firewall rollback command.
10. Establish trial-specific evidence storage.

Do not begin experimental traffic until the protocol is reviewed, committed, and approved for execution.

## 8. NORMAL Condition — F08-N01 to F08-N05

The NORMAL condition measures permitted TCP/22 behavior without an experimental DROP rule.

For each trial:

1. Confirm that the experimental DROP rule is absent.
2. Start a new trial-specific packet capture.
3. Execute five sequential TCP/22 connection attempts.
4. Record each attempt's connection result and exit status.
5. Inspect relevant TCP SYN, SYN-ACK, and retransmission observations.
6. Record supported packet and duration measurements.
7. Execute the defined operational-preservation checks.
8. Stop and preserve the capture.
9. Record evidence filenames and integrity fingerprints.

All five NORMAL trials must use the same connection-attempt sequence.

No experimental firewall rule will be installed during this condition.

Experimental DROP counters are NOT APPLICABLE when the corresponding rule is absent; they must not be recorded as measured zero drops.

## 9. DENIED Condition — F08-D01 to F08-D05

### 9.1 Experimental Rule

The approved experimental rule is:

~~~bash
iptables -I FORWARD 1 -s 192.168.20.10 -d 192.168.30.10 -p tcp --dport 22 -j DROP
~~~

Install this rule once, immediately before the DENIED condition.

The rule must be confirmed before generating denied-access traffic.

### 9.2 Trial Execution

For each DENIED trial:

1. Verify that the exact experimental DROP rule remains at FORWARD position 1.
2. Reset only the experimental rule's counters.
3. Start a new trial-specific packet capture.
4. Execute five sequential TCP/22 connection attempts.
5. Record connection outcomes and exit statuses.
6. Record the experimental rule's packet and byte counters.
7. Inspect initial SYN packets and retransmissions.
8. Execute operational-preservation checks.
9. Stop and preserve the packet capture.
10. Record evidence filenames and integrity fingerprints.

The experimental rule must remain installed through F08-D05.

Do not flush firewall chains, change unrelated rules, or alter the existing NAT configuration.

A timeout alone does not prove firewall enforcement. Packet observations and matching firewall counters must corroborate the denied-access condition.

DROP packet counts must not be equated with distinct connection-attempt counts because retransmissions may occur.

## 10. RECOVERY Condition — F08-P01 to F08-P05

After the DENIED condition, remove the exact experimental rule using:

~~~bash
iptables -D FORWARD -s 192.168.20.10 -d 192.168.30.10 -p tcp --dport 22 -j DROP
~~~

Before beginning recovery trials:

1. Confirm that the experimental rule is absent.
2. Verify the FORWARD chain policy and remaining rules.
3. Check that the baseline NAT configuration remains intact.
4. Verify that TCP/22 connectivity has been restored.

Execute five RECOVERY trials using the same connection-attempt procedure as the NORMAL and DENIED conditions.

For each trial, preserve source-side output, packet evidence, operational checks, and supported quantitative measurements.

Experimental DROP counters are NOT APPLICABLE after the rule has been removed.

## 11. Standard Connection-Attempt Procedure

The planned SERVER-1 connection-attempt procedure is:

~~~bash
for i in 1 2 3 4 5; do
  echo "F08 attempt $i"
  nc -zv -w 3 192.168.30.10 22
  echo "NC_EXIT=$?"
  if [ "$i" -lt 5 ]; then sleep 2; fi
done
~~~

Each trial must retain its unique trial identifier in the associated evidence record.

The command and timing parameters must remain unchanged across the three experimental conditions.

The source-side exit status must be interpreted together with the connection output; a nonzero exit status alone does not identify the reason for failure.

## 12. Packet-Capture Requirements

A separate packet capture must be retained for each trial.

The capture should support identification of:

- Source and destination addresses.
- TCP destination port.
- Initial SYN packets.
- SYN retransmissions.
- SYN-ACK or other TCP response packets, where present.
- TCP connection behavior.
- Packet counts and captured frame bytes.
- Capture duration, where independently measurable.

Do not assume that firewall layer-3 byte counters equal link-layer captured frame bytes.

Capture completeness and dropped-packet statistics must be qualified unless independently supported by retained measurement evidence.

Original A03 packet captures and screenshots must remain unchanged.

## 13. Operational-Preservation Checks

At predefined checkpoints, verify:

- SERVER-1 to TELEMETRY-1 ICMP reachability.
- TCP/514 reachability.
- UDP/514 telemetry marker receipt at TELEMETRY-1.
- Required HTTP application availability.
- External ICMP connectivity where applicable.
- Routing and NAT configuration preservation.

Record failures, exceptions, and repeated checks separately.

A successful checkpoint does not establish uninterrupted availability between measurements.

If an unexpected disruption occurs, stop the current experiment, preserve the available evidence, and restore the original firewall state before continuing.

## 14. Required Trial Dataset

Create a new F08 dataset separate from the original A03 dataset.

Suggested columns:

~~~text
Trial_ID
Condition
Connection_Attempts
Successful_Attempts
Timed_Out_Attempts
Other_Failed_Attempts
Observed_Initial_SYNs
Observed_SYN_Retransmissions
Observed_SYN_ACKs
Firewall_Drop_Packets
Firewall_Drop_Bytes
Captured_Packets
Captured_Frame_Bytes
Capture_Duration_Seconds
Internal_ICMP
TCP514_Reachability
UDP514_Collector_Receipt
HTTP_Status
External_ICMP
Firewall_Rule_State
Operational_Preservation
Evidence_Reference
Notes
~~~

Use NA for measurements that are inapplicable or unsupported.

Do not enter zero where a measurement was not collected or could not be verified.

## 15. Evidence Preservation

All new evidence must use F08-specific identifiers.

Recommended structure:

~~~text
Docs/14-M6-Research-Execution/RC002-WP01-F08-A03-Comparator/
    F08-Results.md
    F08-Evidence-Manifest.md
    F08-Comparator-Analysis.md

datasets/M6-WP01-F08/
    F08-Trial-Summary.csv

Images/M6-WP01-F08/
    NORMAL/
    DENIED/
    RECOVERY/
~~~

Raw packet captures may be retained outside Git where required by repository policy.

For each retained raw capture, record:

- Trial ID.
- Original filename.
- Storage location.
- File size.
- SHA-256 fingerprint.
- Capture interface.
- Available capture-quality limitations.

Do not overwrite or rename original M5 A03 evidence.

## 16. Comparison Strategy

Compare NORMAL, DENIED, and RECOVERY using:

1. Connection success proportion.
2. Connection timeout proportion.
3. Initial SYN count per trial.
4. SYN retransmissions per trial.
5. TCP response observations.
6. Firewall DROP counters during the DENIED condition.
7. Supported packet-volume and duration measurements.
8. Operational-preservation results.

Report results individually by trial and as descriptive summaries across conditions.

Distinguish connection attempts from packets, retransmissions, and firewall counter increments.

The new F08 measurements must not be represented as historical M4 or original A03 results.

## 17. Acceptance Criteria

WP01-F08 may be recommended for closure when:

1. A reproducible NORMAL TCP/22 comparator is established.
2. The DENIED condition is supported by source-side, packet-level, and firewall evidence.
3. Post-rollback TCP/22 recovery is independently verified.
4. Comparable connection-outcome measurements are recorded.
5. Packet and firewall counter interpretations are consistent and qualified.
6. Operational-preservation results and exceptions are documented.
7. Raw evidence references and integrity fingerprints are recorded.
8. Remaining measurement limitations are disclosed.
9. The evidence audit and requirements traceability matrix are reconciled.

Failure to meet an expected experimental result must be retained and analyzed rather than altered or omitted.

## 18. Research Boundaries

This experiment evaluates controlled network-policy enforcement and selected behavioral differences within the RC-002 laboratory.

It does not establish:

- Malicious intent.
- Successful attack detection.
- Automated anomaly detection.
- General network security effectiveness.
- Continuous service availability.
- Universal firewall behavior.
- Causal attribution for historical A03 network exceptions.

Observed differences must be interpreted only within the tested laboratory conditions.

## 19. Rollback and Safety Requirements

The experiment must not intentionally disrupt unrelated network services.

The experimental DROP rule must be narrowly scoped to the designated source, destination, protocol, and port.

If unexpected disruption occurs, remove only the experimental rule and verify the original firewall configuration.

Do not flush firewall chains or delete pre-existing NAT rules.

The experiment is complete only after successful rule removal, restoration checks, and evidence preservation.

## 20. Execution State

| Activity | Status |
|---|---|
| Historical A03 results reviewed | COMPLETE |
| Original A03 protocol reviewed | COMPLETE |
| Original TCP/22 baseline evidence reviewed | COMPLETE |
| Historical firewall rollback evidence reviewed | COMPLETE |
| Historical TCP/22 restoration evidence reviewed | COMPLETE |
| Current FORWARD chain prechecked | COMPLETE |
| Current normal TCP/22 connectivity prechecked | COMPLETE |
| F08 comparator protocol | DRAFT |
| Protocol committed to Git | NOT YET |
| NORMAL trials | NOT STARTED |
| DENIED trials | NOT STARTED |
| RECOVERY trials | NOT STARTED |
| Quantitative comparison | NOT STARTED |
| WP01-F08 disposition | OPEN |

No F08 experimental trial may begin until this protocol has been reviewed, frozen, and committed.

---

Project Aegis — RC-002 Enterprise Network Security Observability

M6 / WP01 / F08 — Controlled Access Comparator Protocol
