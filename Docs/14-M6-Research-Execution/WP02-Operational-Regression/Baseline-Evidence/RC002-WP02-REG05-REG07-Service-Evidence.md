# RC-002 — WP-02 Baseline Service Evidence

**Project:** Project Aegis  
**Milestone:** M6 — Research Execution  
**Work Package:** WP-02 — Operational Regression Verification  
**Test IDs:** REG-05 through REG-07  
**Execution Date:** 13 October 2026  
**Environment:** GNS3  
**Evidence Type:** Transcript-derived test records  

## 1. Evidence Provenance

These records were reconstructed from console outputs shared during
the WP-02 operational regression baseline execution.

The observations are based on actual command outputs, but this document
is not an independently exported GNS3 console capture.

No configuration changes were introduced during these baseline tests.

## 2. REG-05 — HTTP Service Accessibility

**Source:** TELEMETRY-1 (192.168.30.10)  
**Destination:** SERVER-1 (192.168.20.10)  
**Command:** `curl -i --max-time 10 http://192.168.20.10/`  
**Result:** PASS  

### 2.1 Observed HTTP Response

The command returned the following HTTP headers:

```text
HTTP/1.1 200 OK
Content-Type: text/html
ETag: "4169854954"
Last-Modified: Thu, 10 Sep 2026 14:10:24 GMT
Content-Length: 361
Accept-Ranges: bytes
Date: Tue, 13 Oct 2026 09:39:41 GMT
Server: lighttpd/1.4.85

2.2 Assessment

The HTTP 200 OK response and expected application content confirm
successful HTTP accessibility during the test.
Observation: The final HTML paragraph contained an invalid
closing sequence (<>). This did not prevent the HTTP request from
succeeding.
Limitation: One successful HTTP request does not demonstrate
continuous service availability.


## 3. REG-06 — External IPv4 Connectivity
Source: SERVER-1 (192.168.20.10)
Destination: 8.8.8.8
Command: ping -c 5 8.8.8.8
Result: PASS

3.1 Observed ICMP Output

PING 8.8.8.8 (8.8.8.8): 56 data bytes
64 bytes from 8.8.8.8: seq=0 ttl=253 time=43.644 ms
64 bytes from 8.8.8.8: seq=1 ttl=253 time=43.941 ms
64 bytes from 8.8.8.8: seq=2 ttl=253 time=47.188 ms
64 bytes from 8.8.8.8: seq=3 ttl=253 time=46.129 ms
64 bytes from 8.8.8.8: seq=4 ttl=253 time=52.521 ms

--- 8.8.8.8 ping statistics ---
5 packets transmitted, 5 packets received, 0% packet loss
round-trip min/avg/max = 43.644/46.684/52.521 ms


3.2 Assessment

Five of five ICMP probes received replies, with no observed packet loss.
The results confirm external IPv4 reachability during execution.
Limitation: The test does not independently demonstrate continuous
internet availability or prove which NAT translations were applied.

## 4. REG-07 — External DNS Resolution
Source: SERVER-1 (192.168.20.10)
DNS Server: 8.8.8.8
Command: nslookup example.com
Result: PASS

4.1 Observed DNS Output

Server:         8.8.8.8
Address:        8.8.8.8:53

Non-authoritative answer:
Name:   example.com
Address: 2606:4700:10::ac42:93f3
Name:   example.com
Address: 2606:4700:10::6814:179a

Non-authoritative answer:
Name:   example.com
Address: 104.20.23.154
Name:   example.com
Address: 172.66.147.243


4.2 Assessment
The configured DNS server successfully returned two IPv4 and two IPv6
addresses for the requested hostname.
Limitation: Successful DNS resolution does not independently
demonstrate IPv6 connectivity or continuous DNS availability.

## 5. Overall Evidence Assessment
Test ID	Verification	Result
REG-05	HTTP service accessibility	PASS
REG-06	External IPv4 connectivity	PASS
REG-07	External DNS resolution	PASS
All three tests passed during the baseline execution.
Together, they provide point-in-time evidence that the HTTP service,
external IPv4 connectivity, and DNS resolution were operational.

## 6. Evidence Limitations
- Records were reconstructed from previously shared console outputs.
- Original GNS3 console capture files were not separately archived.
- No sustained service availability measurements were performed.
- No post-change comparison has yet been completed.
- Successful baseline results do not establish zero operational impact.