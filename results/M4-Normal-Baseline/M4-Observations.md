# RC-002 M4 — Normal Traffic Baseline Observations

## Purpose

M4 characterizes observable behavior produced by legitimate operations in the RC-002 environment. The purpose of this baseline is to establish a documented normal reference condition for later comparison with controlled abnormal-behavior experiments.

M4 does not test whether abnormal behavior can be distinguished from normal behavior. It establishes the normal reference required for that later comparison.

## N01 — Normal Inter-Zone ICMP

SERVER-1 (192.168.20.10) generated ICMP traffic to TELEMETRY-1 (192.168.30.10) across the routed internal network.

Five controlled trials were performed. Every trial completed with zero packet loss. Each packet capture contained six ICMP packets, corresponding to three echo requests and three echo replies, with zero kernel drops.

The mean of the five trial-average RTT values was 3.388 ms. Trial-average RTT values ranged from 2.664 ms to 5.450 ms.

### Observation

Legitimate inter-zone ICMP communication produced a consistent and reproducible packet pattern with low latency and no observed packet loss during the experiment.

## N02 — Normal HTTP Application Access

TELEMETRY-1 (192.168.30.10) accessed the HTTP service hosted by SERVER-1 (192.168.20.10).

Five controlled trials were performed. All trials returned HTTP status 200. Each capture contained 10 TCP packets, and the expected client SYN and server SYN-ACK were observed in every trial. No kernel drops were recorded.

Mean HTTP completion time was 0.0097632 seconds, with observed trial values ranging from 0.007903 to 0.011616 seconds.

### Observation

Normal application access produced a repeatable TCP/HTTP interaction pattern and successful application responses across all five trials.

## N03 — Normal Internet ICMP

SERVER-1 generated ICMP traffic to 8.8.8.8 through EDGE-R1 and the GNS3 NAT environment.

Five controlled trials were performed. All trials completed with three requests, three replies, and zero packet loss.

The mean of the five trial-average RTT values was 116.3288 ms. Trial-average RTT values ranged from 55.676 ms to 277.110 ms.

### Observation

Internet ICMP traffic exhibited substantially greater latency variability than internal inter-zone ICMP while still maintaining successful packet delivery in every trial.

This demonstrates that legitimate behavior in the RC-002 environment is not necessarily low-variance behavior. Consequently, elevated latency alone should not be treated as sufficient evidence of abnormal activity in later experiments.

## N04 — Normal System Activity

Centralized Syslog records from SERVER-1 (192.168.20.10) and EDGE-R1 (192.168.30.1) were examined.

Controlled normal events were successfully observed from both sources. Routine cron activity was also observed from both systems, and source attribution was preserved by the centralized telemetry architecture.

### Observation

Normal host and infrastructure activity produces source-attributable centralized telemetry that can be used as contextual evidence alongside packet-level observations.

## Cross-Scenario Baseline Characterization

The M4 experiments establish several characteristics of legitimate RC-002 behavior:

1. Internal ICMP communication was reliable and comparatively low-latency during the measured trials.
2. Normal HTTP application access produced repeatable TCP behavior and successful HTTP responses.
3. Internet ICMP communication remained reliable but showed substantial RTT variability.
4. Centralized Syslog preserved source attribution for controlled and routine system events.
5. Packet-level evidence and centralized logs provide complementary views of legitimate network behavior.

## Research Interpretation

The results establish an empirical reference condition for normal operation within the tested RC-002 environment.

The measurements also show why a future abnormal-behavior assessment should not rely on a single feature or threshold. In particular, the N03 results demonstrate that substantial latency variation can occur during successful legitimate communication.

M4 therefore provides the normal reference against which later controlled abnormal conditions can be compared.

## Hypothesis Relationship

### H1

M4 provides additional evidence that the implemented observability architecture can collect useful, source-attributable information from selected RC-002 components. It does not establish comprehensive observability of every network source.

### H2

H2 is **not tested in M4**.

M4 establishes the normal baseline required for a subsequent controlled comparison between normal and abnormal behavior.

### H3

All tested legitimate operations remained functional during M4. This provides additional evidence consistent with H3 within the scope of the tested scenarios, but it does not establish that the observability architecture has zero performance or operational impact.

## Limitations

The baseline is limited to the RC-002 laboratory environment and the specific scenarios tested. Five trials were used for N01, N02, and N03, which is sufficient for the controlled laboratory baseline used here but not for broad statistical generalization.

N03 measurements depend partly on external network conditions and the GNS3 NAT path, so Internet RTT values may vary independently of activity inside the RC-002 topology.

The N04 assessment is primarily categorical rather than quantitative.

No claim is made that the baseline represents all legitimate enterprise behavior or that deviations from these measurements necessarily indicate malicious activity.
