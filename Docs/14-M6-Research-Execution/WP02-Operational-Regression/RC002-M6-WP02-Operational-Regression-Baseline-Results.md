# RC-002 — M6 WP-02 Operational Regression Baseline Results

**Project:** Project Aegis  
**Research Component:** RC-002 — Enterprise Network Security Observability and Telemetry  
**Milestone:** M6 — Research Execution  
**Work Package:** WP-02 — Operational Regression Verification  
**Document Type:** Baseline Test Results  
**Test Execution Date:** 13 October 2026  
**Test Environment:** GNS3  
**Status:** Baseline execution completed; evidence documentation in progress  

## 1. Purpose

This document records the observed results of the WP-02 operational regression baseline tests (REG-01 through REG-10).

The baseline establishes a reference point for evaluating whether subsequent RC-002 changes preserve legitimate network connectivity, application accessibility, external connectivity, DNS resolution, routing, NAT, and Syslog collection.

The results support evaluation of:

- FR-11 — Operational Regression Verification
- NFR-03 — Operational Preservation
- RG-05 — Operational Regression Evidence Gap
- M-05 — Operational Preservation Metrics

Successful baseline tests demonstrate functionality at the time of execution. They do not independently establish continuous availability, zero operational impact, or successful preservation after future changes.

## 2. Baseline Test Results Summary

| Test ID | Verification Objective | Result | Evidence Summary |
|---|---|---|---|
| REG-01 | CLIENT-1 to USER gateway | PASS | 5/5 ICMP replies; 0% observed loss; average RTT 1.380 ms |
| REG-02 | CLIENT-1 to CLIENT-2 | PASS | 5/5 ICMP replies; 0% observed loss; average RTT 0.521 ms |
| REG-03 | CLIENT-1 to SERVER-1 | PASS | 5/5 ICMP replies; 0% observed loss; average RTT 2.692 ms |
| REG-04 | CLIENT-1 to TELEMETRY-1 | PASS | 5/5 ICMP replies; 0% observed loss; average RTT 2.775 ms |
| REG-05 | TELEMETRY-1 to SERVER-1 HTTP service | PASS | HTTP 200 OK; expected RC-002 test page returned |
| REG-06 | SERVER-1 external IP connectivity | PASS | 5/5 ICMP replies to 8.8.8.8; 0% observed loss; average RTT 46.684 ms |
| REG-07 | SERVER-1 external DNS resolution | PASS | example.com resolved through 8.8.8.8; IPv4 and IPv6 records returned |
| REG-08 | EDGE-R1 routing verification | PASS WITH LIMITATION | Expected connected networks and default route present; interface state and DHCP assignment not separately inspected |
| REG-09 | EDGE-R1 NAT verification | PASS WITH LIMITATION | MASQUERADE rule on eth3; 77 packets and 15,386 bytes counted; controlled counter delta not measured |
| REG-10 | Remote Syslog collection | PASS | Fresh EDGE-R1 and SERVER-1 events received on TELEMETRY-1; complete message contents confirmed by byte-level inspection |

**Execution Summary:** All 10 planned baseline tests were executed. Eight received PASS and two received PASS WITH LIMITATION. No functional failure was observed during these tests.

**Interpretation:** These results establish a point-in-time operational baseline. They do not demonstrate continuous availability or prove that subsequent configuration changes have no operational impact.

## 3. Detailed ICMP Connectivity Results

### 3.1 Test Environment

The following endpoints were used for the internal connectivity tests:

| Device | IP Address | Network Zone |
|---|---|---|
| CLIENT-1 | 192.168.10.10 | USER |
| CLIENT-2 | 192.168.10.11 | USER |
| EDGE-R1 eth0 | 192.168.10.1 | USER gateway |
| SERVER-1 | 192.168.20.10 | SERVER |
| TELEMETRY-1 | 192.168.30.10 | OBSERVABILITY |

### 3.2 ICMP Test Measurements

| Test ID | Source | Destination | Replies | Packet Loss | Minimum RTT | Average RTT | Maximum RTT | Status |
|---|---|---|---|---|---|---|---|---|
| REG-01 | CLIENT-1 | 192.168.10.1 | 5/5 | 0% | 1.000 ms | 1.380 ms | 2.084 ms | PASS |
| REG-02 | CLIENT-1 | 192.168.10.11 | 5/5 | 0% | 0.268 ms | 0.521 ms | 0.944 ms | PASS |
| REG-03 | CLIENT-1 | 192.168.20.10 | 5/5 | 0% | 2.186 ms | 2.692 ms | 3.503 ms | PASS |
| REG-04 | CLIENT-1 | 192.168.30.10 | 5/5 | 0% | 1.865 ms | 2.775 ms | 3.454 ms | PASS |

### 3.3 Observations

All four internal ICMP connectivity tests returned five successful replies, with no observed packet loss.

REG-01 verified connectivity between CLIENT-1 and its USER-zone gateway.

REG-02 verified same-zone connectivity between CLIENT-1 and CLIENT-2.

