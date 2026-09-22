\# RC-002 M5 — Controlled Abnormal-Behavior Experimental Protocol



\*\*Project:\*\* Project Aegis — RC-002 Enterprise Network Security Observability and Telemetry  

\*\*Project Aegis Phase:\*\* OBSERVE  

\*\*Milestone:\*\* M5 — Controlled Abnormal-Behavior Experiments  

\*\*Protocol Status:\*\* PRE-EXPERIMENT  

\*\*Primary Hypothesis:\*\* H2 — Behavioral Differentiation



\---



\## 1. Purpose



M5 evaluates whether selected telemetry measurements collected during predefined controlled abnormal operating conditions exhibit measurable differences from measurements obtained during the M4 normal-operation baseline.



M4 established a measured reference condition for legitimate network and system behavior. M5 introduces controlled changes to selected operating conditions while retaining the existing RC-002 topology and observability architecture.



The purpose is behavioral comparison, not automated anomaly or threat detection.



A measurable difference between normal and controlled abnormal conditions does not, by itself, demonstrate that an automated detection system can identify malicious activity.



\---



\## 2. Research Hypothesis



\### H2 — Behavioral Differentiation



Selected telemetry measurements collected during predefined controlled abnormal test periods will exhibit measurable differences from the same measurements collected during defined normal-operation baseline periods.



\### H02 — Null Hypothesis



Selected telemetry measurements collected during predefined controlled abnormal test periods will not exhibit measurable differences from the same measurements collected during defined normal-operation baseline periods.



\---



\## 3. Experimental Comparison Model



The M5 comparison model is:



\*\*M4 NORMAL BASELINE → CONTROLLED CHANGE → OBSERVATION → COMPARISON\*\*



Two experimental states are defined:



\*\*State A:\*\* M4 defined normal operation  

\*\*State B:\*\* M5 defined controlled abnormal activity



Measurements obtained under State B will be compared with the relevant measurements retained from State A.



Where a direct quantitative comparison is not supported by the available telemetry, the difference will be documented qualitatively and identified as such.



\---



\## 4. Experimental Environment



The existing RC-002 GNS3 environment will be retained.



The environment contains:



\- USER zone

\- SERVER zone

\- OBSERVABILITY zone

\- WAN/Internet connectivity

\- EDGE-R1

\- CLIENT-1

\- CLIENT-2

\- SERVER-1

\- TELEMETRY-1

\- Centralized Syslog collection

\- Packet-level observation capability



The M4 configuration represents the reference configuration for M5.



No topology or telemetry configuration change shall be introduced during an experiment unless that change is explicitly part of the defined experimental condition.



\---



\## 5. Controlled Variables



Where technically possible, the following variables shall remain fixed or be recorded:



\- Network topology

\- Device configurations

\- Routing configuration

\- Zone structure

\- Security-policy configuration

\- Telemetry-source configuration

\- Test endpoints

\- Test duration

\- Test procedure

\- Observation window

\- Time synchronization

\- Background traffic conditions



Any uncontrolled or changed variable shall be documented as an experimental limitation or potential confounding factor.



\---



\## 6. Repetition Strategy



Each quantitative M5 scenario shall be executed across five controlled trials where technically practical.



This follows the repeated-trial structure used for the quantitative M4 baseline experiments.



Each trial shall:



1\. Begin from a known operational state.

2\. Use the predefined procedure for that scenario.

3\. Retain the required evidence.

4\. Record the measured outcome.

5\. Verify relevant legitimate operations where required.

6\. Return the environment to the defined starting condition before the next trial where technically necessary.



If five trials are not technically appropriate for a particular scenario, the reason shall be documented.



\---



\## 7. M5 Experimental Scenarios



\### A01 — Elevated Connection Frequency



\*\*Objective:\*\*  

Determine whether a controlled increase in connection or request frequency produces measurable telemetry characteristics different from the corresponding normal-operation condition.



\*\*Independent Variable:\*\*  

Connection/request frequency.



\*\*Candidate Dependent Variables:\*\*



\- Connection frequency

\- Traffic volume

\- Event frequency

\- Packet behavior

\- Application response behavior

\- Source-attributed telemetry observations



\*\*Reference Condition:\*\*  

Relevant M4 legitimate application/network behavior.



\*\*Method:\*\*  

Generate a predefined series of legitimate connection or application requests from an authorized test endpoint at a frequency greater than the normal reference condition.



