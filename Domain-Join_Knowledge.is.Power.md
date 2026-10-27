# APilot: Secure Enterprise Onboarding and Domain Join Method
# Identity-First, Zero-Trust, CMDB-Governed Device Trust Orchestration

## 1. Purpose

APilot is a secure enterprise onboarding and domain join method for Windows devices operating across internal, remote, and cloud-managed environments. It defines a structured lifecycle for device onboarding, trust validation, compliance verification, recovery, and controlled retirement. The method is intended to standardize how enterprise devices transition from an untrusted state to a trusted, compliant, and productive state across AD DS, Microsoft Entra ID, Intune, PKI, VPN, CMDB, and enterprise compliance controls.

This method is not limited to hybrid identity environments. It is designed for organizations that operate standard Windows endpoints, remote users, cloud-managed devices, AD DS-integrated environments, and mixed operating models where enterprise security and operational consistency must be enforced at scale.

The objective is clear: ensure that a device is not considered trusted or productive until identity, device posture, compliance, certificate validity, and operational readiness have all been validated. The method reduces manual work, prevents partial trust states, enforces rollback when appropriate, and includes a final wipe/decommission control for devices that are lost, stolen, compromised, or retired.

APilot is not a standalone Microsoft product. It is a Microsoft-aligned operating model that organizes native Microsoft capabilities into a single enforceable lifecycle for secure onboarding and domain join.

## 2. Scope

This method applies to enterprise Windows devices, including:

- laptops and desktops
- physical servers
- virtual machines
- remote-user devices
- cloud-managed endpoints
- hybrid AD DS + Entra environments
- internal-only enterprise networks
- remote access and VPN-connected environments

This method is intentionally general and enterprise-focused. It is not a hybrid-only design, and it is not a custom workaround for a single platform. It is a secure lifecycle model that can be implemented across multiple enterprise Windows management and identity environments.

This method covers:

- pre-onboarding validation
- identity verification
- device identity collection
- CMDB validation
- join mode selection
- VPN bootstrap and network context determination
- certificate issuance
- policy enforcement
- security control validation
- cleanup and hardening
- domain join and Entra registration
- compliance validation
- quarantine
- rollback
- wipe and decommission
- CMDB and ITSM update
- support and troubleshooting
- server and VM onboarding
- full operational traceability

## 3. Design Principles

APilot is built on the following principles:

1. Identity-first onboarding
2. Trust before productivity
3. Zero-trust enforcement during onboarding
4. Network-aware routing and join decisions
5. Certificate-based trust
6. Policy-driven decisioning
7. CMDB as the authoritative state source
8. Observability and lifecycle traceability
9. Quarantine and rollback by design
10. Wipe and decommission as final security control
11. Human-error reduction through automation and standardization
12. Privilege separation across orchestration, identity, PKI, and network planes

These principles form the foundation of the APilot method. They ensure that trust is established as a controlled process rather than assumed from a successful registration event or the presence of a user account.

## 4. Problem Statement

Existing onboarding models often fail because they focus on technical join actions rather than the real operational trust lifecycle. In large enterprise environments, the most common failure patterns are:

- user identity is not validated correctly
- device identity is not trusted before access is granted
- partial join states are allowed to persist
- policy compliance is not validated before the device becomes productive
- remote devices are joined without secure network reachability
- CMDB is stale or incomplete
- certificate status is not validated
- rollback is not supported
- support teams cannot explain why onboarding failed
- lost, stolen, or retired devices remain in a trusted or partially trusted state

This creates:

- redundant helpdesk effort
- inconsistent technician behavior
- onboarding drift
- security gaps
- partial trust states
- compliance exposure
- operational bottlenecks
- residual trust risk after loss, theft, or retirement

APilot addresses these issues by shifting onboarding from a human-driven process to a governed trust lifecycle. The purpose is not to add another layer of bureaucracy to a working process, but to make identity, posture, policy, and trust explicit, validated, enforceable, and recoverable.

## 5. APilot Method Definition

APilot defines a secure and governed method for enterprise onboarding and domain join. It covers the full lifecycle from untrusted endpoint to trusted, compliant, and productive device, and includes final wipe or decommission when the device is no longer safe or operationally valid.

The method is not a replacement for Intune, Microsoft Entra ID, or AD DS. Instead, it is a control model that layers trust validation around these systems so the enterprise can use them consistently and safely. It introduces a standard pattern for:

- validating identity
- validating device posture
- validating CMDB ownership
- validating identity and compliance before join
- granting time-bounded access
- enforcing short-lived trust artifacts
- quarantining failing devices
- rolling back failed joins
- wiping or decommissioning unsafe devices

The method enforces a simple rule:

A device is not considered ready until identity, posture, compliance, and trust are validated together.

## 6. High-Level Architecture

### 6.1 APilot Web Portal

The APilot portal provides centralized orchestration for onboarding activities. It supports:

- MFA verification
- invitation and approval flow
- hostname confirmation
- onboarding session control
- certificate issuance workflow
- join mode initiation
- wipe and decommission approval
- monitoring and escalation

The portal is the administrative control layer for the method. It provides the workflow context required to validate identity, confirm asset ownership, and track operational outcomes.

### 6.2 APilot Plugin

The APilot plugin is installed on the target device and performs device-side validation. It is responsible for:

- DIT collection
- CMDB validation
- OS compatibility and hardware validation
- VPN profile installation
- temporary certificate installation
- join execution
- cleanup and hardening
- post-onboarding validation
- wipe readiness and decommission signaling

The plugin is the execution element that translates policy into device actions. It ensures that device validation is performed before the device is allowed to continue in the lifecycle.

### 6.3 APilot Gateway

The APilot gateway provides the network and trust routing layer. It is responsible for:

- network context detection
- reachability validation
- certificate issuance routing
- join mode routing
- connectivity validation to AD DS, Entra ID, PKI, and CMDB
- security event routing for wipe and containment decisions

The gateway ensures that onboarding decisions are based on real operational context rather than assumptions about the device’s location or accessibility.

### 6.4 APilot Automation Engine

