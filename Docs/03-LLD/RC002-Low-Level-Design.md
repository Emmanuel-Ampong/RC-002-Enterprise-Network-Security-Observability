\# RC-002 - Low-Level Design



\## Enterprise Network Security Observability and Telemetry



\*\*Project:\*\* Project Aegis

\*\*Research Cycle:\*\* RC-002

\*\*Project Aegis Phase:\*\* OBSERVE

\*\*Document Type:\*\* Low-Level Design

\*\*Status:\*\* Implemented and retrospectively documented from the verified M1-M4 environment



\---



\## 1. Purpose



This document defines the low-level implementation of the RC-002 Enterprise Network Security Observability and Telemetry laboratory.



It complements the High-Level Design by documenting the implemented topology, addressing, routing, services, telemetry paths, persistence mechanisms, and experimental observation points used during RC-002 M1-M4.



This document describes the environment that was actually implemented and verified. It does not imply that the LLD preceded implementation.



\---



\## 2. Laboratory Platform



RC-002 is implemented as a virtual network-security research laboratory using GNS3.



The verified laboratory environment includes:



\- GNS3 Desktop 2.2.61.

\- GNS3 VM 2.2.61.

\- VirtualBox 7.2.4.

\- GNS3 VM configured with 1 vCPU and 2048 MB RAM.

\- FRRouting 8.2.2 for EDGE-R1.

\- VPCS for lightweight client endpoints.

\- Alpine Linux 3.18.4 for SERVER-1.

\- Ubuntu Cloud Guest 24.04 LTS for TELEMETRY-1.



The design intentionally uses lightweight components to support reproducible experimentation within the available host resources.



\---



\## 3. Implemented Topology



The implemented minimum observability topology is:



