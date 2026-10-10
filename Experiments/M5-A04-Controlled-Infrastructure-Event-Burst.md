# RC-002 M5-A04 — Controlled Infrastructure-Event Burst

**Project:** Project Aegis — RC-002 Enterprise Network Security Observability and Telemetry
**Project Aegis Phase:** OBSERVE
**Milestone:** M5 — Controlled Abnormal-Behavior Experiments
**Scenario:** A04 — Controlled Infrastructure-Event Burst
**Protocol Status:** FROZEN — PRE-EXECUTION
**Primary Hypothesis:** H2 — Behavioral Differentiation
**Secondary Hypothesis:** H3 — Operational Preservation

---

## 1. Purpose

A04 investigates whether a controlled increase in system-generated Syslog message frequency produces observable changes in centralized telemetry compared with the M4 normal-operation baseline.

The experiment evaluates event frequency, centralized collection, source attribution, event-count consistency, and operational preservation.

This experiment does not evaluate automated threat detection or simulate actual infrastructure failures.

## 2. Experimental Environment

| Component | Configuration |
|---|---|
| Event source | SERVER-1 |
| Source IP | 192.168.20.10 |
| Source operating system | BusyBox Linux |
| Event-generation utility | BusyBox `logger` |
| Syslog forwarding service | BusyBox `syslogd` |
| Central collector | TELEMETRY-1 |
| Collector IP | 192.168.30.10 |
| Syslog destination port | 514 |
| Collector software | rsyslog |
| Remote log path | `/var/log/remote/192.168.20.10/syslog.log` |
| Existing network topology | Unchanged |

SERVER-1 uses an existing Syslog forwarding configuration targeting TELEMETRY-1. The collector stores received messages in source-specific log files.

The single-event prechecks `RC002-A04-PRECHECK-001` and `RC002-A04-FORMAT-CHECK-E01` were confirmed in centralized Syslog before protocol execution.

These prechecks are excluded from the experimental dataset.

## 3. Experimental Variables

### Independent Variable

The frequency of predefined benign system