REG-03 verified inter-zone connectivity from CLIENT-1 to SERVER-1.

REG-04 verified inter-zone connectivity from CLIENT-1 to TELEMETRY-1.

The replies from SERVER-1 and TELEMETRY-1 reported TTL values of 63, consistent with traffic traversing one router.

### 3.4 Limitations

These measurements represent five ICMP probes per test destination. They establish point-in-time reachability but do not demonstrate continuous connectivity, sustained network performance, or absence of intermittent packet loss.

No configuration changes were introduced during these baseline connectivity tests.

## 4. Application and External Connectivity Results

### 4.1 REG-05 — HTTP Service Accessibility

**Source:** TELEMETRY-1 (192.168.30.10)  
**Destination:** SERVER-1 (192.168.20.10)  
**Command:** `curl -i --max-time 10 http://192.168.20.10/`  
**Result:** PASS

The HTTP request returned:

- HTTP status: 200 OK
- Web server: lighttpd/1.4.85
- Content type: text/html
- Content length: 361 bytes
- Application title: Project Aegis RC-002

The response contained the expected RC-002 application page, confirming that the HTTP service was accessible from the OBSERVABILITY zone during the test.

**Observation:** The final HTML paragraph contained an invalid closing sequence (`<>`). This did not prevent the HTTP request from succeeding. No corrective change was made during baseline testing.

### 4.2 REG-06 — External IP Connectivity

**Source:** SERVER-1 (192.168.20.10)  
**Destination:** 8.8.8.8  
**Command:** `ping -c 5 8.8.8.8`  
**Result:** PASS

| Measurement | Observed Value |
|---|---|
| Packets transmitted | 5 |
| Packets received | 5 |
| Packet loss | 0% |
| Minimum RTT | 43.644 ms |
| Average RTT | 46.684 ms |
| Maximum RTT | 52.521 ms |

The test confirmed external IPv4 reachability during execution.

Successful ICMP connectivity alone does not establish continuous internet availability or independently prove NAT translation behavior.

### 4.3 REG-07 — External DNS Resolution

**Source:** SERVER-1 (192.168.20.10)  
**DNS server:** 8.8.8.8  
**Command:** `nslookup example.com`  
**Result:** PASS

The DNS lookup returned the following records:

| Record Type | Address |
|---|---|
| AAAA | 2606:4700:10::ac42:93f3 |
| AAAA | 2606:4700:10::6814:179a |
| A | 104.20.23.154 |
| A | 172.66.147.243 |

The results confirm successful hostname resolution through the configured DNS server.

The presence of IPv6 DNS records does not independently demonstrate IPv6 network connectivity.

### 4.4 Interpretation and Limitations

REG-05, REG-06, and REG-07 passed during baseline execution.

Together, these tests establish point-in-time evidence of HTTP accessibility, external IPv4 connectivity, and external DNS resolution.

They do not establish uninterrupted service availability, sustained performance, or preservation following subsequent network changes.

## 5. EDGE-R1 Routing and NAT Verification

### 5.1 REG-08 — Routing Table Verification

**Device:** EDGE-R1  
**Command:** `ip -4 route show`  
**Result:** PASS WITH LIMITATION

The routing table contained the following entries:

| Destination | Next Hop / Interface | Observation |
|---|---|---|
| 192.168.10.0/24 | eth0 | USER network connected |
| 192.168.20.0/24 | eth1 | SERVER network connected |
| 192.168.30.0/24 | eth2 | OBSERVABILITY network connected |
| 192.168.42.0/24 | eth3 | WAN/NAT network connected |
| default | via 192.168.42.1 dev eth3 | Default route present |

The WAN interface's observed IPv4 address was 192.168.42.184.

The routing table confirmed the expected connected networks and default route during baseline execution.

**Limitation:** Interface operational state and DHCP lease details were not independently inspected during REG-08. The routing table alone does not establish continuous WAN availability.

### 5.2 REG-09 — NAT Configuration Verification

**Device:** EDGE-R1  
**Command:** `iptables -t nat -L POSTROUTING -n -v`  
**Result:** PASS WITH LIMITATION

The NAT inspection returned the following MASQUERADE rule:

| Parameter | Observed Value |
|---|---|
| Table | nat |
| Chain | POSTROUTING |
| Target | MASQUERADE |
| Outbound interface | eth3 |
| Source | 0.0.0.0/0 |
| Destination | 0.0.0.0/0 |
| Matched packets | 77 |
| Matched bytes | 15,386 |

The rule was present, and its nonzero counters demonstrated that traffic had matched it.

**Limitation:** These counters are cumulative. No before-and-after counter measurements were taken around a controlled traffic generation event. Therefore, the observed counts cannot be attributed specifically to the WP-02 regression tests.

### 5.3 Interpretation

REG-08 and REG-09 established point-in-time evidence that EDGE-R1 had the expected IPv4 routes and an outbound MASQUERADE rule.

The findings support the operational baseline but do not independently establish continuous routing availability, DHCP lease stability, or controlled NAT translation performance.

