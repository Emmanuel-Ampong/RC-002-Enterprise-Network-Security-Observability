
# RC-002 — WP-02 Baseline Remote Syslog Evidence

**Project:** Project Aegis  
**Milestone:** M6 — Research Execution  
**Work Package:** WP-02 — Operational Regression Verification  
**Test ID:** REG-10  
**Execution Date:** 13 October 2026  
**Environment:** GNS3  
**Collector:** TELEMETRY-1 (192.168.30.10)  
**Evidence Type:** Transcript-derived console observations  

## 1. Evidence Provenance

This document records the REG-10 remote syslog verification performed during the WP-02 operational regression baseline.

Test messages were generated on EDGE-R1 and SERVER-1. Their arrival was subsequently verified in source-specific log files on TELEMETRY-1.

The evidence was reconstructed from console outputs shared during execution. It is not an independently exported GNS3 console capture.

No syslog configuration changes were made as part of this baseline verification.

## 2. REG-10 — Remote Syslog Collection Verification

**Result:** PASS

**Objective:** Verify that TELEMETRY-1 receives identifiable syslog messages from EDGE-R1 and SERVER-1.

### 2.1 Collector Log Organization

The collector used a remote logging template with the following destination pattern:

```text
/var/log/remote/%FROMHOST-IP%/syslog.log
```

This arrangement places received events into directories associated with their reported source IP addresses.

### 2.2 EDGE-R1 Syslog Test

**Source device:** EDGE-R1  
**Collector destination:** TELEMETRY-1  
**Source-associated log file:** `/var/log/remote/192.168.30.1/syslog.log`

The following command was executed on EDGE-R1:

```sh
logger -t RC002-REG10 "WP02 operational regression syslog verification from EDGE-R1"
```

The collector contained the following record at line 1030:

```text
2026-10-13T09:46:00+00:00 _gateway RC002-REG10: WP02 operational regression syslog verification from EDGE-R1
```

**Assessment:** The expected test tag and message were present in the collector's source-associated log file.

The hostname field appeared as `_gateway`. The source association with `192.168.30.1` was established through the collector's remote log directory.

### 2.3 SERVER-1 Syslog Test

**Source device:** SERVER-1 (192.168.20.10)  
**Collector destination:** TELEMETRY-1  
**Source-associated log file:** `/var/log/remote/192.168.20.10/syslog.log`

The following command was executed on SERVER-1:

```sh
logger -t RC002-REG10-SERVER "WP02 operational regression syslog verification from SERVER-1"
```

The collector contained the following record at line 592:

```text
2026-10-13T09:53:21+00:00 192.168.20.10 RC002-REG10-SERVER: WP02 operational regression syslog verification from SERVER-1
```

**Assessment:** The expected test tag, source address and message were present in the collector's source-associated log file.

### 2.4 Message Integrity Checks

During the console inspection, portions of the displayed messages appeared truncated.

The complete stored records were subsequently checked using commands equivalent to:

```sh
sudo sed -n '1030p' /var/log/remote/192.168.30.1/syslog.log | od -An -tx1c
```

```sh
sudo sed -n '592p' /var/log/remote/192.168.20.10/syslog.log | od -An -tx1c
```

The byte-level inspections confirmed that the complete expected message text was stored in each corresponding log record.

## 3. Evidence Assessment

| Source | Test Tag | Collector File | Result |
|---|---|---|---|
| EDGE-R1 | RC002-REG10 | `/var/log/remote/192.168.30.1/syslog.log` | PASS |
| SERVER-1 | RC002-REG10-SERVER | `/var/log/remote/192.168.20.10/syslog.log` | PASS |

The observations demonstrate successful receipt and storage of both deliberately generated test messages during the baseline execution.

The results support the operational status of the remote syslog collection path at that point in time.

## 4. Evidence Limitations

- This record was reconstructed from previously shared console outputs.
- Original GNS3 console capture files were not separately archived.
- Only two deliberately generated messages were verified.
- The test did not measure sustained log delivery, loss rates, latency or throughput.
- The observed log-directory associations do not independently validate every source-identification mechanism.
- No post-change comparison has yet been completed.

Accordingly, REG-10 is classified as **PASS** for the specified point-in-time verification, without claiming continuous or lossless syslog collection.

## 5. Follow-Up

The baseline observations should be retained for comparison with post-change operational regression results.

Future tests should verify whether the same source devices continue to deliver identifiable messages after observability or security configuration changes.
