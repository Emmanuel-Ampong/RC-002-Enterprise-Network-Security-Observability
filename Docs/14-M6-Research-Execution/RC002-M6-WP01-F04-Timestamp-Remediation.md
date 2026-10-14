# RC-002 M6 WP01-F04 — Timestamp Discrepancy Investigation and Remediation

**Project:** Project Aegis — RC-002 Enterprise Network Security Observability
**Milestone:** M6 — Research Execution
**Work Package:** WP-01 — Evidence Audit and Corrective Actions
**Finding ID:** WP01-F04
**Priority:** HIGH
**Investigation Date:** 2026-10-14
**Status:** CLOSED WITH LIMITATIONS — recorded in the M6 Evidence Audit
## 1. Purpose and Scope

This record documents the investigation and remediation of the source-versus-collector timestamp discrepancy identified in WP01-F04.

The investigation examined clock synchronization on EDGE-R1 and TELEMETRY-1, corrected identified synchronization deficiencies, and performed controlled syslog tests to evaluate timestamp agreement.

The objective is to establish a defensible basis for event chronology and subsequent security telemetry correlation.

This investigation does not independently establish precise network latency, millisecond-level end-to-end event-delivery delay, or historical timestamp accuracy before synchronization.

## 2. Initial Investigation Findings

### 2.1 EDGE-R1 Clock Discrepancy

Initial inspection identified a substantial discrepancy between EDGE-R1's system clock and the external NTP reference.

The following observations were recorded:

- EDGE-R1 used a lightweight FRRouting environment with BusyBox NTP.
- The `timedatectl` utility was unavailable.
- BusyBox `ntpd` was installed but was not running.
- The NTP service was not registered for automatic startup.
- A read-only NTP diagnostic successfully contacted `185.125.190.56`.
- The diagnostic reported a clock offset of approximately +21,372 seconds, indicating that EDGE-R1 was approximately 5 hours and 56 minutes behind the NTP reference.

These observations established a significant source-clock synchronization deficiency that could undermine reliable event chronology and cross-device log correlation.

The discrepancy was measured before corrective action. Historical log timestamps were therefore retained as pre-synchronization observations rather than treated as independently validated event times.


### 2.2 TELEMETRY-1 Clock Synchronization Investigation

The investigation also identified a clock synchronization problem on TELEMETRY-1, the centralized syslog collector.

Initial diagnostic observations included:

- `timedatectl status` reported `System clock synchronized: no`.
- The `systemd-timesyncd` service was active but had not selected an NTP server.
- `timedatectl timesync-status` reported `Server: n/a` and `Packet count: 0`.
- DNS resolution tests for `ntp.ubuntu.com` and `example.com` returned no results.
- `resolvectl status` showed no configured upstream DNS server.
- The active Netplan configuration assigned `192.168.30.10/24` with default gateway `192.168.30.1`, but did not specify DNS servers.
- An ICMP connectivity test to `8.8.8.8` succeeded with five replies and zero observed packet loss.

These findings established that TELEMETRY-1 had working external IPv4 connectivity but lacked functional hostname resolution and verified clock synchronization.

The missing upstream DNS configuration was a likely contributor to the NTP synchronization failure. Subsequent corrective action restored DNS resolution and established successful NTP synchronization.



## 3. Corrective Actions and Remediation

### 3.1 TELEMETRY-1 DNS and NTP Remediation

The following corrective actions were performed on TELEMETRY-1:

1. Preserved the original Netplan configuration as `50-cloud-init.yaml.pre-dns-fix`.
2. Added upstream DNS servers `8.8.8.8` and `1.1.1.1` to the existing Netplan configuration.
3. Preserved the static IPv4 address `192.168.30.10/24` and default gateway `192.168.30.1`.
4. Successfully validated the updated configuration using `sudo netplan generate`.
5. Applied the configuration using `sudo netplan try --timeout 120` and confirmed the changes.
6. Verified successful DNS resolution of `ntp.ubuntu.com`.
7. Confirmed that `systemd-timesyncd` subsequently achieved clock synchronization.

Post-remediation verification produced the following observations:

| Verification Parameter | Observed Result |
|---|---|
| System clock synchronized | yes |
| NTP service | active |
| NTP server | 185.125.190.56 (ntp.ubuntu.com) |
| NTP stratum | 2 |
| Reported clock offset | +43.546 ms |
| NTP packet count | 5 |
| Leap status | normal |

