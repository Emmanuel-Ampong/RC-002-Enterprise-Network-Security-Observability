\# M5-A02 - Increased Destination or Service Diversity



\## Project



RC-002 - Enterprise Network Security Observability and Telemetry



\## Milestone



M5 - Controlled Abnormal Behavior



\## Scenario



A02 - Increased Destination or Service Diversity



\## Status



DEFINED - NOT YET EXECUTED



\---



\## 1. Objective



Determine whether a predefined increase in destination and service-access diversity produces measurable telemetry differences from the normal-operation reference condition established during M4.



This experiment operationalizes the A02 scenario defined in the frozen RC-002 M5 Controlled Abnormal Behavior Protocol.



\---



\## 2. Research Boundary



A02 evaluates measurable changes in destination or service-access patterns.



It does not attempt to establish that the generated activity is:



\- malicious,

\- an attack,

\- an intrusion,

\- an automatically detected anomaly, or

\- evidence of threat detection.



Any observed difference shall be interpreted only within the controlled conditions of this experiment.



\---



\## 3. Independent Variable



The independent variable is the distribution of approved destinations and services contacted during the defined observation period.



The A02 condition increases destination/service diversity relative to the narrower normal-operation reference patterns characterized during M4.



\---



\## 4. Candidate Dependent Variables



The following measurements may be evaluated:



\- destination diversity,

\- service/port diversity,

\- protocol diversity,

\- connection frequency,

\- packet count,

\- traffic volume,

\- event characteristics, and

\- operational-preservation observations.



Only measurements supported by retained evidence shall be used in the final interpretation.



\---



\## 5. Laboratory Source



Primary source:



```text

SERVER-1

IPv4: 192.168.20.10

Zone: SERVER

```



SERVER-1 is used as the initiating endpoint for the predefined A02 sequence.



\---



\## 6. Approved Destination and Service Set



Only endpoints and services verified as available within the existing RC-002 laboratory or established external-connectivity reference shall be used.



\### D01 - TELEMETRY-1 ICMP



```text

Source:      SERVER-1

Destination: TELEMETRY-1

Address:     192.168.30.10

Protocol:    ICMP

```



\### D02 - TELEMETRY-1 SSH Listener



```text

Source:      SERVER-1

Destination: TELEMETRY-1

Address:     192.168.30.10

Protocol:    TCP

Port:        22

Service:     SSH

```



The experiment shall test TCP service reachability only. Interactive authentication attempts are outside the intended scope.



\### D03 - TELEMETRY-1 Syslog TCP Listener



```text

Source:      SERVER-1

Destination: TELEMETRY-1

Address:     192.168.30.10

Protocol:    TCP

Port:        514

Service:     Syslog

```



\### D04 - TELEMETRY-1 Syslog UDP Listener



```text

Source:      SERVER-1

Destination: TELEMETRY-1

Address:     192.168.30.10

Protocol:    UDP

Port:        514

Service:     Syslog

```



\### D05 - Established External Connectivity Reference



```text

Source:      SERVER-1

Destination: 8.8.8.8

Protocol:    ICMP

```



This destination is retained only as the external-connectivity reference previously used during normal-operation characterization.



\---



\## 7. Excluded Services



The following shall not be used to artificially increase diversity:



\- SERVER-1 loopback or self-directed HTTP traffic,

\- TELEMETRY-1 loopback-only DNS listeners,

\- newly installed services,

\- newly created destination hosts,

\- unauthorized external services, or

\- services introduced after execution begins.



SERVER-1 HTTP service on TCP/80 remains an established laboratory service but is not used as a self-directed A02 transaction because local self-access would not provide an equivalent routed network observation.



\---



\## 8. Reference Condition



The reference condition is the normal destination/service behavior documented during M4.



Relevant M4 scenarios include:



```text

N01 - ICMP SERVER-1 -> TELEMETRY-1

N02 - HTTP TELEMETRY-1 -> SERVER-1

N03 - Internet ICMP SERVER-1 -> 8.8.8.8

N04 - Normal host/system activity

```



A02 does not assume that these scenarios collectively represent all legitimate enterprise behavior.



They provide the defined RC-002 normal-operation references available for controlled comparison.



\---



\## 9. Experimental Condition



Each A02 trial shall execute the same predefined low-frequency sequence from SERVER-1:



```text

1\. ICMP -> 192.168.30.10

2\. TCP/22 -> 192.168.30.10

3\. TCP/514 -> 192.168.30.10

4\. UDP/514 -> 192.168.30.10

5\. ICMP -> 8.8.8.8

```



The purpose is to change the distribution of destinations, protocols, and services contacted during one observation period without introducing the high connection frequency tested separately in A01.



