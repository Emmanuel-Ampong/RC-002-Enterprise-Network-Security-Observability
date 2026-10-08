# RC-002 M5 - A02 Results

## Increased Destination or Service Diversity

**Project:** Project Aegis
**Research Cycle:** RC-002
**Milestone:** M5 - Controlled Abnormal Behavior
**Scenario:** A02 - Increased Destination or Service Diversity
**Status:** Five controlled trials executed; final analysis completed with documented limitations

---

## 1. Objective

A02 evaluated whether a predefined sequence of legitimate network interactions involving multiple destinations, protocols, and services could be observed using the RC-002 packet-capture and centralized telemetry infrastructure.

The experiment examined destination diversity, service diversity, protocol visibility, and operational preservation.

Unlike A01, which manipulated HTTP connection frequency, A02 focused on the diversity of network interactions.

---

## 2. Experimental Path

**Source:**

`SERVER-1 (192.168.20.10)`

**Internal destination:**

`TELEMETRY-1 (192.168.30.10)`

**External destination:**

`8.8.8.8`

**Protocols:**

- ICMP
- TCP
- UDP

**Services and destination ports:**

- TCP/22 - SSH
- TCP/514 - Syslog service reachability
- UDP/514 - Syslog telemetry transmission

Each trial followed the same predefined sequence:

1. Internal ICMP connectivity test.
2. TCP/22 reachability test.
3. TCP/514 reachability test.
4. UDP/514 test-marker transmission.
5. External ICMP connectivity test.

Five controlled trials, A02-T01 through A02-T05, were executed.

Packet observations were collected using Wireshark on the SERVER-1 network connection. Collector-side telemetry was examined on TELEMETRY-1.

---

## 3. Trial Results

| Trial | Intended destinations | TCP services | UDP/514 evidence | Operational result |
|---|---:|---|---|---|
| A02-T01 | 2 | 22, 514 | Collector receipt confirmed | PASS |
| A02-T02 | 2 | 22, 514 | Not observed in retained evidence | PARTIAL |
| A02-T03 | 2 | 22, 514 | Packet capture and collector receipt confirmed | PASS |
| A02-T04 | 2 | 22, 514 | Packet capture and collector receipt confirmed | PASS |
| A02-T05 | 2 | 22, 514 | Packet capture and collector receipt confirmed | PASS with exception |

Internal and external ICMP connectivity succeeded during the documented trials.

TCP/22 and TCP/514 were reported reachable.

The UDP/514 test marker was confirmed at the collector for T01, T03, T04, and T05.

For T02, the retained packet-filter evidence displayed no matching UDP/514 packet, and the collector search returned no corresponding marker.

This is an evidence gap rather than proof that the transmission never occurred.

---

## 4. Aggregate Results

Across the five trials:

- Controlled trials executed: 5.
- Intended destinations per trial: 2.
- Intended protocol types: 3.
- Tested TCP destination ports: 2.
- Trials with successful internal ICMP connectivity: 5.
- Trials with successful external ICMP connectivity: 5.
- Trials reporting TCP/22 reachability: 5.
- Trials reporting TCP/514 reachability: 5.
- Trials with confirmed UDP/514 collector evidence: 4.
- Trials with unresolved UDP/514 evidence: 1.
- Trials containing a documented ICMP Network Unreachable event: 1.

The confirmed UDP/514 collector-observation proportion was 4/5, or 80%.

This figure describes the availability of affirmative evidence across the five trials. It must not be interpreted as an independently measured UDP delivery success rate.

Exact trial packet counts, captured-byte totals, and trial durations were not consistently established from the retained evidence and are therefore not reported as aggregate measurements.

---

## 5. Comparison with M4 Normal Operation

A02 was conducted within the same RC-002 enterprise network and observability environment used to establish the M4 normal-operation reference.

The A02 experimental condition deliberately combined internal ICMP, TCP service reachability, UDP telemetry transmission, and external ICMP connectivity within each trial.

These interactions demonstrate the intended destination and service diversity of the controlled condition.

However, a numerical comparison against a specific M4 reference dataset has not yet been established in this report.

Accordingly, the results do not claim a measured increase in destination or service diversity relative to M4.

A formal baseline comparison requires verified M4 destination counts, protocol counts, service-port counts, and compatible observation windows.

---

## 6. Behavioral Interpretation

A02 demonstrates that the RC-002 observability infrastructure can expose several distinct network interaction types during a controlled experimental sequence.