The same procedure and request count/rate shall be used for each repeated trial.



\*\*Evidence:\*\*



\- Trial results

\- Packet capture where applicable

\- Centralized telemetry observations

\- Relevant timestamps

\- Operational-preservation observations



\---



\### A02 — Increased Destination or Service Diversity



\*\*Objective:\*\*  

Determine whether controlled changes in destination or service-access patterns produce observable differences from the normal baseline.



\*\*Independent Variable:\*\*  

Number or distribution of destinations/services contacted during the observation period.



\*\*Candidate Dependent Variables:\*\*



\- Destination diversity

\- Port/service distribution

\- Connection frequency

\- Traffic volume

\- Event characteristics



\*\*Reference Condition:\*\*  

M4 normal destination/service behavior.



\*\*Method:\*\*  

Generate a predefined sequence of connections toward multiple approved laboratory destinations or services that differs from the normal reference pattern.



Only services and endpoints explicitly available within the controlled RC-002 laboratory shall be used.



\*\*Evidence:\*\*



\- Trial results

\- Packet-level observations

\- Relevant telemetry

\- Destination/service records

\- Operational-preservation observations



\---



\### A03 — Controlled Denied-Access Activity



\*\*Objective:\*\*  

Determine whether repeated policy-denied activity produces observable telemetry characteristics distinguishable from normal legitimate operation.



\*\*Independent Variable:\*\*  

Frequency of predefined denied-access attempts.



\*\*Candidate Dependent Variables:\*\*



\- Denied-access frequency

\- Connection/event frequency

\- Source attribution

\- Destination/service information

\- Infrastructure or security-policy events where available



\*\*Reference Condition:\*\*  

M4 legitimate operation without the predefined denied-access sequence.



\*\*Method:\*\*  

From an authorized laboratory endpoint, perform a predefined set of connection attempts that are expected to be denied by an existing or explicitly defined laboratory policy.



The policy behavior and expected result shall be documented before execution.



\*\*Evidence:\*\*



\- Denied-access results

\- Relevant packet observations

\- Centralized telemetry where available

\- Source and destination attribution

\- Operational-preservation observations



\---



\### A04 — Controlled Infrastructure-Event Burst



\*\*Objective:\*\*  

Determine whether a controlled increase in infrastructure or system-event frequency produces observable changes in centralized telemetry.



\*\*Independent Variable:\*\*  

Frequency of predefined infrastructure/system events.



\*\*Candidate Dependent Variables:\*\*



\- Infrastructure-event frequency

\- Syslog event frequency

\- Source attribution

\- Event-type distribution

\- Event observability



\*\*Reference Condition:\*\*  

M4 normal system activity.



\*\*Method:\*\*  

Generate a predefined sequence of benign administrative or system events within the controlled laboratory.



Events shall be reproducible, authorized, and designed to avoid unnecessary disruption to the experimental environment.



\*\*Evidence:\*\*



\- Event timestamps

\- Source-specific centralized Syslog

\- Event counts/types

\- Relevant system observations

\- Operational-preservation observations



\---



\## 8. Operational Preservation



M5 shall continue to collect evidence relevant to H3 — Operational Preservation.



Where applicable, experiments shall verify that required legitimate operations remain functional, including:



\- Required network connectivity

\- Routing stability

\- HTTP/application availability

\- Internet/NAT connectivity where applicable

\- Telemetry collection

\- Required management or infrastructure services



An abnormal experimental condition shall not automatically be interpreted as successful if it destroys the functionality required to make the comparison meaningful.



Any material degradation shall be recorded.



\---



\## 9. Measurements



M5 will prioritize measurements supported by the implemented RC-002 environment.



Candidate measurements include:



\- Connection frequency

\- Destination diversity

\- Port/service distribution

\- Denied-access frequency

\- Traffic volume

\- Infrastructure-event frequency

\- Packet behavior

\- Application completion behavior

\- Source-attributed centralized telemetry

\- Event observability latency where timestamp precision permits



Measurements shall not be reported with greater precision than the experimental environment supports.



\---



\## 10. Evidence Retention



Raw and derived M5 evidence shall be retained separately.



Proposed raw evidence location:



`datasets/M5-Controlled-Abnormal-Behavior/`



Proposed derived-results location:



`results/M5-Controlled-Abnormal-Behavior/`



Each experimental scenario shall have a dedicated subdirectory.



Proposed structure:



`datasets/M5-Controlled-Abnormal-Behavior/A01-Connection-Frequency/`



