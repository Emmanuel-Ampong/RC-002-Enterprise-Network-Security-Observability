\# RC-002 M5 - A01 Results



\## Elevated Connection Frequency



\*\*Project:\*\* Project Aegis

\*\*Research Cycle:\*\* RC-002

\*\*Milestone:\*\* M5 - Controlled Abnormal Behavior

\*\*Scenario:\*\* A01 - Elevated Connection Frequency

\*\*Status:\*\* Five controlled trials completed



\---



\## 1. Objective



A01 evaluated whether a controlled increase in legitimate HTTP request frequency produced measurable telemetry characteristics different from the corresponding M4 normal-operation reference condition.



The experiment manipulated request frequency while preserving the authorized source, destination, application service, routing environment, and telemetry architecture.



\---



\## 2. Experimental Path



Source:



`TELEMETRY-1 (192.168.30.10)`



Destination:



`SERVER-1 (192.168.20.10:80)`



Application:



`HTTP`



Each controlled trial generated 20 sequential HTTP requests with a nominal 0.25-second inter-request interval.



Five trials were executed.



\---



\## 3. Trial Results



| Trial | Requests | HTTP 200 | Failures | TCP Conversations | Displayed Packets | Displayed Bytes | Traffic Span |

|---|---:|---:|---:|---:|---:|---:|---:|

| A01-T1 | 20 | 20 | 0 | 20 | 200 | 26,560 | 6.182 s |

| A01-T2 | 20 | 20 | 0 | 20 | 200 | 26,560 | 6.090 s |

| A01-T3 | 20 | 20 | 0 | 20 | 200 | 26,560 | 6.168 s |

| A01-T4 | 20 | 20 | 0 | 20 | 200 | 26,560 | 6.232 s |

| A01-T5 | 20 | 20 | 0 | 20 | 200 | 26,560 | 6.147 s |



\---



\## 4. Aggregate Results



Across the five controlled trials:



\- Total requests attempted: 100.

\- Successful HTTP 200 responses: 100.

\- Failed requests: 0.

\- Application success rate: 100%.

\- Total TCP conversations: 100.

\- Total displayed HTTP/TCP packets: 1,000.

\- Total displayed bytes: 132,800.

\- Packets per HTTP transaction: 10.

\- Mean filtered traffic span: 6.164 seconds.

\- Minimum filtered traffic span: 6.090 seconds.

\- Maximum filtered traffic span: 6.232 seconds.

\- Observed traffic-span range: 0.142 seconds.

\- No packet-capture drops were reported in the retained Wireshark statistics.



The five trials therefore produced highly consistent packet and application behavior under the defined A01 condition.



\---



\## 5. Comparison with M4 N02



The corresponding M4 normal reference condition was N02 - HTTP Application Access.



M4 N02 recorded:



\- One controlled HTTP transaction per trial.

\- HTTP 200 in all five baseline trials.

\- 10 TCP packets per trial.

\- 0 kernel packet-capture drops.

\- Mean completion time of 0.0097632 seconds.

\- Observed completion-time range of 0.007903-0.011616 seconds.



A01 retained the same source, destination, and HTTP service while increasing the number and frequency of requests.



Each A01 trial generated:



\- 20 HTTP requests.

\- 20 TCP conversations.

\- 200 displayed HTTP/TCP packets.



Relative to the single-transaction M4 N02 reference trial, A01 therefore produced a 20-fold increase in the number of controlled HTTP transactions and a corresponding 20-fold increase in observed HTTP/TCP packet count per trial.



This difference was reproducible across all five A01 trials.



\---



\## 6. Behavioral Interpretation



A01 demonstrates that the selected packet-level measurements make the deliberately increased HTTP request frequency observable.



The principal measurable differences were:



\- Increased request frequency.

\- Increased connection frequency.

\- Increased packet count.

\- Increased traffic volume over the controlled trial window.



The underlying application interaction remained legitimate and successful.



The observed difference should therefore be interpreted as a measurable behavioral difference associated with the controlled experimental condition.



It is not, by itself, evidence of malicious activity, automated anomaly detection, or successful threat detection.



\---



\## 7. Operational Preservation



SERVER-1 remained responsive throughout all five A01 trials.



All 100 HTTP requests returned HTTP 200.



No trial produced an observed application failure.



Routing and the tested HTTP service therefore remained operational under the A01 condition.



This provides evidence consistent with operational preservation within the tested scope.



It does not establish zero performance impact under other workloads or operating conditions.



\---



\## 8. H2 Relevance



A01 provides evidence relevant to RC-002 Hypothesis H2.



The selected telemetry measurements exhibited a clear and reproducible difference between the M4 N02 reference condition and the controlled A01 elevated-frequency condition.



A01 therefore supports the proposition that selected telemetry can make this predefined behavioral change measurable.



No overall conclusion regarding H2 is made at this stage because the remaining predefined M5 scenarios have not yet been evaluated.



\---



\## 9. Measurement Limitation



Manual shell timestamps were collected during the trials but were not consistently synchronized with the exact beginning and end of request generation.



For this reason, manual timestamp differences are not used as the primary measure of A01 traffic duration.



Wireshark's filtered traffic span is used as the primary trial-span measurement because it directly represents the observed experimental HTTP/TCP traffic.



This limitation does not affect the recorded request counts, HTTP response outcomes, TCP conversation counts, packet counts, or displayed byte counts.



\---



\## 10. Conclusion



A01 was completed across five controlled trials using the predefined experimental condition.



The experiment produced reproducible measurable differences from the corresponding M4 N02 reference condition while preserving the tested HTTP application operation.



A01 is therefore complete within its defined experimental scope.



The next predefined M5 scenario is A02 - Increased Destination or Service Diversity.