The automation engine is the orchestration component responsible for:

- join mode selection
- domain join execution
- Entra registration or hybrid join orchestration
- policy application
- rollback
- quarantine
- wipe and decommission execution
- CMDB write-back
- audit and event recording

The automation engine is the core control point for lifecycle continuity. It transforms the onboarding process from a sequence of isolated steps into a managed and governable workflow.

### 6.5 APilot CMDB

The CMDB is authoritative and must contain:

- device identity token
- assigned user
- device type
- device class
- join mode
- compliance state
- certificate state
- policy baseline
- wipe state
- support and incident attributes

The CMDB is not a passive record. It is the operational source of truth for device lifecycle state and is used to validate whether the device is allowed to advance in the trust flow or whether it must be wiped, quarantined, or decommissioned.

### 6.6 APilot PKI

The PKI model includes:

- temporary onboarding certificates
- permanent device certificates
- certificate templates
- revocation and renewal
- certificate-based trust for network and access validation
- certificate invalidation on wipe and retirement

The PKI layer is essential to the method because trust cannot be established on device identity alone. Certificate state must be validated in relation to the join operation, the network context, compliance state, and decommission decision.

## 7. Device Identity Model

APilot depends on a Device Identity Token (DIT). The DIT is the minimum structured trust record required before onboarding can continue. It represents the identity and posture of the device at a given point in time and provides a stable foundation for trust decisions.

The DIT includes:

- serial number
- BIOS UUID
- TPM identity
- hardware hash
- OS version and edition
- VM GUID if applicable
- MAC addresses as secondary identifiers
- assigned user
- asset ownership
- site or location
- device type
- join mode
- compliance baseline

This DIT becomes the anchor for:

- device validation
- CMDB consistency
- join decisions
- certificate issuance
- compliance enforcement
- quarantine decisions
- wipe and decommission decisions

The DIT is necessary because a simple device registration or inventory record is not sufficient evidence of trust. Device trust must be grounded in an identity record that includes the hardware, software, and policy context relevant to onboarding.

## 8. Security Model

### 8.1 Identity Security

The APilot process requires:

- MFA at critical steps
- user identity verification before trust is granted
- device and user validation before onboarding continues
- Conditional Access enforcement during trust stages
- identity invalidation when the device is lost, stolen, or retired

This aligns with Microsoft identity-first access principles and the broader Zero Trust enforcement model used in modern enterprise identity and access architectures.

### 8.2 Device Security

The device must satisfy:

- DIT validation
- hardware and identity checks
- posture readiness
- baseline policy readiness
- certificate validation
- no quarantine status
- no partial trust state
- no active risk state if a wipe decision has been authorized

This ensures the device is not treated as trusted simply because it has a valid user session or has been registered in a management platform.

### 8.3 Certificate Security

APilot requires:

- temporary onboarding certificates for remote bootstrap and device trust staging
- permanent device certificates for final trust
- certificate validation before productivity
- revocation and renewal controls
- expiry and CRL/OCSP enforcement
- automatic quarantine on certificate failure
- certificate invalidation on wipe or decommission

Certificate validation is necessary because a device may be joined and enrolled but still not be safe to use if its trust state, certificate lifecycle, or compliance posture is invalid.

### 8.4 Network Security

The method requires:

- secure network reachability validation
- VPN bootstrap where required
- identification of corporate vs non-corporate context
- validation of path to AD DS, Entra ID, PKI, and CMDB
- network-aware decisioning before join
- containment of a device when a wipe request is issued

This is essential in remote or hybrid environments in which a device may be unable to reach the required identity and trust services without first establishing a secure network path.

### 8.5 Operational Security

APilot requires:

- backend-driven privileged operations
- auditable operations
- no manual local admin execution by default
- restricted or sandboxed plugin execution
- rollback support
- explicit quarantine eventing
- explicit wipe authorization for decommission or loss events

This is important because uncontrolled local operations can create drift, hidden failure states, and difficult-to-trace misconfiguration.

## 9. Lifecycle States

APilot uses explicit onboarding states:

- untrusted
- discovered
- preflight validated
- temporary trust granted
- bootstrapped
- DIT verified
- join mode selected
- joined
- certificate issued
- policy applied
- compliant
- productive
- failed
- quarantined
- rollback initiated
- wipe required
- wiped
- decommissioned

This state model reduces ambiguity and makes the process observable. Each state has a distinct operational meaning, and transition to the next stage requires a validation gate. This eliminates the common enterprise failure mode where a device appears to be “joined” but is not actually safe, compliant, or trusted.

The final states—wipe required, wiped, and decommissioned—are critical for highly sensitive loss, theft, or retirement scenarios where the trust lifecycle must be closed forcibly.

## 10. Operational Sequence

### 10.1 Sequence Overview

1. User receives onboarding invitation
2. User authenticates with MFA
3. APilot portal validates identity and invitation
4. APilot plugin is delivered
5. Plugin collects DIT
6. CMDB validation runs
7. Network context is evaluated
8. VPN bootstrap is triggered if required
9. Temporary certificate is issued if required
10. Join mode is selected
11. Join execution is performed
12. Permanent certificate is issued
13. Policy baseline is applied
14. Cleanup and hardening run
15. Compliance validation occurs
16. Device enters productive state
17. CMDB write-back is performed
18. Audit record is stored
19. Wipe decision is evaluated for theft, loss, or retirement
20. Wipe or decommission executes when required

This sequence defines the onboarding lifecycle. It is intentional, stage-based, and governed by explicit validation gates.

### 10.2 User Onboarding Flow

#### Step 1 — Invitation Sent
An administrator or workflow initiates onboarding.

#### Step 2 — User Opens the Invitation
The user opens the onboarding link on the target device.

#### Step 3 — MFA Validation
The user authenticates using an approved identity method.

#### Step 4 — Plugin Delivery
The APilot plugin is downloaded and installed.

#### Step 5 — DIT Collection
The plugin collects:

- serial number
- BIOS UUID
- TPM data
- OS version
- hardware details
- device ownership data

#### Step 6 — CMDB Validation
The device checks:

- asset match
- assigned user match
- device class match
- join mode match
- quarantine state
- certificate state
- wipe state

#### Step 7 — Network Evaluation
The device validates:

- corporate network
- remote network
- VPN requirements
- path to AD DS, Entra ID, PKI, CMDB

#### Step 8 — VPN Bootstrap
If required:

- install signed VPN profile
- install temporary certificate
- authenticate to VPN
- verify corporate network reachability

#### Step 9 — Join Execution
APilot selects a join mode:

- AD DS only
- hybrid AD DS + Entra ID
- Entra ID only
- remote bootstrap
- server/VM path

#### Step 10 — Certificate Issuance
Permanent certificate is issued after trust checkpoint passes.

#### Step 11 — Policy Enforcement
The platform applies:

- GPO or equivalent legacy configuration
- Intune configuration
- compliance profile
- security policy
- certificate dependency checks

#### Step 12 — Cleanup and Hardening
The platform performs:

- software cleanup
- stale package removal
- security hardening
- Office and security tool application
- final reboot

#### Step 13 — Final User Authentication
The user completes final login and the device is marked as productive.

#### Step 14 — Wipe and Decommission Decision
A device may later transition to a wipe or decommission path when:

- it is lost or stolen
- it is retired from service
- it fails a trust or compliance event that cannot be remediated safely
- its identity or CMDB record is no longer valid
- it is subject to legal, regulatory, or security containment action

When this condition is reached, the device exits the productive state and enters a security-controlled wipe path.

## 11. APilot Integration Map — Enterprise Zero-Trust Trust Boundary Model

APilot’s Integration Map defines the trust boundaries, API contracts, and communication flows between all planes of the onboarding architecture. It ensures that:

- no device can directly communicate with privileged systems
- no trust artifact is issued without validation
- no domain join occurs without explicit authorization
- every action is logged, auditable, and reversible
- every integration follows zero-trust principles

This model is aligned with:
- Microsoft Zero Trust Architecture
- AWS Zero Trust Maturity Model
- IBM Zero Trust Framework
- Oracle Identity Security Architecture
- NinjaOne Zero Trust RMM Practices

### 11.1 Integration Philosophy
APilot enforces strict privilege separation:

- Device Plane: lowest privilege; no direct access to identity or PKI
- Control Plane: orchestrator and gatekeeper; issues temporary trust artifacts
- Identity Plane: final authority for domain join
- PKI Plane: issues and revokes certificates
- Policy Plane: validates posture and compliance
- CMDB Plane: authoritative asset state
- Network Plane: grants temporary access and enforces segmentation

This is identical to how Microsoft, AWS, IBM, Oracle, and NinjaOne design enterprise onboarding and trust boundaries.

### 11.2 Integration Matrix (Vendor-Aligned)

#### Device → Control Plane
Purpose: registration, DIT upload, posture telemetry

Payload:
- DIT
- serial
- BIOS UUID
- TPM identity
- hardware hash
- OS version
- network context
- user identity token

Authentication:
- mTLS using Temporary Onboarding Certificate (TOC)
- device-bound private key

Vendor alignment:
- Microsoft: Autopilot / Intune device registration
- AWS: SSM hybrid join bootstrap
- IBM: MaaS360 device registration
- Oracle: OCI Identity Domain device enrollment
- NinjaOne: agent telemetry upload

#### Control Plane → CMDB Plane
Purpose: validate asset ownership and lifecycle state

Payload:
- DIT
- asset ID
- owner ID
- department
- device classification

Authentication:
- OAuth 2.0 client credentials
- service principal with least privilege

Vendor alignment:
- ServiceNow CMDB
- BMC Helix
- IBM Control Desk
- Oracle Enterprise Manager
- NinjaOne Asset Inventory

#### Device → Control Plane (User Identity Validation)
Purpose: validate user identity and MFA

Payload:
- user JWT
- MFA claim
- session ID

Authentication:
- OAuth 2.0 / OIDC
- Entra ID / AWS IAM / IBM Verify / Oracle Identity

#### Device → Control Plane (DIT Telemetry Upload)
Purpose: upload hardware identity and posture

Payload:
- DIT
- TPM attestation
- patch level
- encryption state
- AV / EDR status

Authentication:
- mTLS

#### Control Plane → Policy Plane
Purpose: validate posture and compliance

Payload:
- DIT
- compliance rules
- policy assignments

Authentication:
- mTLS
- API key

#### Control Plane → PKI Plane
Purpose: issue temporary and permanent certificates

Payload:
- CSR
- DIT
- certificate template ID

Authentication:
- managed identity
- HSM-backed key protection

#### Control Plane → Network Plane
Purpose: grant temporary join window access

Payload:
- device IP
- target DC
- duration
- access scope

Authentication:
- OAuth 2.0
- mTLS

#### Control Plane → Identity Plane
Purpose: request one-time join token

Payload:
- DIT
- user context
- join mode (AD DS / Entra / hybrid)

Authentication:
- Kerberos (AD DS)
- OAuth 2.0 (Entra ID)
- IAM (AWS Managed AD)
- IBM Security Verify
- Oracle Identity Domain

#### Device → Identity Plane
Purpose: execute domain join

Payload:
- join token
- domain config
- computer object attributes

Authentication:
- join token
- RPC / Kerberos / LDAP

#### Identity Plane → Control Plane
Purpose: confirm join success

Payload:
- device ID
- join timestamp
- computer object SID

Authentication:
- OAuth 2.0
- webhook callback

#### Control Plane → CMDB Plane (Final Write-Back)
Purpose: update lifecycle state

Payload:
- device ID
- join status
- certificate state
- compliance state
- quarantine state

Authentication:
- OAuth 2.0 client credentials

### 11.3 Trust Boundary Enforcement
APilot enforces strict trust boundaries:

- Device Plane → Control Plane only
- No device can directly access:
  - Domain Controllers
  - PKI
  - CMDB
  - Identity Plane
- Control Plane mediates all privileged actions
- Identity Plane is final authority
- PKI Plane is trust issuer
- CMDB Plane is lifecycle authority
- Network Plane grants only temporary access