The retained evidence includes:

- ICMP connectivity exchanges.
- TCP connection establishment and closure.
- UDP/514 telemetry traffic.
- Collector-side syslog records.
- An ICMP Network Unreachable error associated with a separate UDP/514 datagram.

The predefined experimental condition involved two intended destinations and multiple protocols and services.

The packet-level evidence therefore provides useful information about destination, protocol, and service visibility.

However, the results should not be interpreted as evidence of malicious behavior, successful anomaly detection, or a quantified increase in diversity over the normal-operation reference.

### T02 telemetry evidence limitation

During A02-T02, the retained Wireshark UDP/514 filter displayed zero matching packets.

The collector-side search also returned no matching test marker.

The available evidence does not establish successful UDP/514 transmission or receipt for this trial.

No retrospective correction or assumed successful delivery is applied.

### T05 ICMP Network Unreachable observation

During A02-T05, Wireshark recorded an ICMP Type 3, Code 0 Network Unreachable message from `192.168.20.1` to `192.168.20.10`.

The embedded original packet contained:

- Source: `192.168.20.10`
- Destination: `192.168.30.10`
- Protocol: UDP
- Source port: `53378`
- Destination port: `514`

The confirmed T05 test-marker datagram used UDP source port `56853`.

The different source ports establish that the ICMP error referenced a different UDP datagram from the identified T05 marker packet.

The T05 marker was separately observed in Wireshark and confirmed in the collector's syslog.

The root cause of the unreachable event remains undetermined.

This observation is retained as a network exception rather than discarded or treated as proof that the T05 marker failed.

---

## 7. Operational Preservation

The tested network remained operational for the principal connectivity checks across all five A02 trials.

The retained execution evidence supports:

- Successful internal ICMP connectivity.
- Successful external ICMP connectivity.
- TCP/22 reachability.
- TCP/514 reachability.

Centralized UDP/514 telemetry receipt was confirmed in four trials.

T02 lacked affirmative UDP/514 delivery evidence, while T05 contained a separate network-unreachable event.

The experiment therefore supports preservation of the tested connectivity functions but does not establish uninterrupted delivery of every datagram or absence of transient network errors.

---

## 8. H2 Relevance

A02 provides evidence relevant to RC-002 Hypothesis H2.

The experiment demonstrates that selected telemetry sources can expose different protocol and service interactions under a predefined controlled condition.

The confirmed packet captures and collector records support the observability of several components of the A02 sequence.

However, because the quantitative M4 comparison remains outstanding and T02 contains a telemetry evidence gap, A02 does not independently establish a reproducible numerical increase in destination or service diversity relative to baseline.

The scenario therefore provides supporting observability evidence while leaving the formal baseline-relative H2 assessment unresolved.

No overall conclusion regarding H2 is made at this stage.

---

## 9. Measurement Limitations

Several limitations affect interpretation of the A02 results.

**Packet counts and captured bytes:** Exact trial-specific packet counts and byte totals were not consistently established from the retained captures.

**Trial duration:** Comparable start-to-end observation windows were not consistently established.

**T02 telemetry:** The UDP/514 marker was not confirmed in the retained packet or collector evidence.

**T05 network exception:** An ICMP Network Unreachable event was observed for a separate UDP/514 datagram, but its root cause was not established.

**Baseline comparison:** The numerical M4 reference values required for a formal destination-diversity and service-diversity comparison have not yet been incorporated.

**Scope:** Successful connectivity checks do not establish complete network reliability under all operating conditions.

These limitations are retained explicitly to preserve the integrity and reproducibility of the research record.

---

## 10. Conclusion

A02 was executed across five controlled trials using the predefined destination and service interaction sequence.

The experiment produced evidence of internal and external ICMP connectivity, TCP service reachability, UDP telemetry transmission, and centralized collector observation.

Four trials contained affirmative UDP/514 collector evidence. T02 retained an unresolved telemetry evidence gap.

T05 also revealed an ICMP Network Unreachable event involving a separate UDP/514 datagram.

The results demonstrate visibility into several network interaction types while preserving the principal tested connectivity functions.

A02 trial execution and evidence collection are complete, subject to the documented limitations.

A quantified comparison with the M4 normal-operation reference remains outstanding before making a definitive claim about baseline-relative increases in destination or service diversity.

The next stage is to consolidate the M5 experimental findings and continue with the remaining predefined research scenarios.