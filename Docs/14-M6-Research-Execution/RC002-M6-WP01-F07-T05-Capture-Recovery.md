# RC002-M6-WP01-F07 — A02-T05 Historical Packet-Capture Recovery

**Project:** Project Aegis — RC-002 Enterprise Network Security Observability

**Milestone:** M6 — Research Execution

**Work Package:** WP01 — Evidence Audit and Remediation

**Finding:** WP01-F07 — Missing Quantitative A02 Traffic Measurements

**Original Experiment:** M5-A02 — Increased Destination or Service Diversity

**Recovered Trial:** A02-T05

**Original Capture Date:** 2026-10-08

**Status:** Partial quantitative evidence recovered; WP01-F07 remains OPEN

---

## 1. Objective

Recover reproducible quantitative packet-level measurements from an existing GNS3 capture associated with A02-T05.

The investigation addresses part of WP01-F07, which identifies missing packet counts, captured-byte measurements, and trial durations in the original A02 trial records.

Historical evidence is preferred over retrospective estimates or unsupported assumptions.

## 2. Original Evidence Source

**Capture filename:** `SERVER-1_eth0_to_SERVER-SW1_Ethernet1.pcap`

**Capture location:**

`C:\Users\Emma\GNS3\projects\RC002-M2-Minimum-Observability-Topology\project-files\captures\SERVER-1_eth0_to_SERVER-SW1_Ethernet1.pcap`

**Capture source:** SERVER-1 to SERVER-SW1 GNS3 network link.

**SHA-256 fingerprint:**

`e5340d37ac67b3f95c6e4f1a1f39122d554b5c48eb40d495584d50215d7bf613`

The fingerprint was generated using Windows CertUtil.

The original capture remains in GNS3 project storage. It has not been copied into the Git repository as part of this recovery step.

## 3. Trial Attribution

TShark inspection identified the following UDP/514 packet:

| Field | Observed value |
|---|---|
| Frame number | 66 |
| Timestamp | 2026-10-08T14:33:01.936505Z |
| Source IP | 192.168.20.10 |
| Destination IP | 192.168.30.10 |
| Source UDP port | 56853 |
| Destination UDP port | 514 |
| Syslog payload | `RC002-A02-T05-UDP514` |
| Captured frame length | 63 bytes |

The explicit T05 marker identifies frame 66 as an A02-T05 experimental packet.

Surrounding traffic closely matches the predefined A02 sequence:

| Frames | Observed traffic |
|---|---|
| 40–45 | Three ICMP requests and replies involving TELEMETRY-1 |
| 46–49 | ARP traffic |
| 50–58 | TCP/22 communication |
| 59–65 | TCP/514 communication |
| 66 | Confirmed A02-T05 UDP/514 marker |
| 67–72 | Three ICMP requests and replies involving 8.8.8.8 |

This correspondence supports identifying frames 40–72 as a reconstructed T05-related capture window.

It does not independently prove that every frame in the window was caused by the experiment or that the window represents the complete trial execution period.

## 4. Recovered Quantitative Measurements

**Selected capture window:** Frames 40–72, inclusive.

| Measurement | Value |
|---|---|
| First frame | 40 |
| Last frame | 72 |
| First frame timestamp | 2026-10-08T14:32:40.685747Z |
| Last frame timestamp | 2026-10-08T14:33:12.095739Z |
| Number of captured frames | 33 |
| Sum of captured frame lengths | 2,549 bytes |
| Elapsed first-to-last-frame time | 31.410 seconds |

### Measurement interpretation

**Packet count:** 33 frames were retained within the selected frame-number range.

**Captured bytes:** 2,549 bytes represents the sum of the captured frame lengths, including associated ARP frames and other traffic in the window. It is not equivalent to application payload volume or exclusively experimental traffic volume.

**Duration:** 31.410 seconds is the difference between the first and last packet timestamps. It is not an independently recorded start-to-finish experimental duration.

These three measurements describe the reconstructed capture window only.

## 5. Recovery Methodology

The original capture was inspected using TShark from the Windows Wireshark installation.

The A02 marker was located by searching for the hexadecimal representation of `RC002-A02`.

The packet sequence surrounding the identified T05 marker was then examined to establish a candidate observation window.

Captured-frame counts and byte totals were calculated from TShark field output for frames 40–72.

The duration was calculated from the corresponding first and last packet epoch timestamps.

The capture's SHA-256 fingerprint was recorded to support future evidence-integrity checks.

No original packet-capture contents were intentionally modified during this investigation.

## 6. Limitations

1. **Retrospective measurement boundaries:** The original experiment start and end times were not independently recovered. Frame 40 is a candidate start based on the observed traffic sequence.

2. **Incomplete trial attribution:** The confirmed T05 marker identifies frame 66, but the complete 33-frame window contains traffic whose individual relationship to the trial has not been independently established.

3. **Potential background traffic:** ARP and other network activity are included in the unfiltered capture-window totals.

4. **Trial coverage:** Equivalent quantitative measurements have not been recovered for A02-T01 through A02-T04.

5. **No definitive normal-versus-abnormal comparison:** These measurements alone do not establish a quantified difference from the M4 normal-operation references.

6. **Source evidence preservation:** The original capture is stored outside the Git repository. The recorded SHA-256 fingerprint enables subsequent comparison but does not guarantee that the external file will remain available.

7. **Separate T02 limitation:** Recovery of the T05 UDP/514 marker does not resolve WP01-F06, the missing UDP/514 marker observation in A02-T02.

## 7. WP01-F07 Remediation Assessment

| Requirement | Assessment |
|---|---|
| Recover A02-related packet evidence | Partially achieved |
| Identify original T05 marker | Confirmed |
| Recover T05-related packet count | 33 frames in selected window |
| Recover T05-related captured bytes | 2,549 bytes in selected window |
| Recover T05-related elapsed duration | 31.410 seconds |
| Establish complete T05 trial boundaries | Not established |
| Recover equivalent measurements for all five trials | Not achieved |
| Complete WP01-F07 remediation | Not achieved |

**Recommended finding disposition: OPEN.**

The historical capture supports a reproducible, qualified quantitative observation for A02-T05. However, it does not fully satisfy the five-trial quantitative measurement requirement.

Additional recovery or controlled measurement work may be necessary.

## 8. Conclusion

The investigation successfully recovered an identifiable A02-T05 UDP/514 marker and a surrounding traffic sequence consistent with the original experimental procedure.

The selected 33-frame observation window contains **2,549 captured bytes** and spans **31.410 seconds**.

These results strengthen the retained quantitative evidence for A02-T05 without overstating what can be inferred from the historical capture.

WP01-F07 remains **OPEN** pending further evidence assessment and appropriate corrective action.