This mirrors:
- Microsoft Conditional Access
- AWS Verified Access
- IBM Zero Trust Gateway
- Oracle ZTNA
- NinjaOne Zero Trust RMM

## 12. APilot Identity Plane — Enterprise Identity Authority

The Identity Plane is the final arbiter of trust in APilot. It validates user identity, device identity, and computer object creation, and ensures that no device becomes trusted until identity, posture, and CMDB validation are complete.

This model aligns with:
- Microsoft Entra ID + AD DS hybrid identity
- AWS IAM Identity Center + AWS Managed AD
- IBM Security Verify
- Oracle Identity Domains
- NinjaOne SSO + directory sync

### 12.1 Identity Plane Responsibilities

1. User Identity Validation
- MFA enforcement
- Conditional Access evaluation
- identity segmentation by role, department, and privilege
- ownership validation against CMDB

2. Device Identity Validation
- validate DIT (serial, BIOS UUID, TPM identity, hardware hash)
- validate device classification (endpoint, server, VM)
- validate device ownership and assignment
- validate device is not duplicate, stale, or quarantined

3. Computer Object Lifecycle
- pre-stage computer object
- issue one-time join token
- validate secure channel establishment
- confirm domain join success
- update identity metadata such as group membership, OU placement, and trust attributes

4. Trust Enforcement
- the device is not trusted until join is confirmed
- the device is not productive until compliance is validated
- the device is revoked immediately upon quarantine
- the device is blocked when CMDB or ownership mismatch is detected

### 12.2 Vendor Alignment
Microsoft:
- Entra ID MFA
- hybrid join (AD DS + Entra)
- Conditional Access
- device registration
- computer object lifecycle

AWS:
- IAM Identity Center
- AWS Managed AD computer object creation
- SSM hybrid join

IBM:
- Security Verify MFA
- directory governance
- device identity validation

Oracle:
- Identity Domain MFA
- directory object lifecycle
- trust mapping

NinjaOne:
- directory sync
- identity validation for RMM actions

## 13. APilot Control Plane — Zero-Trust Orchestration Engine

The Control Plane is the central orchestrator of APilot. It enforces policy gates, validates identity, evaluates posture, issues temporary trust artifacts, and coordinates all privileged actions.

This mirrors:
- Microsoft Conditional Access + Intune Compliance Engine
- AWS Verified Access + Systems Manager
- IBM Zero Trust Gateway
- Oracle ZTNA + Identity Governance
- NinjaOne Zero Trust RMM

### 13.1 Control Plane Responsibilities

1. Workflow Orchestration
- manage onboarding state machine
- sequence validation gates
- enforce deterministic state transitions
- control the order of action and trust validation

2. Trust Artifact Issuance
- issue temporary onboarding certificates
- issue one-time join tokens
- grant temporary network access
- revoke expired or stale artifacts

3. Policy Enforcement
- validate posture
- validate compliance
- validate CMDB ownership
- validate identity segmentation
- determine whether a device may proceed or must stop in quarantine

4. Privilege Separation
- the device cannot directly access identity or PKI
- the Control Plane mediates all privileged actions
- the Control Plane logs all actions for audit and traceability

5. Lifecycle Governance
- update CMDB
- update ITSM
- trigger rollback
- trigger quarantine
- trigger wipe or decommission
- close the trust window after successful finalization

### 13.2 Vendor Alignment
Microsoft:
- Conditional Access
- Intune Compliance Engine
- Autopilot workflow orchestration

AWS:
- Verified Access
- Systems Manager Automation
- IAM policy enforcement

IBM:
- Zero Trust Gateway
- BigFix compliance orchestration

Oracle:
- ZTNA
- Identity Governance workflows

NinjaOne:
- policy engine
- zero-trust RMM orchestration

## 14. APilot Network Plane — Secure Join Window Enforcement

The Network Plane provides the temporary, tightly-scoped network access required for onboarding and domain join. It ensures that devices cannot access privileged systems until identity, posture, and CMDB validation are complete.

This aligns with:
- Microsoft Always On VPN + ZTNA
- AWS Verified Access + VPC Security Controls
- IBM ZTNA
- Oracle ZTNA
- NinjaOne Network Policy Enforcement

### 14.1 Network Plane Responsibilities

1. Network Context Detection
- internal vs external
- VPN vs direct
- cloud-managed vs hybrid
- remote access vs corporate LAN

2. Temporary Join Window
Grant access only to:
- Domain Controllers
- PKI
- Identity Plane
- Control Plane

Duration:
- 15 minutes
- access is revoked automatically after the allowed timeframe or after successful join

3. Segmentation Enforcement
- block lateral movement
- block privileged ports
- block non-essential services
- enforce ZTNA and micro-segmentation policies

4. Post-Join Network Hardening
- close join window
- enforce full segmentation
- apply VPN or zero-trust policies
- restrict network access to only the required operational services

### 14.2 Vendor Alignment
Microsoft:
- Always On VPN
- Conditional Access network policies
- ZTNA / Entra Private Access

AWS:
- Verified Access
- VPC security groups
- PrivateLink for identity services

IBM:
- ZTNA
- network segmentation

Oracle:
- ZTNA
- Identity Domain network policies

NinjaOne:
- network policy enforcement
- remote access governance

## 15. APilot State Model — Deterministic, Auditable Lifecycle

APilot enforces a deterministic, auditable state machine for device onboarding. No device can skip states, bypass validation, or enter a productive state without passing all gates.

This mirrors:
- Microsoft Autopilot + Intune compliance lifecycle
- AWS Systems Manager lifecycle
- IBM BigFix lifecycle
- Oracle OS Management lifecycle
- NinjaOne device lifecycle governance

State Sequence:
- Unknown
- Invited / Identity Validation
- Discovery / DIT Collection
- Posture Validation
- Deployment Initiation
- Secure Channel Established
- Joined / Compliant
- Productive
- Failed
- Quarantined
- Rollback Initiated
- Wipe Required
- Wiped
- Decommissioned