**Assessment:** DNS resolution was restored, and TELEMETRY-1 achieved verified NTP synchronization. The reported offset reflects the synchronization status at the time of measurement and is not a guarantee of constant clock accuracy.


### 3.2 EDGE-R1 Clock Correction and NTP Service Configuration

The following corrective actions were performed on EDGE-R1:

1. Confirmed that BusyBox `ntpd` was installed but not running.
2. Verified DNS resolution of `ntp.ubuntu.com`.
3. Used a read-only NTP diagnostic to confirm communication with `185.125.190.56` and identify the clock discrepancy.
4. Recorded the pre-correction system timestamp as `2026-10-13 23:37:37 UTC`.
5. Executed a controlled NTP synchronization using `timeout 180 ntpd -dnq -p 185.125.190.56`.
6. Confirmed that BusyBox NTP corrected the clock by `+21371.783021` seconds.
7. Verified that EDGE-R1 retained the corrected UTC time.
8. Repeated the read-only NTP diagnostic and observed offsets between approximately -3.8 ms and -4.8 ms.
9. Started the existing OpenRC NTP service using `rc-service ntpd start`.
10. Verified that OpenRC reported the service as started.
11. Enabled automatic NTP startup using `rc-update add ntpd default`.
12. Verified that `ntpd` appeared in the default OpenRC runlevel.

| Verification Parameter | Observed Result |
|---|---|
| NTP implementation | BusyBox 1.35.0 |
| Initial clock offset | Approximately +21,372 seconds |
| Clock correction applied | +21,371.783021 seconds |
| Corrected timestamp reported by NTP | 2026-10-14 05:34:45.899636 |
| Post-correction NTP offset | Approximately -3.8 to -4.8 ms |
| NTP service status | started |
| Automatic startup registration | Confirmed in default runlevel |

**Assessment:** EDGE-R1's substantial clock discrepancy was corrected. Continuous NTP service operation was enabled, and automatic startup was configured.

**Operational limitation:** The approximately six-hour clock adjustment created a discontinuity between historical and post-remediation timestamps. Automatic startup was verified through OpenRC configuration inspection, not through an actual reboot test. Long-term synchronization stability remains to be assessed.


## 4. Post-Remediation Timestamp Validation

### 4.1 Controlled Syslog Event Tests

Two controlled syslog events were generated on EDGE-R1 after the clock synchronization corrective actions.

Each message contained an embedded UTC timestamp generated by EDGE-R1. The corresponding records were retrieved from TELEMETRY-1's centralized log storage.

**Collector log path:**
`/var/log/remote/192.168.30.1/syslog.log`

| Test | EDGE-R1 Embedded UTC Timestamp | Stored Syslog Timestamp | Result |
|---|---|---|---|
| Timestamp validation 1 | 2026-10-14 05:47:02 | 2026-10-14 05:47:02 | PASS |
| Timestamp validation 2 | 2026-10-14 05:48:44 | 2026-10-14 05:48:44 | PASS |

Both events were successfully received and archived by TELEMETRY-1.

The embedded source timestamps matched the timestamps displayed in the stored syslog records at one-second precision.

**Evidence limitation:** These comparisons do not independently establish collector reception time because the stored syslog timestamps may originate from the source messages. They also do not prove zero clock skew or zero event-delivery latency.


### 4.2 Independent Collector-Side Packet Capture Validation

An additional controlled syslog event was generated on EDGE-R1 while TELEMETRY-1 captured incoming traffic using `tcpdump`.

The capture was performed using:

```bash
sudo timeout 120 tcpdump -i any -nn -tttt -s 0 -A -c 1 'src host 192.168.30.1 and dst port 514'
```

The captured packet contained the following observations:

| Measurement | Observed Result |
|---|---|
| Source IP address | 192.168.30.1 |
| Collector IP address | 192.168.30.10 |
| Destination port | 514 |
| Collector packet capture timestamp | 2026-10-14 20:54:06.910910 UTC |
| Embedded source UTC timestamp | 2026-10-14 20:54:07 UTC |
| Packets captured | 1 |
| Packets received by capture filter | 1 |
| Packets dropped by kernel | 0 |

The captured message contained:

```text
RC002-WP01-F04: PACKET-TIMESTAMP-TEST SOURCE_UTC=2026-10-14T20:54:07Z
```

The same event was subsequently verified in the centralized syslog archive:

```text
2026-10-14T20:54:07+00:00 _gateway RC002-WP01-F04: PACKET-TIMESTAMP-TEST SOURCE_UTC=2026-10-14T20:54:07Z
```

