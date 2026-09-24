\# RC-002 M5 - A01 Elevated Connection Frequency



\## Experiment Execution Sheet



\*\*Project:\*\* Project Aegis

\*\*Research Cycle:\*\* RC-002

\*\*Milestone:\*\* M5 - Controlled Abnormal Behavior

\*\*Scenario:\*\* A01 - Elevated Connection Frequency

\*\*Status:\*\* Pre-execution parameters frozen



\---



\## 1. Objective



Determine whether a controlled increase in legitimate HTTP request frequency produces measurable telemetry characteristics different from the corresponding M4 normal-operation condition.



A01 manipulates connection/request frequency while preserving the authorized source, destination, application service, routing environment, and telemetry architecture.



A measurable difference is not, by itself, interpreted as evidence of malicious activity or automated threat detection.



\---



\## 2. Reference Condition



The reference condition is M4 scenario N02 - HTTP Application Access.



Reference path:



TELEMETRY-1 (192.168.30.10) -> SERVER-1 (192.168.20.10:80)



The M4 N02 baseline used controlled legitimate HTTP access to SERVER-1.



Recorded M4 observations included:



\- HTTP 200 response in all five baseline trials.

\- 10 captured TCP packets per trial.

\- 0 kernel packet-capture drops.

\- Mean completion time: 0.0097632 seconds.

\- Observed completion-time range: 0.007903-0.011616 seconds.



These measurements provide the reference condition for A01 comparison.



\---



\## 3. Independent Variable



Connection/request frequency.



A01 increases the frequency of legitimate HTTP requests relative to the M4 N02 reference condition.



\---



\## 4. Controlled Variables



The following shall remain unchanged during A01 unless required to recover from a technical failure:



\- Source host: TELEMETRY-1.

\- Source network: 192.168.30.0/24.

\- Destination host: SERVER-1.

\- Destination address: 192.168.20.10.

\- Destination service: HTTP/TCP port 80.

\- SERVER-1 Lighttpd configuration.

\- EDGE-R1 routing configuration.

\- Network topology.

\- IP addressing.

\- Telemetry configuration.

\- Syslog forwarding configuration.

\- Packet-capture location and procedure.

\- Request-generation procedure.

\- Number of requests per trial.

\- Inter-request interval.

\- Rest interval between trials.



Any required deviation shall be documented before the affected trial is interpreted.



\---



\## 5. Frozen A01 Parameters



| Parameter | Value |

|---|---|

| Source | TELEMETRY-1 (192.168.30.10) |

| Destination | SERVER-1 (192.168.20.10) |

| Service | HTTP/TCP 80 |

| Requests per trial | 20 |

| Inter-request interval | 0.25 seconds |

| Expected request-generation window | Approximately 5 seconds |

| Number of controlled trials | 5 |

| Rest interval between trials | 30 seconds |

| Authorization | Legitimate laboratory traffic |

| Experimental change | Increased request frequency |



The same request count, interval, source, destination, and service shall be used for all five trials.



\---



\## 6. Request-Generation Procedure



For each trial, TELEMETRY-1 shall generate 20 sequential HTTP requests to:



`http://192.168.20.10/`



A 0.25-second interval shall be inserted between requests.



The request-generation procedure shall remain identical across all five trials.



The exact execution command shall be recorded with the trial evidence.



\---



\## 7. Evidence to Collect



For each trial, retain or record where technically practical:



\- Trial identifier.

\- Trial start timestamp.

\- Trial end timestamp.

\- Number of requests attempted.

\- Number of successful HTTP responses.

\- Number of failed requests.

\- HTTP response status.

\- Total trial duration.

\- Packet count.

\- TCP connection behavior.

\- Source and destination addresses.

\- Source and destination ports where applicable.

\- Traffic volume where available.

\- Packet-capture drop information where available.

\- Relevant centralized telemetry observations.

\- Operational-preservation observations.



\---



\## 8. Packet Capture



Packet-level evidence shall be collected for the TELEMETRY-1 to SERVER-1 HTTP interaction.



The capture should permit observation of:



\- TCP connection establishment.

\- HTTP request/response behavior.

\- TCP connection termination.

\- Packet count.

\- Timing.

\- Repeated connection behavior.

\- Source/destination attribution.



Raw packet-capture files remain subject to the repository `.gitignore` policy.



Derived measurements and relevant screenshots may be retained in the repository.



\---



\## 9. Trial Structure



The experiment consists of:



\- A01-T1

\- A01-T2

\- A01-T3

\- A01-T4

\- A01-T5



Each trial shall use the frozen parameters defined in this document.



A 30-second rest interval shall separate completed trials.



If a trial is invalidated by an unrelated technical failure, the reason shall be documented and the trial shall not silently be replaced.



\---



\## 10. Operational Preservation



During A01, the experiment shall verify that the controlled increase in request frequency does not prevent legitimate operation of SERVER-1.



At minimum, observations shall determine whether:



\- SERVER-1 continues responding to HTTP requests.

\- Routing remains functional.

\- TELEMETRY-1 remains operational.

\- Centralized telemetry collection remains operational.



Successful preservation under A01 shall be interpreted only within the tested laboratory scope.



\---



\## 11. Comparison Plan



A01 results shall be compared with the M4 N02 normal reference condition.



The comparison may consider:



\- Request/connection frequency.

\- Packet count.

\- Traffic volume.

\- TCP behavior.

\- Application response behavior.

\- Completion duration.

\- Centralized telemetry observations.

\- Operational preservation.



Observed differences shall be reported as measurable behavioral differences.



They shall not automatically be characterized as malicious activity, anomalies, attacks, or successful threat detection.



\---



\## 12. Acceptance Conditions



A01 execution is considered valid when:



1\. The pre-experiment environment is operational.

2\. The predefined source, destination, and HTTP service are used.

3\. Twenty requests are attempted per valid trial.

4\. The 0.25-second inter-request interval is maintained by the defined procedure.

5\. Five controlled trials are completed where technically practical.

6\. Relevant packet and/or telemetry evidence is retained.

7\. Trial timestamps and outcomes are recorded.

8\. Operational-preservation observations are recorded.

9\. Deviations or failed trials are explicitly documented.

10\. Results are compared against the corresponding M4 N02 baseline without overclaiming detection capability.



\---



\## 13. Pre-Execution State



At the time of experiment preparation:



\- TELEMETRY-1 rsyslog service was verified active.

\- SERVER-1 centralized telemetry was accessible on TELEMETRY-1.

\- EDGE-R1 centralized telemetry was accessible on TELEMETRY-1.

\- No A01 experimental traffic had yet been intentionally generated.



The remaining network pre-checks shall be completed before A01-T1 if not already verified.



\---



\## 14. Research Integrity Note



The numerical A01 parameters in this execution sheet were defined before A01 experimental execution.



This document operationalizes the previously committed M5 Controlled Abnormal Behavior Protocol. It does not alter the M5 research hypothesis or the conceptual definition of A01.



Any post-execution change to these parameters must be documented as a methodological change and must not be represented as having been predefined.
