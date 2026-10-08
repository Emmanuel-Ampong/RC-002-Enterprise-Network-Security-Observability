# RC-002 M5 - A03 Controlled Denied-Access Experimental Protocol

**Project:** Project Aegis
**Research Cycle:** RC-002
**Milestone:** M5 - Controlled Abnormal Behavior
**Scenario:** A03 - Controlled Denied-Access Activity
**Protocol Status:** APPROVED - NOT YET EXECUTED
**Primary Hypothesis:** H2 - Behavioral Differentiation

---

## 1. Objective

Determine whether repeated, explicitly policy-denied TCP connection attempts produce observable telemetry characteristics distinguishable from the M4 legitimate-operation baseline.

The experiment evaluates network-policy enforcement, denied-connection frequency, packet-level visibility, source attribution, and operational preservation.

## 2. Experimental Environment

| Component | Configuration |
|---|---|
| Source | SERVER-1 - 192.168.20.10 |
| Destination | TELEMETRY-1 - 192.168.30.10 |
| Target service | TCP/22 (SSH) |
| Enforcement device | EDGE-R1 |
| Enforcement mechanism | Linux iptables FORWARD chain |
| Observation tools | iptables counters, Wireshark, available centralized telemetry |
| Repetitions | Five controlled trials |

Pre-experiment checks confirmed that SERVER-1 can reach TCP/22 on TELEMETRY-1.

EDGE-R1 initially had an ACCEPT policy on the FORWARD chain, with no explicit filter rules shown.

IPv4 forwarding was enabled, and the existing NAT MASQUERADE rule on eth3 was present.

## 3. Independent and Dependent Variables

**Independent variable:** Frequency of predefined TCP/22 connection attempts denied by the laboratory policy.

**Dependent variables:**

- Number of denied connection attempts.
- Firewall rule packet-counter changes.
- Observed TCP connection behavior.
- Source and destination attribution.
- Relevant centralized telemetry, where available.
- Operational-preservation outcomes.

## 4. Proposed Controlled Denial Policy

A temporary, narrowly scoped iptables rule will deny forwarded TCP traffic matching:

- Source: 192.168.20.10
- Destination: 192.168.30.10
- Protocol: TCP
- Destination port: 22
- Action: DROP

The rule must be documented and verified before the first trial.

No other forwarding or NAT rules shall be intentionally changed.

The rule will be removed after the experiment using an exact-match rollback command.

## 5. Frozen Trial Design

**Protocol status:** APPROVED - NOT YET EXECUTED

The following parameters are fixed before experimental execution.

| Parameter | Value |
|---|---|
| Trials | 5 (A03-T01 through A03-T05) |
| Attempts per trial | 5 |
| Source | SERVER-1 - 192.168.20.10 |
| Destination | TELEMETRY-1 - 192.168.30.10 |
| Protocol/service | TCP/22 |
| Firewall device | EDGE-R1 |
| Firewall action | DROP |
| Connection timeout | 3 seconds |
| Interval between attempts | 2 seconds |
| Observation point | SERVER-1 interface and EDGE-R1 firewall counters |
| Firewall state | Rule remains installed across all five trials |
| Counter handling | Reset only the experimental rule's counters before each trial |

### 5.1 Pre-experiment preparation

Record the original firewall rules and NAT configuration.

Verify that SERVER-1 can reach TCP/22 on TELEMETRY-1 before the experimental DROP rule is installed.

Record the experimental rule's exact match conditions and rollback command.

### 5.2 Experimental firewall rule

On EDGE-R1, install the following rule once, before A03-T01:

```bash
iptables -I FORWARD 1 -s 192.168.20.10 -d 192.168.30.10 -p tcp --dport 22 -j DROP
```

Verify the rule using:

```bash
iptables -L FORWARD -n -v --line-numbers
```

### 5.3 Trial execution

Before each trial, reset only the experimental rule's counters:

```bash
iptables -Z FORWARD 1
```

This command is permitted only after confirming that the experimental DROP rule remains at position 1.

Start the predefined packet capture.

On SERVER-1, execute five sequential TCP/22 connection attempts:

```bash
for i in 1 2 3 4 5; do
  echo "A03 attempt $i"
  nc -zv -w 3 192.168.30.10 22
  if [ "$i" -lt 5 ]; then sleep 2; fi
done
```

Record the source-side results, firewall counter changes, and packet observations.

Perform and record the required operational-preservation checks.

Retain the evidence before proceeding to the next trial.

### 5.4 Measurement interpretation

The expected result is five unsuccessful TCP/22 connection attempts per trial.

Firewall packet counters may exceed five because of TCP retransmissions. Therefore, counter values shall not be interpreted as connection-attempt counts without corroborating packet evidence.

A failed connection alone does not prove firewall enforcement.

### 5.5 Post-experiment rollback

After A03-T05, remove the exact experimental rule:

```bash
iptables -D FORWARD -s 192.168.20.10 -d 192.168.30.10 -p tcp --dport 22 -j DROP
```

Verify that the rule is absent and that SERVER-1 can again establish a TCP/22 connection to TELEMETRY-1.

Confirm that the original NAT configuration remains intact.

## 6. Expected Results

Under the defined DROP policy:

- TCP/22 connection attempts matching the rule should fail to establish a connection.
- The matching firewall rule should record packet activity.
- Packet captures should provide supporting evidence of connection attempts.
- Unrelated permitted connectivity should remain operational.

A failed connection alone will not be considered sufficient proof of policy enforcement.

Firewall counters and packet observations must corroborate the outcome.

Packet counts must not automatically be equated with connection-attempt counts because retransmissions may occur.

## 7. Evidence Requirements

For each trial, retain:

- Source-side connection-attempt output.
- Firewall rule and counter evidence.
- Relevant packet-capture observations.
- Source and destination attribution.
- Centralized telemetry evidence, if available.
- Operational-preservation checks.
- Unexpected observations and limitations.

## 8. Comparison with M4

A03 will be compared with the relevant M4 legitimate-operation measurements.

Where equivalent M4 metrics are unavailable, comparisons will be qualitative or reported as insufficient.

No numerical baseline difference will be invented.

## 9. Operational Preservation

The experiment must verify that the controlled TCP/22 denial does not unintentionally disrupt:

- Internal ICMP connectivity.
- Required HTTP/application services.
- External connectivity, where applicable.
- UDP/514 telemetry collection.
- Existing NAT and routing functions.

Any unexpected disruption must be recorded and investigated.

## 10. Rollback and Safety

Before applying the rule, record the complete iptables configuration.

Use a source-specific, destination-specific, port-specific rule.

Do not flush firewall chains or remove the existing NAT rule.

After the experiment, remove only the experimental rule and verify restoration of baseline TCP/22 reachability.

If unexpected disruption occurs, stop the experiment and restore the prior firewall state.

## 11. Interpretation Boundary

A03 evaluates the observability of controlled policy-denied activity.

It does not establish malicious intent, automated anomaly detection, or successful threat detection.

Unexpected outcomes shall be preserved in the research record.

## 12. Pre-Execution Requirements

Before A03-T01:

- Finalize the exact firewall commands and rollback procedure.
- Finalize the fixed trial timing and observation window.
- Confirm the relevant M4 reference condition.
- Confirm Wireshark observation points.
- Confirm required legitimate services.
- Establish the evidence and dataset structure.
- Commit the frozen protocol to GitHub.

**Status:** Experimental parameters approved and frozen before A03-T01. No experimental firewall rule has been applied.