```text

&#x20;                        NAT2 / WAN

&#x20;                     192.168.42.0/24

&#x20;                             |

&#x20;                          EDGE-R1

&#x20;                             |

&#x20;            +----------------+----------------+

&#x20;            |                |                |

&#x20;       ACCESS-SW1        SERVER-SW1         OBS-SW1

&#x20;         /    \\               |                |

&#x20;    CLIENT-1 CLIENT-2      SERVER-1       TELEMETRY-1



The topology separates user, server, observability, and external-connectivity functions while permitting routed communication required by the experimental methodology.



\## 4. Network Zones

Zone	Network	Devices / Function

USER	192.168.10.0/24	CLIENT-1, CLIENT-2, ACCESS-SW1

SERVER	192.168.20.0/24	SERVER-1, SERVER-SW1

OBSERVABILITY	192.168.30.0/24	TELEMETRY-1, OBS-SW1

WAN / INTERNET	192.168.42.0/24	EDGE-R1 WAN interface and GNS3 NAT2



\## 5. Device Addressing



\### 5.1 EDGE-R1



EDGE-R1 provides Layer-3 connectivity between the internal zones and the external GNS3 NAT environment.



Interface	Addressing	Function

eth0	192.168.10.1/24	USER gateway

eth1	192.168.20.1/24	SERVER gateway

eth2	192.168.30.1/24	OBSERVABILITY gateway

eth3	DHCP on 192.168.42.0/24	WAN / NAT uplink



During verification, eth3 received:



192.168.42.184/24



The verified WAN default route used:



192.168.42.1



Because eth3 is DHCP-assigned, 192.168.42.184/24 represents the address observed during verification and should not be interpreted as a permanently assigned static address.



\### 5.2 Endpoints

Device	Address	Default Gateway	Role

CLIENT-1	192.168.10.10/24	192.168.10.1	USER traffic-generation endpoint

CLIENT-2	192.168.10.11/24	192.168.10.1	USER traffic-generation endpoint

SERVER-1	192.168.20.10/24	192.168.20.1	Application server and telemetry source

TELEMETRY-1	192.168.30.10/24	192.168.30.1	Central telemetry collector and observation host



\## 6. Routing Design



EDGE-R1 performs routing between:



USER and SERVER.

USER and OBSERVABILITY.

SERVER and OBSERVABILITY.

Internal zones and the WAN/Internet path.



IPv4 forwarding is enabled on EDGE-R1.



The internal networks are directly connected to EDGE-R1:



192.168.10.0/24 -> eth0

192.168.20.0/24 -> eth1

192.168.30.0/24 -> eth2



The external interface is:



eth3 -> DHCP on 192.168.42.0/24



The verified default route is provided through the GNS3 NAT service using:



192.168.42.1



\## 7. WAN and NAT Design



External connectivity is provided through the GNS3 NAT2 node.



EDGE-R1 uses eth3 as the WAN-facing interface.



Source NAT is implemented using an iptables POSTROUTING MASQUERADE rule:



\-A POSTROUTING -o eth3 -j MASQUERADE



This permits systems on the internal RC-002 networks to reach external destinations through EDGE-R1.



NAT functionality was verified through external connectivity tests and observation of the POSTROUTING MASQUERADE counter.



\## 8. WAN and NAT Persistence



During M3 persistence testing, the initial EDGE-R1 configuration revealed that:



Internal interface configuration persisted.

Syslog forwarding persisted.

eth3 DHCP configuration did not initially recover automatically after restart.

The manually configured MASQUERADE rule did not initially persist.



These findings resulted in persistence corrections.



\### 8.1 eth3 DHCP Persistence



The persistent WAN configuration includes:



auto eth3

iface eth3 inet dhcp



The networking service is enabled at boot.



\### 8.2 iptables Persistence



The NAT configuration is retained using the appliance's native iptables persistence mechanism.



The saved rules include:



\-A POSTROUTING -o eth3 -j MASQUERADE



The saved rules are retained under:



/etc/iptables/rules-save



The OpenRC iptables service is enabled at boot.



Following the persistence corrections, reboot verification confirmed automatic restoration of:



eth3 DHCP addressing.

WAN default routing.

NAT MASQUERADE.

Syslog forwarding.

External connectivity.



\## 9. SERVER-1 Design



SERVER-1 is an Alpine Linux 3.18.4 system located in the SERVER zone.



Its network configuration is:



IP address:      192.168.20.10/24

Default gateway: 192.168.20.1

DNS:             8.8.8.8



SERVER-1 provides two principal functions within RC-002:



Reproducible HTTP application service.

Source of centralized host telemetry.



\## 10. HTTP Application Service



SERVER-1 runs a lightweight Lighttpd HTTP service.



The service is reachable at:



http://192.168.20.10/



The web service provides a controlled application endpoint for connectivity and traffic-characterization experiments.



The deployed test page identifies the service as part of Project Aegis RC-002 and provides a repeatable HTTP target for observability experiments.



Lighttpd is configured to start automatically with the server.



Cross-zone HTTP connectivity from TELEMETRY-1 to SERVER-1 was verified during M2 and subsequently used during the M4 normal-operation baseline.



\## 11. TELEMETRY-1 Design



TELEMETRY-1 is an Ubuntu Cloud Guest 24.04 LTS system located in the OBSERVABILITY zone.



Its network configuration is:



IP address:      192.168.30.10/24

Default gateway: 192.168.30.1



TELEMETRY-1 uses a static network configuration.



Its principal functions are:



Centralized Syslog collection.

Preservation of source-attributed telemetry.

Packet-level observation where required.

Execution of selected experimental traffic and measurement procedures.

Storage and inspection of collected experimental evidence.



\## 12. Centralized Syslog Architecture



TELEMETRY-1 operates as the centralized Syslog collector using rsyslog.



The collector accepts remote Syslog traffic on port 514.



Both UDP and TCP reception were enabled and verified during M3.



The implemented collector configuration uses source-based dynamic file storage.



The relevant configuration is:



module(load="imudp")

input(type="imudp" port="514")



module(load="imtcp")

input(type="imtcp" port="514")



template(

&#x20;   name="RC002RemoteLog"

&#x20;   type="string"

&#x20;   string="/var/log/remote/%FROMHOST-IP%/syslog.log"

)



if ($inputname == "imudp" or $inputname == "imtcp") then {

&#x20;   action(

&#x20;       type="omfile"

&#x20;       dynaFile="RC002RemoteLog"

&#x20;       createDirs="on"

&#x20;   )

&#x20;   stop

}



The configuration was syntax-validated using:



rsyslogd -N1



The rsyslog service was subsequently verified as active and listening on the configured Syslog ports.



\## 13. Telemetry Sources



\### 13.1 SERVER-1



SERVER-1 forwards selected system telemetry to:



192.168.30.10:514



Its remote telemetry is stored on TELEMETRY-1 under:



/var/log/remote/192.168.20.10/syslog.log



The forwarding configuration was verified to persist after restart.



\### 13.2 EDGE-R1



EDGE-R1 forwards infrastructure telemetry to:



192.168.30.10:514



Its remote telemetry is stored on TELEMETRY-1 under:



/var/log/remote/192.168.30.1/syslog.log



During M3, the collector observed the router telemetry source address as 192.168.30.1.



The forwarding configuration was verified to persist after restart.



\### 13.3 TELEMETRY-1



TELEMETRY-1 also produces local host telemetry.



This permits observation of the collector itself while retaining remotely received events from SERVER-1 and EDGE-R1.



\## 14. Telemetry Flow



The principal centralized telemetry paths are:



SERVER-1

192.168.20.10

&#x20;     |

&#x20;     | Syslog

&#x20;     v

EDGE-R1 routing

&#x20;     |

&#x20;     v

TELEMETRY-1

192.168.30.10:514

&#x20;     |

&#x20;     v

/var/log/remote/192.168.20.10/syslog.log



and:



EDGE-R1

192.168.30.1

&#x20;     |

&#x20;     | Syslog

&#x20;     v

TELEMETRY-1

192.168.30.10:514

&#x20;     |

&#x20;     v

/var/log/remote/192.168.30.1/syslog.log



Source-specific storage supports attribution of received events to the observed source IP.



\## 15. Packet-Level Observation



Packet capture is used alongside centralized Syslog to provide network-level evidence.



Wireshark and/or tcpdump are used where required by the experimental procedure.



Packet-level observations may include:



Protocol.

Source address.

Destination address.

Source and destination ports where applicable.

Packet count.

Packet size where useful.

Connection behavior.

Timestamps.

Trial duration.

Packet-capture drop information where available.



Packet capture and centralized Syslog are treated as complementary evidence sources.



\## 16. M4 Normal Baseline Integration



The implemented architecture was used to establish the M4 normal-operation reference condition.



The controlled scenarios were:



ID	Scenario	Source	Destination	Observation

N01	Inter-zone ICMP	SERVER-1 (192.168.20.10)	TELEMETRY-1 (192.168.30.10)	ICMP packet capture and RTT

N02	HTTP application access	TELEMETRY-1 (192.168.30.10)	SERVER-1 (192.168.20.10:80)	TCP/HTTP behavior and completion

N03	Internet ICMP	SERVER-1 (192.168.20.10)	8.8.8.8 through EDGE-R1/NAT	ICMP connectivity and RTT

N04	Normal system activity	SERVER-1 and EDGE-R1	TELEMETRY-1	Centralized Syslog observations



N01-N03 were executed using five controlled trials each.



These observations form the normal reference condition for subsequent controlled abnormal-behavior experiments.



\## 17. Operational Preservation



The observability architecture is required to preserve the legitimate network operations necessary for the experiment.



Verified operations include:



Internal routed connectivity.

Cross-zone connectivity.

SERVER-1 HTTP service access.

Centralized telemetry forwarding.

External connectivity through EDGE-R1 and GNS3 NAT.

Persistence of required router functions after restart.



Successful operation of these functions provides evidence relevant to RC-002 Hypothesis H3 within the tested scope.



It does not imply that the observability architecture has zero performance impact under all conditions.



\## 18. Experimental Control Considerations



To support reproducibility, experiments should preserve the following variables unless the experimental protocol explicitly changes them:



Network topology.

Addressing.

Routing configuration.

NAT configuration.

Telemetry configuration.

Endpoint roles.

Application-service configuration.

Experimental duration.

Trial procedure.

Observation window.

Time synchronization where practical.

Background traffic conditions where controllable.



Potential confounding factors include:



Background traffic.

Clock synchronization.

Telemetry buffering.

Processing delay.

Packet loss.

Host resource constraints.

Emulator limitations.

Operating-system behavior.

Repeated-run variation.

Changes to telemetry configuration.



\## 19. Evidence and Repository Structure



Implementation and experimental evidence is retained within the RC-002 repository.



Relevant locations include:



Docs/06-Verification/

datasets/

results/

Images/

Experiments/

Configurations/



Raw packet-capture files are excluded from Git tracking where specified by .gitignore.



Structured measurements, text evidence, verification documents, and selected screenshots are retained where appropriate for reproducibility and review.



\## 20. Relationship to the High-Level Design



The architectural intent of RC-002 is documented in:



Docs/02-HLD/RC002-High-Level-Design.md



This LLD provides the implementation-specific detail required to understand and reproduce the verified RC-002 M1-M4 environment.



\## 21. Design Limitations



The implemented environment is a controlled laboratory rather than a production enterprise deployment.



Current limitations include:



Limited number of endpoints.

Limited number of telemetry sources.

GNS3/emulated infrastructure behavior.

External connectivity dependent on the GNS3 NAT environment.

Lightweight centralized logging rather than a full SIEM platform.

Limited trial counts during baseline characterization.

Host resource constraints.

No claim of comprehensive enterprise-wide observability.

No claim of automated anomaly or threat detection at the M1-M4 stage.



These limitations define the scope within which RC-002 findings should be interpreted.



\## 22. Documentation Note



This LLD was completed retrospectively after implementation and verification of the RC-002 M1-M4 environment.



The document consolidates implementation details already established and tested during those milestones.



It should therefore be interpreted as a technical record of the implemented laboratory rather than evidence that all low-level design decisions were formally documented before implementation.



Subsequent implementation changes should be reflected in this document, the Engineering Log, relevant ADRs, and milestone verification records.
