# RC-002 — WP-02 Baseline ICMP Evidence

**Project:** Project Aegis
**Milestone:** M6 — Research Execution
**Work Package:** WP-02 — Operational Regression Verification
**Test IDs:** REG-01 through REG-04
**Execution Date:** 13 October 2026
**Environment:** GNS3
**Evidence Type:** Transcript-derived test records

## 1. Evidence Provenance

These records were reconstructed from console outputs shared during
the WP-02 baseline execution.

They preserve the observed measurements but are not independently
exported GNS3 console captures.

No configuration changes were introduced during these tests.

## 2. ICMP Test Results

| Test ID | Source | Destination | Replies | Loss | Min RTT | Avg RTT | Max RTT | Result |
|---|---|---|---|---|---|---|---|---|
| REG-01 | CLIENT-1 | 192.168.10.1 | 5/5 | 0% | 1.000 ms | 1.380 ms | 2.084 ms | PASS |
| REG-02 | CLIENT-1 | 192.168.10.11 | 5/5 | 0% | 0.268 ms | 0.521 ms | 0.944 ms | PASS |
| REG-03 | CLIENT-1 | 192.168.20.10 | 5/5 | 0% | 2.186 ms | 2.692 ms | 3.503 ms | PASS |
| REG-04 | CLIENT-1 | 192.168.30.10 | 5/5 | 0% | 1.865 ms | 2.775 ms | 3.454 ms | PASS |

## 3. Observed Results

### REG-01 — USER Gateway Connectivity

CLIENT-1 successfully reached EDGE-R1 at 192.168.10.1.

Observed reply times:

1.182, 1.000, 1.157, 2.084, 1.477 ms.

**Result:** PASS

### REG-02 — Same-Zone Connectivity

CLIENT-1 successfully reached CLIENT-2 at 192.168.10.11.

Observed reply times:

0.268, 0.477, 0.410, 0.944, 0.505 ms.

**Result:** PASS

### REG-03 — USER-to-SERVER Connectivity

CLIENT-1 successfully reached SERVER-1 at 192.168.20.10.

Observed reply times:

2.587, 3.503, 2.425, 2.758, 2.186 ms.

All five replies reported TTL 63.

**Result:** PASS

### REG-04 — USER-to-OBSERVABILITY Connectivity

CLIENT-1 successfully reached TELEMETRY-1 at 192.168.30.10.

Observed reply times:

3.454, 2.938, 1.865, 2.892, 2.728 ms.

All five replies reported TTL 63.

**Result:** PASS

## 4. Evidence Assessment

All four tests returned five successful ICMP replies with no
observed packet loss.

The results support point-in-time connectivity across the tested
network paths.

Five probes per destination do not establish continuous availability,
absence of intermittent failures, or sustained network performance.

## 5. Evidence Limitations

- Results are reconstructed from previously shared console transcripts.
- Original GNS3 console capture files were not separately preserved.
- No packet captures were collected specifically for these four tests.
- No post-change comparison has yet been performed.