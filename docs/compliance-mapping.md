# Compliance Mapping

**Project:** AWS Cloud Security Risk Assessment & GRC Simulation  
**Author:** Tushar Sharma  
**Frameworks:** ISO/IEC 27001:2022 | NIST SP 800-53 Rev. 5  

---

## Overview

Each risk identified in this assessment has been mapped to relevant controls from two widely adopted information security frameworks:

- **ISO/IEC 27001:2022** — Annex A Controls
- **NIST SP 800-53 Rev. 5** — Security and Privacy Control Families

This mapping demonstrates how technical misconfigurations translate into governance and compliance gaps, and supports prioritisation of remediation efforts.

---

## Mapping Table

| Risk ID | Risk Title | ISO 27001 Controls | NIST Control Families |
|---------|-----------|-------------------|----------------------|
| R1 | Public S3 Bucket Exposure | A.9, A.13 | AC, SC |
| R2 | Over-permissive IAM Policy (`*:*`) | A.9 | AC |
| R3 | EC2 Exposed to Internet (SSH/HTTP) | A.13 | SC |
| R4 | No MFA for IAM Users | A.9 | IA |
| R5 | Weak CloudTrail Configuration | A.12 | AU |

---

## Detailed Control Mapping

---

### R1 — Public S3 Bucket Exposure

**ISO 27001 Controls:**

| Control | Title | Relevance |
|---------|-------|-----------|
| A.9.1 | Access Control Policy | Absence of a policy restricting data access to authorised users only |
| A.9.2 | Access to Networks and Network Services | Unrestricted public access to stored data via network (object URL) |
| A.9.4 | Access to System and Application Information | No authentication required to access sensitive stored files |
| A.13.1 | Network Security Management | Lack of controls to prevent unauthorised network-level data access |

**NIST Controls:**

| Control Family | ID | Title | Relevance |
|----------------|-----|-------|-----------|
| Access Control | AC-3 | Access Enforcement | No enforcement of access restrictions on stored objects |
| Access Control | AC-6 | Least Privilege | Bucket policy grants read access to `*` (all principals) |
| System & Comms Protection | SC-7 | Boundary Protection | No boundary enforced between public internet and data store |
| System & Comms Protection | SC-28 | Protection of Information at Rest | Data exposed without encryption or access control |

---

### R2 — Over-Permissive IAM Policy (`*:*`)

**ISO 27001 Controls:**

| Control | Title | Relevance |
|---------|-------|-----------|
| A.9.1 | Access Control Policy | Policy violates documented access control requirements |
| A.9.2 | User Registration and De-provisioning | Excessive permissions granted outside of role-based provisioning |
| A.9.4 | Use of Privileged Utility Programmes | Dev user granted equivalent of admin-level privilege |

**NIST Controls:**

| Control Family | ID | Title | Relevance |
|----------------|-----|-------|-----------|
| Access Control | AC-2 | Account Management | No governance of account access level based on job function |
| Access Control | AC-3 | Access Enforcement | Policy fails to enforce access restrictions |
| Access Control | AC-6 | Least Privilege | `*:*` policy is a direct violation of least privilege |
| Access Control | AC-6(1) | Least Privilege — Authorise Access to Security Functions | Unrestricted access to all security-related API operations |

---

### R3 — EC2 Exposed to Internet (SSH/HTTP)

**ISO 27001 Controls:**

| Control | Title | Relevance |
|---------|-------|-----------|
| A.13.1 | Network Security Management | No controls limiting inbound network access to compute resources |
| A.13.2 | Transfer Policies and Procedures | Unencrypted HTTP traffic allowed without policy justification |

**NIST Controls:**

| Control Family | ID | Title | Relevance |
|----------------|-----|-------|-----------|
| System & Comms Protection | SC-7 | Boundary Protection | Security group permits inbound traffic from any IP (`0.0.0.0/0`) |
| System & Comms Protection | SC-7(5) | Deny by Default | Default-deny not applied; all internet traffic permitted on SSH/HTTP |
| System & Comms Protection | SC-8 | Transmission Confidentiality and Integrity | HTTP (port 80) transmits data without encryption |

---

### R4 — No MFA for IAM Users

**ISO 27001 Controls:**

| Control | Title | Relevance |
|---------|-------|-----------|
| A.9.3 | Use of Secret Authentication Information | Password-only authentication increases risk of credential compromise |
| A.9.4 | Password Management System | No compensating control (MFA) in place for authentication |

**NIST Controls:**

| Control Family | ID | Title | Relevance |
|----------------|-----|-------|-----------|
| Identification & Authentication | IA-2 | Identification and Authentication (Organisational Users) | Users authenticated with single factor only |
| Identification & Authentication | IA-2(1) | MFA for Privileged Accounts | MFA not enforced for admin-user or dev-user |
| Identification & Authentication | IA-5 | Authenticator Management | No secondary authenticator (MFA token) configured |

---

### R5 — Weak CloudTrail Configuration

**ISO 27001 Controls:**

| Control | Title | Relevance |
|---------|-------|-----------|
| A.12.1 | Operational Procedures and Responsibilities | No monitoring integration to support operational visibility |
| A.12.4 | Logging and Monitoring | Log validation disabled; CloudWatch not integrated |
| A.12.7 | Information Systems Audit Considerations | Audit trail integrity cannot be guaranteed without log validation |

**NIST Controls:**

| Control Family | ID | Title | Relevance |
|----------------|-----|-------|-----------|
| Audit & Accountability | AU-2 | Event Logging | Data event logging not configured; only management events captured |
| Audit & Accountability | AU-3 | Content of Audit Records | Limited data captured per event due to partial configuration |
| Audit & Accountability | AU-6 | Audit Record Review, Analysis, and Reporting | No CloudWatch integration means no automated alerting on events |
| Audit & Accountability | AU-9 | Protection of Audit Information | Log file validation disabled; log tampering cannot be detected |
| Audit & Accountability | AU-12 | Audit Record Generation | Incomplete audit record generation across service types |

---

## Framework Reference Summary

### ISO/IEC 27001:2022 — Annex A Controls Referenced

| Control Ref | Title |
|-------------|-------|
| A.9 | Access Control |
| A.12 | Operations Security (Logging and Monitoring) |
| A.13 | Communications Security (Network Security) |

### NIST SP 800-53 Rev. 5 — Control Families Referenced

| Abbreviation | Control Family |
|-------------|----------------|
| AC | Access Control |
| IA | Identification and Authentication |
| AU | Audit and Accountability |
| SC | System and Communications Protection |

---

## Key Observations

1. **Access Control (ISO A.9 / NIST AC)** is the most heavily implicated control area, appearing across three of five risks (R1, R2, R4). This indicates a systemic weakness in identity and access governance.

2. **R2 and R4 together** represent a compounded compliance gap - the combination of an unrestricted IAM policy and absent MFA means that a single compromised credential could satisfy the conditions for a full account breach, triggering multiple ISO 27001 and NIST control failures simultaneously.

3. **Logging and monitoring (ISO A.12 / NIST AU)** failures (R5) reduce the organisation's ability to detect whether other controls have been violated, amplifying the effective risk of all other findings.

---

*This compliance mapping was produced as part of a simulated GRC assessment for portfolio and educational purposes.*
