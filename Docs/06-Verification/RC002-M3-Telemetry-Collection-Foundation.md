# RC002-M3 — Telemetry Collection Foundation Verification

## 1. Milestone

**Milestone:** M3 — Telemetry Collection Foundation
**Project:** RC-002 — Enterprise Network Security Observability and Telemetry
**Project Aegis Phase:** OBSERVE
**Status:** VERIFIED / PASS

---

## 2. Objective

Establish centralized collection of security-relevant telemetry from selected
RC-002 network components into TELEMETRY-1 while preserving legitimate network
operation.

M3 establishes the telemetry collection foundation required for later
experiments involving abnormal behavior, detection, correlation, and analysis.

M3 does **not** claim anomaly detection or automated threat detection.

---

## 3. Telemetry Architecture

The verified telemetry path is:

SERVER-1 (192.168.20.10)
        |
        | Syslog UDP/514
        |
        v
EDGE-R1
        |
        v
TELEMETRY-1 (192.168.30.10)

EDGE-R1 (192.168.30.1)
        |
        | Syslog UDP/514
        |
        v
TELEMETRY-1 (192.168.30.10)

TELEMETRY-1 operates as the centralized telemetry collector using rsyslog.

Remote telemetry is stored according to observed source IP:

- SERVER-1:
  `/var/log/remote/192.168.20.10/syslog.log`

- EDGE-R1:
  `/var/log/remote/192.168.30.1/syslog.log`

---

## 4. Collector Configuration

TELEMETRY-1 uses rsyslog as the centralized logging service.

Remote Syslog reception was enabled for:

- UDP port 514
- TCP port 514

A dynamic file template was configured to separate remotely received telemetry
according to `%FROMHOST-IP%`.

This produced source-specific storage under:

`/var/log/remote/<SOURCE-IP>/syslog.log`

The rsyslog configuration passed syntax validation using:

`rsyslogd -N1`

The rsyslog service was subsequently verified as active and listening on the
configured ports.

---

## 5. Telemetry Sources

### 5.1 SERVER-1

SERVER-1 uses BusyBox syslogd.

Persistent remote forwarding was configured through:

`/etc/conf.d/syslog`

with:

`SYSLOGD_OPTS="-t -L -R 192.168.30.10:514"`

This preserves local logging while forwarding telemetry to TELEMETRY-1.

A controlled test event was generated with the tag:

`RC002-M3-SERVER1`

The event was successfully received and stored under:

`/var/log/remote/192.168.20.10/syslog.log`

Persistence was verified after restart.

### 5.2 EDGE-R1

EDGE-R1 also uses BusyBox syslogd managed through OpenRC.

Persistent forwarding was configured using:

`SYSLOGD_OPTS="-t -L -R 192.168.30.10:514"`

A controlled event tagged:

`RC002-M3-EDGER1`

was successfully received by TELEMETRY-1.

The collector observed the router's telemetry source address as:

`192.168.30.1`

and stored it under:

`/var/log/remote/192.168.30.1/syslog.log`

This source identity was observed experimentally rather than assumed.

---

## 6. Acceptance Criteria and Results

| ID | Acceptance Criterion | Result |
|---|---|---|
| AC-01 | TELEMETRY-1 listens for remote Syslog | PASS |
| AC-02 | SERVER-1 forwards telemetry centrally | PASS |
| AC-03 | EDGE-R1 forwards telemetry centrally | PASS |
| AC-04 | Central logs remain attributable to the originating source | PASS |
| AC-05 | Deliberately generated known events are observable centrally | PASS |
| AC-06 | Legitimate routing, Internet/NAT, and HTTP operations remain functional | PASS |
| AC-07 | Telemetry forwarding persists after restart where tested | PASS |
| AC-08 | Verification evidence is captured and documented | PASS |

---

## 7. Multi-Source Attribution Test

Controlled telemetry events were generated independently on SERVER-1 and
EDGE-R1.

TELEMETRY-1 successfully retained the two sources separately:

SERVER-1:
`192.168.20.10`

EDGE-R1:
`192.168.30.1`

The experiment demonstrates that the centralized collector can receive and
distinguish telemetry from multiple network sources.

This result provides evidence toward RC-002 Hypothesis H1.

---

## 8. Persistence Verification

Persistence testing was performed to determine whether telemetry functionality
survived device restart.

SERVER-1 automatically restored its BusyBox syslogd remote-forwarding
configuration after restart.

EDGE-R1 automatically restored its Syslog forwarding configuration after
restart.

Post-restart controlled events from both systems were subsequently observed on
TELEMETRY-1 in their respective source-specific logs.

Therefore, the tested centralized telemetry configuration demonstrated
persistence across the performed restart tests.

---

## 9. Infrastructure Persistence Finding