**Assessment:** The independent packet capture demonstrated close agreement between the source timestamp and the collector's packet capture time at approximately one-second resolution. The event was also successfully archived.

**Measurement limitation:** The embedded source timestamp has one-second precision, while the packet capture timestamp includes fractional seconds. The apparent difference of approximately 89 milliseconds is affected by timestamp precision and must not be interpreted as measured network latency or exact clock skew.

The zero kernel-drop count applies only to this short, single-packet capture and does not establish zero packet loss throughout the broader telemetry system.

The evidence supports successful post-remediation timestamp consistency, but not precise end-to-end delivery latency.


## 5. Research Limitations and Residual Risks

Although the corrective actions and validation tests produced successful results, the following limitations remain:

1. **Historical timestamp accuracy:** Events recorded before NTP synchronization cannot be assumed to have accurate timestamps. The approximately six-hour clock correction on EDGE-R1 introduced a discontinuity in the historical event timeline.

2. **Timestamp precision:** The embedded source timestamps were recorded at one-second precision. The tests therefore cannot establish exact millisecond-level clock skew.

3. **Event-delivery latency:** The packet capture confirms collector-side observation of the event, but the available measurements do not establish precise end-to-end event-delivery latency. Quantitative latency claims remain outside the scope of WP01-F04 and require separate validation under WP01-F10.

4. **Long-term synchronization stability:** Both devices demonstrated successful synchronization, but continuous clock accuracy has not been evaluated over an extended observation period.

5. **Restart persistence:** EDGE-R1's NTP service was registered for automatic startup, but successful synchronization following an actual router restart has not yet been tested.

6. **Evidence retention:** The console outputs and packet-capture results were reviewed during the investigation. This document records the observed results but does not itself constitute independently preserved original console transcripts or a saved PCAP file.

7. **Capture completeness:** The independent `tcpdump` test reported zero kernel drops for its single captured packet. This does not establish zero packet loss across the wider telemetry environment.

These limitations must accompany any research conclusions derived from the remediation.

The findings support restoration of clock synchronization and timestamp agreement at the tested precision. They do not independently establish precise latency, historical event-time accuracy, or long-term synchronization reliability.


## 6. Finding Disposition and Closure Assessment

**Finding ID:** WP01-F04
**Priority:** HIGH
**Final Disposition:** CLOSED WITH LIMITATIONS
**Formal Audit Status:** CLOSED WITH LIMITATIONS — recorded in the M6 Evidence Audit

### 6.1 Closure Basis

The investigation identified a substantial clock discrepancy affecting EDGE-R1 and an NTP synchronization deficiency affecting TELEMETRY-1.

Corrective actions restored DNS resolution and NTP synchronization on TELEMETRY-1, corrected EDGE-R1's clock, and enabled continuous NTP service operation with automatic startup configuration.

Post-remediation verification demonstrated:

1. Successful NTP communication and clock correction on EDGE-R1.
2. Successful NTP synchronization on TELEMETRY-1.
3. A post-correction EDGE-R1 NTP offset of approximately 4 milliseconds during the recorded diagnostic.
4. Successful reception and archival of two controlled syslog events with matching timestamps at one-second precision.
5. Independent collector-side packet capture demonstrating close alignment between an incoming event's embedded source timestamp and its capture timestamp.
6. Successful archival of the independently captured test event.

These results support the conclusion that the identified synchronization deficiencies were remediated and that source-versus-collector timestamp consistency was restored at the tested precision.

### 6.2 Residual Limitations

The verification does not establish precise end-to-end event-delivery latency, historical timestamp accuracy, or long-term clock stability.

Original console transcripts and packet-capture files were not independently archived as part of this remediation record. The documented observations must therefore be distinguished from retained raw evidence.

The separate WP01-F10 latency finding remains OPEN and must not be treated as resolved by the WP01-F04 remediation.

### 6.3 Final Disposition

**WP01-F04 is CLOSED WITH LIMITATIONS.** The disposition is supported by verified clock synchronization remediation, controlled timestamp validation tests, and the documented residual limitations.

This disposition recognizes the successful remediation of the identified clock synchronization deficiencies and the verified timestamp consistency at the tested precision, while preserving all documented residual limitations.

The corrective-action register was updated and verified on 2026-10-14. WP01-F04 is formally CLOSED WITH LIMITATIONS, supported by the documented remediation and timestamp validation results. The residual limitations remain applicable, and WP01-F10 remains OPEN.