### 15.1 State Transition Rules

Identity Validation → Discovery
- MFA required
- user identity validated
- CMDB ownership validated

Discovery → Posture Validation
- DIT collected
- hardware identity validated

Posture Validation → Deployment Initiation
- OS version
- patch level
- encryption
- EDR / AV
- compliance baseline

Deployment Initiation → Secure Channel
- temporary certificate issued
- VPN / ZTNA join window opened

Secure Channel → Joined
- domain join executed
- Identity Plane confirms computer object creation

Joined → Productive
- permanent certificate issued
- policies applied
- compliance validated

### 15.2 Failure Handling
Any failure transitions the device to:
- Failed
- Quarantined
- Rollback Initiated
- Wipe Required

No partial trust states are allowed.

## 16. APilot Quarantine & Rollback — Zero-Trust Recovery Model

Quarantine and rollback are first-class citizens in APilot. They ensure that no device remains partially trusted, partially joined, or partially compliant.

This aligns with:
- Microsoft Intune quarantine + Conditional Access
- AWS Verified Access deny policies
- IBM Security Verify quarantine
- Oracle Identity Domain deny policies
- NinjaOne device isolation

### 16.1 Quarantine Conditions
A device enters quarantine when:
- DIT mismatch
- CMDB mismatch
- certificate failure
- VPN failure
- join failure
- compliance failure
- security anomaly
- device reported lost or stolen
- device ownership conflict
- duplicate asset record

### 16.2 Quarantine Actions
- block onboarding
- block certificate issuance
- block VPN / ZTNA
- block domain join
- restrict network access
- notify admin
- notify user
- update CMDB
- create ITSM incident
- trigger rollback or wipe evaluation

### 16.3 Rollback Procedure
Rollback restores the device to a safe state.

Rollback actions:
- revoke temporary certificate
- revoke permanent certificate
- remove computer object
- remove VPN profile
- remove join token
- reset CMDB state
- remove plugin artifacts
- update audit logs
- create ITSM incident

Rollback is allowed only when:
- device is recoverable
- identity is valid
- CMDB ownership is valid
- policy and compliance can be restored

### 16.4 Wipe Procedure
Wipe is required when:
- device is lost
- device is stolen
- device is compromised
- device cannot be trusted
- device cannot be recovered
- CMDB record is invalid
- identity mismatch cannot be resolved

Wipe actions:
- revoke all certificates
- disable all access
- remove device from identity plane
- remove device from CMDB
- trigger secure wipe
- update ITSM
- preserve forensic evidence

## 17. Timeline and Trust Windows

The APilot architecture uses short-lived security artifacts to reduce the risk of replay, unauthorized reuse, or lingering trust after a failed join attempt. The trust window is intentionally narrow and is enforced by the Control Plane, PKI Plane, and Network / Remote Access Plane.

### 17.1 Temporary Join Token Lifetime
- A temporary join token is valid for a single use only.
- Once the domain join handshake is successfully completed, the token is invalidated immediately.
- If the connection is interrupted or the device fails to complete the handshake within the permitted join window, the token ceases to be valid and cannot be replayed.
- A reattempt requires a new onboarding session and a new join token.

### 17.2 Temporary Certificate Lifetime
- A temporary access certificate is also valid for a single use only.
- It is issued for the purpose of establishing the temporary trust path required to complete the join flow.
- Once the handshake is complete, the certificate is invalidated automatically.
- If the connection is interrupted, the device fails to complete the required join step, or the device enters quarantine, the certificate is revoked and cannot be reused.

### 17.3 Allowed Join Window
- The join process must complete within a defined and very short time window.
- If the device does not complete the domain join within the permitted timeframe, the workflow is voided.
- Any issued join token, temporary certificate, and temporary network access are invalidated automatically.
- A new invitation or enrollment session must be created before the device can attempt the join again.

### 17.4 Revocation of Stale Join Tokens and Certificates
Stale or obsolete access tokens and certificates are revoked under the following conditions:
- expiration date reached
- device enters quarantine
- device is disabled
- device is placed on a no-allow list
- trust is revoked due to failed onboarding
- the device is reported lost, stolen, compromised, or decommissioned
- the join process is manually interrupted or failed by policy enforcement

### 17.5 Trust Enforcement Rule
A device is considered trusted only after the Identity Plane confirms the join outcome and the Control Plane updates the CMDB and asset state. Until that point, the device remains in a controlled, time-bounded trust window and retains no permanent enterprise trust.

## 18. Re-entry After Wipe or Decommission

A device that has undergone a wipe or has been decommissioned may be reintroduced to the onboarding lifecycle only after a fresh validation cycle. The device is not automatically trusted simply because the wipe was successful. A wipe removes the current trust state, but it does not imply automatic re-entry.

### 18.1 Automatic Return to Onboarding
- A device may re-enter the onboarding process only after the wipe has been completed successfully and the device is no longer present in:
  - AD DS
  - Entra ID
  - Intune
  - the CMDB / ITSM asset inventory as an active device
- If the device still exists in any trust source, it must remain in a quarantined or blocked state until the trust record is explicitly reconciled.

### 18.2 CMDB Re-registration Requirements
- CMDB registration is unique and must be treated as a closed lifecycle record.
- If the device has no valid active record, a new registration must be created before onboarding can resume.
- If a duplicate record is detected, the device is prevented from proceeding to the join phase and is placed in a quarantine state for manual review.

### 18.3 DIT Re-collection Requirement
- A new Device Identity Token (DIT) must be collected for any device re-entering the join process.
- This includes a fresh readout of:
  - serial number
  - BIOS UUID
  - TPM identity
  - hardware hash
  - OS version and edition
  - ownership and assignment context
- If a duplicate or conflicting DIT is detected, the device is rejected and the case is escalated for review.

### 18.4 Certificate Lifecycle Requirement
- A new certificate is required only when the prior certificate is no longer valid or has been revoked.
- If the existing certificate remains valid and the device is otherwise eligible for re-entry, a reissue may be allowed under policy.
- However, the certificate must always be revalidated against the current trust state and current device posture.

