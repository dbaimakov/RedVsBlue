# Engagement Summary

## Scope

This document summarizes the security engagement represented in the RedVsBlue repository. The exercise combines offensive validation, defensive log analysis, and mitigation planning in a controlled environment.

## Environment

| Host | Address | Role |
| --- | --- | --- |
| Kali | `192.168.1.90` | Assessment workstation |
| Capstone | `192.168.1.105` | Target Linux host |
| ELK | `192.168.1.100` | Logging and analytics |
| Azure Hyper-V Host | `192.168.1.1` | Virtualization host |

## Assessment Narrative

The engagement began with network reconnaissance and service discovery to identify exposed services and likely attack paths. That initial review highlighted externally reachable services, hidden content paths, and authentication surfaces worth deeper inspection.

Follow-on activity focused on validating practical access risk. The assessment found that hidden content and authentication weaknesses materially changed the attack surface. Those weaknesses, combined with risky service exposure such as WebDAV write capability, created an avenue for deeper access inside the lab environment.

The blue-team component used ELK / Kibana artifacts to map activity back to observable evidence. This included identifying scan behavior, request spikes to sensitive directories, authentication abuse patterns, and file-access activity associated with high-risk web paths.

## High-Level Findings

### 1. Service exposure created a broad discovery surface
Service enumeration quickly revealed enough information to guide follow-on analysis. This reinforces the importance of minimizing exposed services and routinely validating external attack surface.

### 2. Hidden content and access control gaps increased risk
Sensitive or restricted paths created opportunities for deeper traversal. Even when content is not directly exposed on the primary application path, discoverable hidden directories can become a major weakness when monitoring and access controls are weak.

### 3. Credential attacks were detectable and preventable
Repeated authentication failures and request velocity created strong signals for brute-force detection. Better credential policy, lockouts, and rate limiting would materially reduce this risk.

### 4. WebDAV write capability represented a critical control gap
A write-enabled directory increased the likelihood of malicious upload activity. This type of exposure should be tightly constrained, monitored, or removed when not required.

## Defensive Priorities

### Alerting
- scan-volume and connection-rate anomalies
- repeated failed authentication attempts
- access to hidden or restricted content
- HTTP PUT or upload activity in sensitive locations
- suspicious shell-like callbacks or unusual outbound connections

### Hardening
- remove or restrict unnecessary services
- enforce IP allowlisting where operationally possible
- strengthen password requirements and rotation policies
- apply account lockout and timeout controls
- restrict or disable risky WebDAV methods
- filter uploaded content and reduce writable surfaces

## Suggested Reviewer Path

For the best reading order inside the repository:

1. Start with the [README](../README.md)
2. Review [Redteam.pptx](../Redteam.pptx)
3. Open [Log Analysis.pptx](../Log%20Analysis.pptx)
4. Finish with [Mitigation.pptx](../Mitigation.pptx)

## Usage Note

This repository documents assessment work performed in an isolated and authorized lab environment. It is intended for security learning, defensive design, and portfolio presentation.