`datasets/M5-Controlled-Abnormal-Behavior/A02-Destination-Service-Diversity/`



`datasets/M5-Controlled-Abnormal-Behavior/A03-Denied-Access/`



`datasets/M5-Controlled-Abnormal-Behavior/A04-Infrastructure-Events/`



Derived analysis shall be retained under corresponding M5 result structures.



Large packet-capture artifacts shall remain subject to the repository's existing evidence-retention and `.gitignore` policy.



\---



\## 11. Comparison with M4



M5 results shall be interpreted against the measured M4 reference condition.



Comparison shall focus on characteristics supported by both experimental states.



Where appropriate, comparison may include:



\- Trial counts

\- Event counts

\- Connection frequency

\- Packet characteristics

\- Traffic volume

\- Response/completion behavior

\- Destination/service diversity

\- Syslog event characteristics

\- Source attribution

\- Variability across repeated trials



A difference shall be described quantitatively where supported by retained measurements and qualitatively where quantitative measurement is not reliable.



\---



\## 12. H2 Evaluation Rule



H2 shall not be accepted or rejected solely through visual inspection.



Evidence from the predefined M5 scenarios shall be compared with the corresponding M4 baseline measurements.



Possible conclusions are:



\- Evidence supports measurable behavioral differentiation for the tested condition.

\- Evidence does not support measurable behavioral differentiation for the tested condition.

\- Evidence is insufficient to evaluate behavioral differentiation for the tested condition.



The overall evaluation of H2 shall consider results across the tested scenarios and the limitations of the implemented environment.



Where the available evidence is insufficient, H2 shall be reported as inconclusive or not testable rather than inferred.



\---



\## 13. Interpretation Boundary



M5 evaluates whether controlled changes in behavior become observably different through the selected telemetry.



M5 does not establish that:



\- A measured deviation is malicious.

\- A single threshold can distinguish normal from abnormal behavior.

\- The environment provides automated anomaly detection.

\- The environment provides automated threat detection.

\- The results generalize directly to production enterprise networks.



M4 demonstrated that legitimate behavior itself can exhibit substantial variability. Therefore, M5 comparisons shall consider multiple observable characteristics where supported rather than treating deviation in a single metric as sufficient evidence of abnormality.



\---



\## 14. Confounding Factors and Limitations



Potential confounding factors include:



\- Background network traffic

\- Clock synchronization differences

\- Telemetry buffering

\- Collection or processing delays

\- Packet loss

\- Device resource constraints

\- GNS3/emulator limitations

\- Host operating-system behavior

\- Differences between repeated trials

\- Telemetry-source configuration changes

\- External-path variability



Particular care shall be taken when interpreting measurements involving the external GNS3 NAT path because M4 demonstrated substantial Internet-facing RTT variability.



\---



\## 15. Pre-Experiment Acceptance Criteria



M5 experimentation may begin only after confirming:



\- M4 baseline evidence is retained.

\- The RC-002 topology is operational.

\- Required routing is functional.

\- TELEMETRY-1 is receiving expected source-attributed telemetry.

\- Required legitimate application/network services are functional.

\- Experimental scenario procedures are defined before execution.

\- Required evidence locations exist.

\- No unintended configuration change has invalidated the M4 reference condition.



\---



\## 16. Milestone Completion Criteria



M5 will be considered complete when:



1\. The predefined controlled scenarios have been executed.

2\. Required repetitions have been completed or deviations justified.

3\. Raw evidence has been retained.

4\. Derived measurements have been documented.

5\. M5 observations have been compared with the relevant M4 baseline.

6\. Operational impact has been documented.

7\. Confounding factors and limitations have been recorded.

8\. H2 has been evaluated only to the extent supported by retained evidence.

9\. A formal M5 milestone verification document has been produced.



\---



\## 17. Research Integrity Rule



Experimental procedures shall be defined before their results are interpreted.



Results that do not support H2 shall be retained and reported.



Unexpected observations shall not be removed solely because they conflict with the expected outcome.



Experimental parameters shall not be retrospectively changed without documenting the change and its justification.



\---



\## 18. Current Status



\*\*M5 protocol defined prior to controlled abnormal-behavior experimentation.\*\*



Next stage: validate the pre-experiment environment and prepare A01 — Elevated Connection Frequency.



\---



\*\*Project Aegis\*\*  

\*Securing Tomorrow's Digital Infrastructure Through Research\*

