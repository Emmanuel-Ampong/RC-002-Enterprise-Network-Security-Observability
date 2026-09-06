# ADR-001: Laboratory Platform Selection for RC-002

## Status

Accepted

---

## Context

RC-002 investigates enterprise network security observability and
telemetry.

The project requires an experimental environment capable of generating,
collecting, preserving, and analyzing selected telemetry from network
devices and supporting systems.

RC-001 used Cisco Packet Tracer as its primary implementation and
validation environment. Packet Tracer was suitable for evaluating
enterprise network architecture, routing, segmentation, access control,
and basic infrastructure services.

RC-002 introduces additional requirements including centralized event
collection, packet-level evidence, integration with Linux-based systems,
and controlled telemetry experiments.

The laboratory platform must therefore support a higher degree of
experimental realism while remaining compatible with available local
computing resources.

The available laboratory system includes:

- AMD Ryzen 5 4500U processor
- 8 GB RAM
- Approximately 68 GB available storage
- Windows 11 host operating system

---

## Decision Drivers

The laboratory platform was evaluated according to:

1. Support for telemetry generation and collection
2. Support for Linux-based systems
3. Packet capture capability
4. Experimental reproducibility
5. Compatibility with the RC-002 requirements
6. Resource requirements
7. Implementation complexity

---

## Considered Options

### Option A — Continue Using Packet Tracer as the Primary Platform

Packet Tracer provides a lightweight environment and remains useful for
conceptual network development.

However, it provides limited support for real telemetry pipelines,
Linux-based telemetry systems, and packet-level experimentation.

**Decision:** Not selected as the primary RC-002 experimental platform.

---

### Option B — GNS3 Network Emulation

GNS3 provides a more flexible environment for integrating network
devices, Linux systems, and packet capture.

However, network emulation alone does not define the centralized
telemetry architecture required by RC-002.

**Decision:** Used as a foundation but not as the complete laboratory
architecture.

---

### Option C — GNS3 with a Lightweight Linux Telemetry Node

This approach combines network emulation with a lightweight centralized
telemetry system.

The telemetry node can support:

- Centralized Syslog collection
- Experimental event storage
- Packet and traffic evidence
- Basic telemetry analysis

This architecture provides increased experimental realism while
remaining compatible with limited local computing resources.

**Decision:** Selected.

---

### Option D — Full Multi-Tool Security Laboratory

A large environment containing multiple monitoring, detection, search,
storage, and visualization platforms was considered.

Although this could provide significant functionality, it would require
substantially greater memory, storage, and implementation complexity.

It would also introduce unnecessary variables into the initial RC-002
experiments.

**Decision:** Deferred.

---

## Decision

RC-002 will use **GNS3 as the primary network experimentation
environment**, supported by a **lightweight Linux telemetry node**.

Cisco Packet Tracer may continue to be used as a conceptual reference
tool where appropriate, but it will not serve as the primary
experimental observability platform.

The initial implementation will prioritize:

1. Network emulation
2. Centralized Syslog collection
3. Controlled packet capture
4. Lightweight event storage
5. Structured experimental evidence

Additional technologies will only be introduced when justified by
experimental requirements.

---

## Consequences

### Positive Consequences

- Increased experimental realism
- Support for Linux-based telemetry systems
- Improved packet-level evidence
- Greater flexibility for controlled experiments
- Expandable laboratory architecture

### Negative Consequences

- Increased configuration complexity
- Greater CPU and memory requirements
- Requirement for compatible network-device images
- Need for careful resource management

---

## Relationship to Project Aegis

RC-001 established the enterprise infrastructure foundation of Project
Aegis.

RC-002 extends that foundation into the **OBSERVE** phase by introducing
centralized telemetry and observability experimentation.

---

## Review

This decision may be revisited if:

- RC-002 requirements change significantly.
- Available computing resources increase.
- The selected environment cannot support required telemetry
  experiments.
- A lighter or more reproducible alternative is identified.