### 18.5 Asset Owner Re-validation
- Following the Zero-Trust principle, the asset owner must be re-validated before the device can re-enter the productive lifecycle.
- The owner, asset assignment, department, and device classification must be confirmed against the current CMDB and identity records.
- If the owner cannot be validated, the device remains blocked from onboarding.

### 18.6 Re-entry Policy Gate
A device re-entering the environment must pass all of the following:
- fresh identity validation
- new DIT collection
- CMDB uniqueness and ownership validation
- compliance baseline validation
- certificate validation or reissue
- successful network trust establishment
- successful join execution and final trust confirmation

### 18.7 Exception and Break-glass Handling
- Re-entry after wipe is not treated as an automatic exception.
- Any exception must be approved by the appropriate authority and recorded as a controlled change.
- Break-glass re-entry must be audited and should trigger a specific security review and SIEM alert.

## 19. APilot PKI Design — Enterprise-Grade, Vendor-Aligned, Zero-Trust Certificate Architecture

APilot’s PKI Plane is responsible for issuing, validating, revoking, and governing all trust artifacts used during onboarding, domain join, compliance validation, and long-term device lifecycle management.

This PKI model is intentionally aligned with best practices from:
- Microsoft (Intune PKI Connector, AD CS, Entra certificate-based authentication)
- AWS (AWS Private CA, ACM PCA, SSM hybrid join)
- IBM (IBM Security Verify, MaaS360 certificate lifecycle)
- Oracle (OCI Certificate Authority Service, Identity Domains)
- NinjaOne (agent-based certificate deployment and monitoring)

It is designed to be vendor-neutral, zero-trust aligned, and fully auditable, ensuring that no device receives permanent trust until identity, posture, compliance, and CMDB validation are complete.

### 19.1 PKI Architecture Overview
APilot uses a dual-certificate model:

Temporary Onboarding Certificate (TOC)
- short-lived
- non-exportable
- bound to the Device Identity Token (DIT)
- used only for onboarding, posture validation, and secure channel establishment

Permanent Device Trust Certificate (PDTC)
- long-lived
- used for VPN, Wi-Fi, mTLS, device trust, and conditional access
- rotated automatically
- revoked immediately upon quarantine, wipe, or decommission

This mirrors Microsoft’s Intune SCEP / PKCS patterns, AWS ACM Private CA usage, IBM MaaS360 certificate lifecycle governance, Oracle OCI CA Service, and NinjaOne agent-based certificate monitoring.

### 19.2 Temporary Certificate Requirements (TOC)
Temporary certificates are issued only after identity, CMDB, and posture validation. They must follow these rules:

Lifetime:
- valid for 15 minutes
- single-use only
- automatically revoked after the join window closes

Security Requirements:
- non-exportable private key
- bound to DIT using device serial, BIOS UUID, TPM identity, and hardware hash
- issued only via mTLS
- stored in the local machine store with ephemeral flags

Vendor Alignment:
- Microsoft: mirrors short-lived onboarding certs used with Intune SCEP / PKCS
- AWS: mirrors short-lived bootstrap certificate patterns used with ACM PCA
- IBM: aligns with MaaS360 ephemeral device identity certs
- Oracle: aligns with OCI CA short-lived enrollment certs
- NinjaOne: agent can validate certificate presence and expiration

Known Issues Addressed:
- prevents replay attacks
- prevents stale cert reuse
- prevents partial trust states
- prevents hybrid join failures caused by expired bootstrap certs
- reduces domain join failures due to expired or missing machine certs

### 19.3 Permanent Device Trust Certificate (PDTC)
Permanent certificates are issued only after successful domain join and identity-plane confirmation.

Lifetime:
- 1–3 years, configurable
- automatic renewal via APilot or Intune PKI Connector
- revoked immediately upon:
  - wipe
  - decommission
  - CMDB mismatch
  - device compromise
  - quarantine

Security Requirements:
- RSA 2048+ or ECC P-256+
- stored in TPM-backed key storage when available
- mTLS enforced for certificate-based authentication
- certificate chain validated against enterprise root CA

Vendor Alignment:
- Microsoft: AD CS + Intune PKCS + Entra CBA
- AWS: ACM PCA + SSM hybrid join
- IBM: MaaS360 device identity certificates
- Oracle: OCI CA Service + Identity Domain trust
- NinjaOne: agent monitors certificate expiration and trust state

Known Issues Addressed:
- hybrid join failures caused by missing machine certs
- VPN failures due to expired certs
- Wi-Fi 802.1X failures due to invalid cert chain
- Intune compliance failures caused by certificate revocation
- stale machine identity in hybrid or cloud-enrolled environments

### 19.4 Certificate Revocation Model
APilot enforces strict revocation rules:

Revocation Triggers:
- device enters quarantine
- device fails posture validation
- device fails join
- device is wiped
- device is decommissioned
- device is reported lost or stolen
- CMDB mismatch
- identity mismatch
- certificate expiration
- certificate compromise

Revocation Mechanisms:
- CRL update
- OCSP update
- immediate revocation via PKI Plane
- notification to:
  - VPN
  - Wi-Fi
  - ZTNA
  - Identity Plane
  - CMDB
  - ITSM

Vendor Alignment:
- Microsoft: AD CS CRL + OCSP, Intune PKI Connector
- AWS: ACM PCA CRL + OCSP
- IBM: MaaS360 certificate revocation API
- Oracle: OCI CA revocation API
- NinjaOne: agent detects revoked certs and alerts ITSM

### 19.5 Certificate Templates
APilot requires two certificate templates:

Template A — Temporary Onboarding Certificate
- EKU: Client Authentication
- Key Usage: Digital Signature
- Non-exportable
- Validity: 15 minutes
- Renewal: not allowed
- Revocation: automatic

Template B — Permanent Device Trust Certificate
- EKU: Client Authentication, Smart Card Logon if required
- Key Usage: Digital Signature, Key Encipherment
- Validity: 1–3 years
- Renewal: automatic
- Revocation: immediate on quarantine