During EDGE-R1 restart verification, an infrastructure limitation inherited
from the M2 baseline was identified.

After the initial router reload:

- the internal interface configuration remained available;
- Syslog forwarding remained persistent;
- eth3 did not automatically recover its DHCP configuration;
- the previously entered iptables MASQUERADE rule was absent;
- external connectivity therefore did not initially recover automatically.

This finding was treated separately from telemetry persistence rather than
being attributed to the telemetry implementation.

### Corrective Action

Persistent WAN DHCP configuration was added through:

`/etc/network/interfaces`

using:

`auto eth3`

`iface eth3 inet dhcp`

The existing OpenRC networking service was already enabled at boot.

The verified NAT rule:

`-A POSTROUTING -o eth3 -j MASQUERADE`

was saved using the appliance's native iptables persistence mechanism:

`/etc/iptables/rules-save`

The OpenRC iptables service was then enabled at the boot runlevel.

A subsequent EDGE-R1 reboot demonstrated automatic recovery of:

- eth3 DHCP addressing;
- the default WAN route;
- the MASQUERADE rule;
- the Syslog forwarding service; and
- EDGE-R1 external connectivity.

This corrective action improved reproducibility of the RC-002 network baseline.

---

## 10. Preservation of Legitimate Operation

After telemetry deployment and persistence testing, SERVER-1 was used to
validate legitimate network operation.

The following tests passed:

1. SERVER-1 → TELEMETRY-1 (`192.168.30.10`)
   - 3/3 ICMP replies
   - 0% packet loss

2. SERVER-1 → Internet (`8.8.8.8`)
   - 3/3 ICMP replies
   - 0% packet loss

3. SERVER-1 HTTP service
   - HTTP request to `http://192.168.20.10/`
   - Project Aegis RC-002 test page returned successfully

These observations provide preliminary evidence toward Hypothesis H3: the
introduction of the tested telemetry components did not prevent the legitimate
operations evaluated in this milestone.

They do not establish that telemetry has zero performance or operational
impact under all workloads.

---

## 11. Evidence

| Evidence | Description |
|---|---|
| M3-01 | Central rsyslog collector active and listening on Syslog ports |
| M3-02 | SERVER-1 centralized telemetry |
| M3-03 | EDGE-R1 centralized telemetry |
| M3-04 | Multi-source telemetry attribution |
| M3-05 | Post-reboot telemetry persistence |
| M3-06 | Preservation of legitimate network operation |

Evidence files:

`Images/M3-Evidence/M3-01-Central-Syslog-Collector-Listening.png`

`Images/M3-Evidence/M3-02-SERVER1-Centralized-Telemetry.png`

`Images/M3-Evidence/M3-03-EDGER1-Centralized-Telemetry.png`

`Images/M3-Evidence/M3-04-Multi-Source-Telemetry-Attribution.png`

`Images/M3-Evidence/M3-05-Post-Reboot-Telemetry-Persistence.png`

`Images/M3-Evidence/M3-06-Network-Operation-Preserved.png`

---

## 12. Hypothesis Implications

### H1 — Centralized Observability

**Evidence strengthened.**

M3 demonstrates centralized collection of telemetry from SERVER-1 and EDGE-R1
with source-specific attribution.

M3 does not yet establish comprehensive observability across every possible
RC-002 data source.

### H2 — Differentiation of Normal and Abnormal Conditions

**Not tested in M3.**

No controlled abnormal-behavior experiment was performed for the purpose of
distinguishing normal from abnormal conditions.

H2 remains for later milestones.

### H3 — Preservation of Legitimate Operations

**Preliminary evidence strengthened.**

Routing, Internet/NAT connectivity, centralized telemetry, and the SERVER-1
HTTP service remained operational after the final configuration and persistence
verification.

More extensive performance and operational testing may be required before
drawing broader conclusions.

---

## 13. Scope Boundary

M3 establishes a centralized telemetry collection foundation.

It does not demonstrate:

- anomaly detection;
- intrusion detection;
- malicious-traffic classification;
- behavioral baselining;
- automated correlation;
- SIEM analytics;
- machine-learning detection;
- automated incident response.

Those capabilities remain outside the scope of M3 and are reserved for later
Project Aegis milestones.

---

## 14. Milestone Conclusion

M3 successfully established and verified a persistent centralized telemetry
foundation for RC-002.

TELEMETRY-1 received source-attributable telemetry from SERVER-1 and EDGE-R1,
controlled events were observed centrally, and the tested telemetry
configuration survived restart.

Legitimate routed connectivity, external NAT connectivity, and the SERVER-1
HTTP service remained functional after deployment.

An infrastructure persistence limitation inherited from the M2 baseline was
identified during restart testing and corrected using the platform's native
networking and iptables persistence mechanisms.

**M3 Status: VERIFIED / PASS**