Both tests are retained as PASS WITH LIMITATION pending additional verification where required.

## 6. Remote Syslog Collection Verification

### 6.1 REG-10 — Verification Objective

**Test ID:** REG-10  
**Collector:** TELEMETRY-1 (192.168.30.10)  
**Sources:** EDGE-R1 and SERVER-1  
**Result:** PASS

The objective was to confirm that TELEMETRY-1 could receive and retain fresh Syslog events from both network infrastructure and application-server sources.

### 6.2 EDGE-R1 Syslog Verification

**Source:** EDGE-R1  
**Source IP observed at collector:** 192.168.30.1  
**Event tag:** RC002-REG10  
**Collector file:** `/var/log/remote/192.168.30.1/syslog.log`  
**Event timestamp:** 2026-10-13T09:46:00+00:00

The following command generated the test event:

`logger -t RC002-REG10 "WP02 operational regression syslog verification from EDGE-R1"`

The collector retained the following record:

```text
2026-10-13T09:46:00+00:00 _gateway RC002-REG10: WP02 operational regression syslog verification from EDGE-R1

 6.3 SERVER-1 Syslog Verification

**Source:** SERVER-1
Source IP observed at collector: 192.168.20.10
Event tag: RC002-REG10-SERVER
Collector file: /var/log/remote/192.168.20.10/syslog.log
Event timestamp: 2026-10-13T09:53:21+00:00
The following command generated the test event:
logger -t RC002-REG10-SERVER "WP02 operational regression syslog verification from SERVER-1"
The collector retained the following record:
2026-10-13T09:53:21+00:00 192.168.20.10 RC002-REG10-SERVER: WP02 operational regression syslog verification from SERVER-1


The complete message was verified using byte-level inspection of the stored record.
Result: PASS


6.4 Collector Storage Configuration
The collector's rsyslog configuration uses the following destination template:
/var/log/remote/%FROMHOST-IP%/syslog.log
This organizes received remote Syslog events by source IP address.
Initial searches of /var/log/syslog did not locate the test events. Subsequent searches of the configured remote-log directory successfully located both records.


6.5 Message Integrity Verification
Both test records initially appeared truncated in the GNS3 console display.
Byte-level inspection using od confirmed that the complete intended message content was retained in each file.
The apparent truncation was therefore attributed to console-display artifacts rather than demonstrated loss of stored message content.


6.6 Interpretation and Limitations
REG-10 passed because fresh, source-attributed Syslog events from EDGE-R1 and SERVER-1 were found at TELEMETRY-1, and their complete message contents were verified.
This result establishes successful collection during the test window. It does not independently demonstrate uninterrupted Syslog delivery, guaranteed event completeness under load, or zero event loss during future network changes.

## 7. Baseline Assessment and Remaining Evidence Gaps

### 7.1 Overall Baseline Assessment

All 10 planned WP-02 operational regression tests were executed.

Eight tests received PASS, while two received PASS WITH LIMITATION.

No functional failure was observed during the baseline test execution.

The results establish a point-in-time reference for legitimate network connectivity, application accessibility, external connectivity, DNS resolution, routing, NAT configuration, and remote Syslog collection.

### 7.2 Requirements Traceability

| Reference | Baseline Contribution | Assessment |
|---|---|---|
| FR-11 — Operational Regression Verification | A defined regression suite was executed and its observed results documented | Baseline established; post-change comparison pending |
| NFR-03 — Operational Preservation | Legitimate connectivity and selected services were operational during testing | Point-in-time evidence only |
| RG-05 — Operational Regression Evidence Gap | A standardized test suite and baseline results address part of the previously identified evidence gap | Partially addressed; before-and-after verification remains outstanding |
| M-05 — Operational Preservation Metrics | Connectivity, HTTP, DNS, routing, NAT, and Syslog observations were recorded | Baseline measurements available; comparative metrics pending |

### 7.3 Remaining Evidence Gaps

The following limitations remain:

1. No equivalent post-change regression results have been collected for comparison with this baseline.
2. Continuous service availability and absence of intermittent failures have not been demonstrated.
3. REG-08 did not independently verify WAN interface operational state or DHCP lease details.
4. REG-09 did not measure NAT counter changes during controlled traffic generation.
5. Syslog collection was verified using selected test events, not sustained delivery or event-loss measurements.
6. The previously documented transient external connectivity issue remains unexplained.
7. Four transcript-derived evidence records covering REG-01 through REG-10 have been archived under Baseline-Evidence. However, independently exported original GNS3 console captures have not yet been archived in the repository.

### 7.4 Baseline Conclusion

The WP-02 operational regression baseline was successfully executed, with eight PASS results and two PASS WITH LIMITATION results.

This baseline provides a reference for subsequent operational preservation assessments.

FR-11, NFR-03, and RG-05 should not be considered fully satisfied or closed solely on the basis of these baseline results.

A subsequent regression run, performed after a clearly identified network or observability change, is required to support a defensible before-and-after comparison.