\---



\## 10. Trial Structure



Five trials shall be performed where practical.



```text

A02-T01

A02-T02

A02-T03

A02-T04

A02-T05

```



Each trial shall use the same source, destination/service sequence, commands, capture method, and measurement procedure.



No experimental parameters shall be changed between trials unless a documented technical problem requires intervention.



Any such intervention shall be recorded before continuing.



\---



\## 11. Packet Capture



A packet capture shall be collected for each trial using the appropriate GNS3 link or capture point that provides visibility into the SERVER-1 initiated traffic.



The capture shall retain sufficient evidence to identify:



\- source address,

\- destination address,

\- protocol,

\- destination port where applicable,

\- packet count,

\- conversation or flow characteristics where applicable, and

\- trial duration where measurable.



Raw packet-capture files remain excluded from Git where required by repository policy.



Derived measurements and representative screenshots may be retained.



\---



\## 12. Trial Measurements



For each trial, record where supported:



```text

Trial ID

Observed destinations

Observed protocols

Observed TCP destination ports

Destination count

Service/port count

Packet count

Captured bytes

Trial duration

ICMP success

TCP/22 reachability observation

TCP/514 reachability observation

UDP/514 transmission observation

External ICMP success

Operational-preservation result

Notes

```



A UDP transmission does not by itself prove application-level receipt. Any claim of UDP Syslog receipt must be supported separately by collector-side evidence.



\---



\## 13. Operational Preservation



The experiment must not intentionally disrupt legitimate laboratory operations.



At minimum, the following shall be checked during or immediately after each trial where practical:



\- TELEMETRY-1 remains reachable,

\- SERVER-1 remains operational,

\- established routing remains functional,

\- external connectivity remains available, and

\- centralized telemetry collection remains operational.



Observed failures shall be documented rather than silently corrected or omitted.



\---



\## 14. Comparison Strategy



A02 results shall be compared with the relevant M4 normal-operation references.



The analysis shall focus primarily on whether the predefined A02 condition produces measurable changes in:



\- number of destinations contacted,

\- number of services or destination ports contacted,

\- protocol distribution,

\- packet-level characteristics, and

\- relevant telemetry/event characteristics.



Traffic volume or connection count may be reported as secondary observations but shall not be treated as the primary manipulated variable.



This distinction prevents A02 from being interpreted as a repetition of A01.



\---



\## 15. Acceptance Criteria



A02 may be considered successfully executed if:



1\. The predefined sequence is performed consistently across the required trials.

2\. Packet-level evidence confirms the intended destination/service pattern.

3\. Relevant measurements can be extracted reproducibly.

4\. At least one defined diversity-related measurement can be compared with the M4 reference condition.

5\. Legitimate laboratory operations remain available, or any observed disruption is explicitly documented.

6\. Results are interpreted without claiming automated anomaly or threat detection.



Failure to produce a measurable difference shall remain a valid experimental outcome and shall not be altered or hidden.



\---



\## 16. Evidence Naming



Representative evidence shall use the following naming convention:



```text

M5-A02-01-Precheck.png

M5-A02-02-Trial-Execution.png

M5-A02-03-Packet-Evidence.png

M5-A02-04-Telemetry-Evidence.png

M5-A02-05-Operational-Preservation.png

```



Additional evidence may be added using sequential numbering where necessary.



\---



\## 17. Dataset Location



Structured A02 measurements shall be stored under:



```text

datasets/M5-Controlled-Abnormal-Behavior/A02-Destination-Service-Diversity/

```



Expected summary dataset:



```text

A02-Trial-Summary.csv

```



\---



\## 18. Results Location



Final A02 observations and interpretation shall be documented under:



```text

results/M5-Controlled-Abnormal-Behavior/A02-Results.md

```



\---



\## 19. Hypothesis Interpretation Boundary



A02 contributes evidence relevant to H2 only.



If the controlled A02 condition produces reproducible measurable differences from the relevant normal-operation references, the result may be described as scenario-specific evidence consistent with behavioral differentiation.



It shall not independently establish:



\- malicious behavior,

\- automated anomaly detection,

\- threat detection,

\- overall validation of H2, or

\- generalization beyond the RC-002 laboratory conditions.



Overall H2 interpretation shall be deferred until the required M5 controlled abnormal-behavior scenarios have been completed and evaluated together.



\---



\## 20. Execution State



```text

Protocol defined:        YES

Execution sheet frozen:  PENDING COMMIT

Experimental traffic:    NOT STARTED

Trials completed:        0/5

Analysis completed:      NO

```



No A02 experimental trial shall begin until this execution sheet has been reviewed, committed, and pushed.