Vendor Alignment:
- Microsoft: AD CS templates + Intune PKCS
- AWS: ACM PCA templates
- IBM: MaaS360 certificate profiles
- Oracle: OCI CA templates
- NinjaOne: agent-based certificate deployment

### 19.6 PKI Plane Trust Boundaries
The PKI Plane is privileged and must be isolated:

Isolation Requirements:
- no direct device access
- all certificate issuance must go through Control Plane
- all revocation must be logged in CMDB
- all certificate operations must be audited

Vendor Alignment:
- Microsoft: PKI isolated from domain controllers
- AWS: ACM PCA isolated from VPC endpoints and network policy paths
- IBM: PKI isolated from MaaS360 MDM
- Oracle: OCI CA isolated from Identity Domain
- NinjaOne: agent must not possess certificate issuance privileges

### 19.7 PKI Known Issues and APilot Mitigations

Microsoft Known Issues:
- Hybrid join fails when the machine certificate is missing
- Intune SCEP failures due to NDES throttling
- Autopilot can stall at “Identifying” when certificate state is mismatched

APilot Mitigation:
- Control Plane validates posture and PKI before join

AWS Known Issues:
- ACM PCA certificates are not trusted by AD DS without proper chain import
- SSM hybrid join fails if bootstrap cert expires

APilot Mitigation:
- temporary certificate lifetime is strictly enforced

IBM Known Issues:
- MaaS360 device identity certificates are not rotated properly
- certificate revocation is not synced to VPN or network access

APilot Mitigation:
- PKI Plane revocation triggers network-plane updates

Oracle Known Issues:
- OCI CA certificates are not recognized by Windows unless the chain is imported
- Identity Domain trust requires explicit certificate mapping

APilot Mitigation:
- Control Plane enforces chain validation before join

NinjaOne Known Issues:
- agent reports certificate expiration but cannot revoke
- certificate deployment requires manual profile creation

APilot Mitigation:
- PKI Plane handles issuance and revocation centrally

### 19.8 PKI Plane Summary
APilot’s PKI Plane is:
- zero-trust aligned
- vendor-neutral
- fully auditable
- short-lived for onboarding
- long-lived for device trust
- revocation-driven
- CMDB-governed
- identity-plane validated
- network-plane enforced

## 20. KPI Model for Validation

To validate the effectiveness, security, and efficiency of the domain join control plane, the following measurable outcomes are tracked.

### 20.1 Performance and Efficiency Metrics

- Average onboarding time
  - Definition: total elapsed time from device state = Unknown to device state = Joined, averaged over all successful joins
  - Target: under 15 minutes
  - Risk if failed: high user frustration and loss of productivity

- Compliant-to-productivity rate
  - Definition: percentage of successful joins completed without manual exceptions or policy overrides
  - Target: above 95%
  - Risk if failed: indicates weak policy baselines or excessive exception behavior

- Deployment latency
  - Definition: time from Posture Validation pass to Secure Channel Established
  - Target: under 120 seconds
  - Risk if failed: indicates a bottleneck in identity issuance or network trust activation

### 20.2 Security and Risk Metrics

- CMDB mismatch rate
  - Definition: percentage of join attempts blocked because hardware identity or ownership data does not match the CMDB
  - Target: low and generally within a baseline tolerance
  - Risk if failed: indicates poor asset hygiene or spoofing attempts

- Time to quarantine
  - Definition: time from failure event to device being placed on the no-allow list
  - Target: under 30 seconds
  - Risk if failed: increases the window for a non-compliant device to attempt lateral movement

- Time to wipe
  - Definition: time from emergency wipe trigger to confirmed OS-level wipe acknowledgement
  - Target: under 5 minutes
  - Risk if failed: increases data exfiltration and trust exposure risk

- Unauthorized join attempts
  - Definition: number of join requests reaching the control plane without a valid bootstrap certificate or valid authorization
  - Target: near zero
  - Risk if failed: indicates discovery, probing, or malicious activity

### 20.3 Reliability and Stability Metrics

- Failed join rate
  - Definition: percentage of join attempts that fail and require retry or intervention
  - Target: under 2%
  - Risk if failed: creates support burden and ticket storms

- Rollback success rate
  - Definition: percentage of failed joins that are fully recovered through automated cleanup and state reset
  - Target: 100%
  - Risk if failed: leaves stale identities, ghost computer objects, or unresolved trust records

- Mean time to resolution
  - Definition: average elapsed time from quarantine ticket generation to engineer validation and state reset
  - Target: under 4 hours
  - Risk if failed: prolonged support backlog and stranded devices

Additional trust-window and lifecycle metrics:
- token expiration enforcement rate
- temporary certificate revocation success
- join timeout rate
- wipe re-entry validation rate
- percentage of direct re-entry attempts blocked without fresh validation

These metrics ensure the control plane is not only functional but also efficient, secure, and resilient under enterprise conditions.

## 21. Operational Guardrails and Failure Handling

To make the architecture operationally enforceable, APilot defines the following guardrails:

### 21.1 Retry and Timeout Policy
- a pending join is allowed a limited time window
- tokens and certs expire automatically
- a failed handshake invalidates all temporary trust artifacts
- the device is quarantined when the allowed window expires
- a new invitation is required before reattempting

### 21.2 Idempotency Rules
- duplicate join requests are treated as a single session
- repeated calls with the same session ID are denied or merged
- join state is finalized only once the identity plane confirms successful creation
- multiple callbacks are deduplicated using correlation ID

### 21.3 Exception Governance
- policy exceptions require approval
- break-glass actions require explicit sign-off
- exceptions are time-bound and auditable
- SIEM and ITSM logging is mandatory

### 21.4 Re-entry Rules
- a wiped device does not re-enter automatically
- fresh DIT is required
- fresh ownership validation is required
- CMDB uniqueness must be confirmed
- certificate validation or reissue is required
- no direct re-entry without a fresh trust cycle

## 22. Supporting Official References

The APilot method is grounded in enterprise and security principles reflected in official documentation from Microsoft, IBM, AWS, Oracle, and NinjaOne. These references support the following ideas:

- identity-first access and trust decisions
- zero-trust verification before access is granted
- device identity and certificate-based trust
- compliance-before-access models
- endpoint health and operational telemetry
- automation and remediation in enterprise environments
- CMDB and ITSM integration for service and governance
- secure device retirement and wipe controls in operational environments
- zero-trust certificate lifecycle and short-lived trust enforcement

### Microsoft Official Documentation
- Windows Autopilot hybrid join  
  https://learn.microsoft.com/en-us/autopilot/windows-autopilot-hybrid

- User-driven Microsoft Entra hybrid join workflow  
  https://learn.microsoft.com/en-us/autopilot/tutorial/user-driven/hybrid-azure-ad-join-workflow

- Intune Windows enrollment guide  
  https://learn.microsoft.com/en-us/intune/device-enrollment/windows/guide

- Windows enrollment methods  
  https://learn.microsoft.com/en-us/mem/intune/enrollment/windows-enrollment-methods

- Intune certificates configuration  
  https://learn.microsoft.com/en-us/mem/intune/protect/certificates-configure

- Intune compliance policies  
  https://learn.microsoft.com/en-us/mem/intune/protect/protect-devices

- Conditional Access overview  
  https://learn.microsoft.com/en-us/entra/identity/conditional-access/overview

- Microsoft Entra device join concepts  
  https://learn.microsoft.com/en-us/entra/identity/devices/concept-directory-join

### IBM Official Documentation
- IBM Zero Trust implementation guide  
  https://www.ibm.com/think/topics/zero-trust-implementation

- IBM Zero Trust solutions  
  https://www.ibm.com/solutions/zero-trust

- IBM Verify adaptive access  
  https://docs.verify.ibm.com/verify/docs/adaptive-access

### AWS Official Documentation
- AWS Zero Trust principles  
  https://docs.aws.amazon.com/prescriptive-guidance/latest/strategy-zero-trust-architecture/zero-trust-principles.html

- AWS Zero Trust components  
  https://docs.aws.amazon.com/prescriptive-guidance/latest/strategy-zero-trust-architecture/components.html

- AWS device manufacturing and provisioning with X.509 certificates  
  https://docs.aws.amazon.com/whitepapers/latest/device-manufacturing-provisioning/reference-architectures.html

- AWS IoT device onboarding guidance  
  https://docs.aws.amazon.com/iot-mi/latest/devguide/managedintegrations-sdk-v2-device-onboarding.html

### Oracle Documentation
- Oracle Cloud Infrastructure Certificate Authority  
  https://docs.oracle.com/en-us/iaas/Content/CertificateAuthority/Concepts/certificateauthorityoverview.htm

- Oracle Identity Domain documentation  
  https://docs.oracle.com/en-us/iaas/Content/identity/home.htm

### NinjaOne Documentation
- NinjaOne automation and remediation documentation  
  https://www.ninjaone.com/

- NinjaOne help center topics on automation, patching, monitoring, scripting, and ITSM integration  
  https://help.ninjaone.com/

## 23. Final Positioning

APilot is a secure enterprise onboarding and domain join method designed to unify identity validation, device trust, certificate-based onboarding, compliance gating, rollback, and final wipe control in a single operational model.

It is not a standalone Microsoft product. It is a Microsoft-aligned operating model built on native Microsoft capabilities and supported by enterprise zero-trust principles used across modern endpoint and identity architectures.

The method is intended to reduce manual technician dependency, prevent partial trust states, improve onboarding consistency, and provide operational traceability for security, support, and governance teams. It also closes the final security gap: when a device is lost, stolen, retired, or compromised beyond safe recovery, the lifecycle includes a controlled wipe so the device cannot continue to hold enterprise trust or access.

## 24. Closing Statement

APilot defines a new operational model for enterprise onboarding and secure device trust. It is designed to close the gaps that remain after enrollment, registration, and policy assignment are complete.

The goal is straightforward:

- no untrusted device becomes productive
- no partial trust state remains hidden
- no device joins without identity, policy, compliance, and certificate validation
- no lost, stolen, or decommissioned device remains in an active trust state
- no onboarding event is left without traceability

The result is a more secure, governable, and operationally consistent onboarding process that works across internal, remote, hybrid, and cloud-managed enterprise environments.

## 25. Final Architecture Gap Review

This version is strong and covers the main enterprise requirements. It now includes:

- a clear multi-plane trust architecture
- explicit validation gates
- a deterministic state model
- trust window enforcement
- re-entry of wiped devices
- source-of-truth definitions
- PKI model and certificate lifecycle
- vendor-aligned guidance for Microsoft, AWS, IBM, Oracle, and NinjaOne
- KPI model for operation monitoring
- governance and security boundary controls

The remaining items that are still worth clarifying before formal sign-off are:

1. Canonical state naming
- Use one final state list everywhere and keep it consistent across the document.

2. Retry and timeout policy
- Define how long a device remains in pending state before it is automatically failed and quarantined.
- Define behavior for stale join tokens and delayed identity callback events.

3. Exception governance model
- Define who can approve policy exceptions and break-glass join actions.
- Define how long exceptions are valid and how they are logged.

4. Re-entry automation rules
- Define whether wiped devices require manual re-registration or can be auto-registered based on policy.
- Define whether a new DIT is always required or only on a policy trigger.

5. Detailed service-to-service contract mapping
- This is not missing conceptually, but it would benefit from a concise API contract list or service dependency matrix.

6. Operational ownership matrix
- Explicitly map:
  - Control Plane owner
  - CMDB owner
  - PKI owner
  - Identity owner
  - Security owner
  - Support owner

7. Event retention and audit policy
- Define how long onboarding and trust events are retained for audit.
- Define how this aligns with enterprise compliance and incident response requirements.

8. Final trust state decomposition
- Clarify whether the device is trusted only after:
  - computer object creation
  - certificate issuance
  - CMDB update
  - policy pass
  - user assignment
  - network access closure

This final iteration remains a solid and enterprise-ready architecture and governance model. The remaining items are operational and governance refinements, not conceptual gaps.
