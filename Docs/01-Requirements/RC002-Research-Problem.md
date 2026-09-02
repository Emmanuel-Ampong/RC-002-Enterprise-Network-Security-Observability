# RC-002 Research Problem

## Project

**RC-002 — Enterprise Network Security Observability and Telemetry**

## Project Aegis Phase

**OBSERVE**

## Background

RC-001 established a segmented, multi-site enterprise network connecting Headquarters, Accra, and Takoradi. The environment incorporated dynamic routing, VLAN segmentation, secure management, access-control policies, centralized infrastructure services, and experimental validation.

While these controls improve the security and operational structure of the network, their presence does not automatically provide comprehensive visibility into the behavior occurring across the infrastructure.

A network may remain operational while administrators have limited centralized awareness of abnormal communication patterns, repeated unauthorized access attempts, routing events, service activity, or other security-relevant behavior.

This creates a need to investigate how network telemetry can provide measurable visibility into enterprise network activity.

## Problem Statement

Modern enterprise networks generate operational and security-relevant activity across network devices, hosts, services, and security controls.

Although mechanisms such as segmentation, access control, secure administration, and dynamic routing can restrict or influence network behavior, their effectiveness is limited when administrators cannot systematically observe and analyze events occurring across distributed infrastructure.

A centralized observability architecture may improve this visibility by collecting and organizing telemetry from multiple network sources. However, its effectiveness should be evaluated experimentally rather than assumed.

RC-002 therefore investigates whether centralized network telemetry can improve the observability of network behavior within a segmented multi-site enterprise environment while preserving legitimate network operations.

## Primary Research Question

**RQ-RC002**

> To what extent can centralized network telemetry improve the observability of normal and abnormal behavior within a segmented multi-site enterprise network while preserving legitimate network operations?

## Supporting Research Questions

### RQ1 — Visibility

What operational and security-relevant behaviors can be observed using the selected telemetry sources?

### RQ2 — Centralization

Can telemetry generated across multiple network segments and sites be reliably centralized for analysis?

### RQ3 — Behavioral Differentiation

Can collected telemetry provide measurable differences between established normal activity and deliberately introduced abnormal test activity?

### RQ4 — Operational Impact

What observable limitations or operational effects result from introducing the telemetry and observability architecture?

## Research Boundary

RC-002 focuses primarily on **OBSERVATION**.

The project may generate controlled abnormal activity to evaluate telemetry visibility, but it does not attempt to develop a complete autonomous detection, investigation, or response platform.

These capabilities are reserved for subsequent stages of Project Aegis:

**OBSERVE → DETECT → INVESTIGATE → RESPOND → ADAPT**