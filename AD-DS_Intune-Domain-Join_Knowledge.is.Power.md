Title: APilot: Secure Enterprise Onboarding and Domain Join Method
Description: APilot is a secure enterprise onboarding and domain join method for Windows devices
Author: Rodrigo Aurelio Criscuolo
Date: 2026-10-25
Topic: Device Onboarding, Domain Join Method, Identity-First, Zero-Trust, CMDB-Governed Device Trust Orchestration
Reviewed by Copilot


# APilot: Secure Enterprise Onboarding and Domain Join Method
# Identity-First, Zero-Trust, CMDB-Governed Device Trust Orchestration

## 1. Purpose

APilot is a secure enterprise onboarding and domain join method for Windows devices operating across internal, remote, and cloud-managed environments. It defines an identity-first, zero-trust, CMDB-governed lifecycle for device onboarding, trust validation, operational activation, and recovery. It is intended to standardize how enterprise devices transition from an untrusted state to a trusted, compliant, and productive state across AD DS, Microsoft Entra ID, Intune, PKI, VPN, and enterprise compliance controls.

The method is designed to replace ad hoc or technician-dependent onboarding with a governed, auditable, automated workflow in which a device is not considered trusted or productive until identity, device posture, network context, certificate validity, and policy compliance have all been validated. This reduces human error, limits partial trust states, improves operational consistency, and strengthens supportability during onboarding, remediation, and rollback.

APilot is not a standalone Microsoft product. It is a Microsoft-aligned operating model that organizes native Microsoft capabilities into a single, enforceable lifecycle for secure onboarding and domain join. It applies across hybrid AD DS + Entra environments, cloud-only Entra deployments, server and VM onboarding, and remote device access scenarios where trust must be established before the endpoint becomes operational.

## 2. Scope

This method applies to enterprise-managed endpoints and server workloads, including laptops, desktops, physical servers, virtual servers, and virtual machines. It is designed to address mixed enterprise environments in which a device may join to AD DS, Entra ID, or a hybrid model depending on policy, device classification, network context, and operating model.

This method explicitly excludes phones and tablets due to different management lifecycles, identity models, and compliance enforcement patterns.

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
10. Human-error reduction through automation and standardization

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

This creates:

- redundant helpdesk effort
- inconsistent technician behavior
- onboarding drift
- security gaps
- partial trust states
- compliance exposure
- operational bottlenecks

APilot addresses these issues by shifting onboarding from a human-driven process to a governed trust lifecycle. The purpose is not to add another layer of bureaucracy to a working process, but to make trust explicit, validated, enforceable, and recoverable.

## 5. APilot Method Definition

APilot defines a structured enterprise method for secure onboarding and domain join that applies across all supported corporate computing assets, including laptops, desktops, physical servers, virtual servers, and virtual machines. The method establishes a controlled path from untrusted device to trusted, compliant, productive endpoint by enforcing validation at each stage of the lifecycle.

The method begins with device and user identity validation, followed by DIT collection, CMDB confirmation, network context analysis, and trust staging. If the device is remote or disconnected from the corporate network, APilot initiates a secure bootstrap process that validates reachability, installs the required trust components, and establishes the correct network path before the join action proceeds. Once trust conditions are satisfied, APilot selects the appropriate join model based on policy, environment, and device classification, then executes the join flow and applies the necessary compliance, certificate, and security baselines.

APilot is designed to ensure that the device cannot become productive until all required trust gates have passed. These gates include identity validation, device attestation, certificate validity, policy readiness, compliance state, and CMDB consistency. If any gate fails, the device is quarantined, rollback is initiated where possible, and the failure is recorded for support, governance, and audit purposes.

The method is intentionally operational. It does not assume that onboarding is a simple registration event. Instead, it implements a state-based lifecycle that makes trust transitions explicit, measurable, and recoverable. This creates a stronger operational model for enterprise onboarding, reduces technician variance, and provides clear evidence of device state throughout the lifecycle.

## 6. High-Level Architecture

### 6.1 APilot Web Portal

The APilot portal provides centralized orchestration for onboarding activities. It supports:

- MFA verification
- invitation and approval flow
- hostname confirmation
- onboarding session control
- certificate issuance workflow
- join mode initiation
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

The plugin is the execution element that translates policy into device actions. It ensures that device validation is performed before the device is allowed to continue in the lifecycle.

### 6.3 APilot Gateway

The APilot gateway provides the network and trust routing layer. It is responsible for:

- network context detection
- reachability validation
- certificate issuance routing
- join mode routing
- connectivity validation to AD DS, Entra ID, PKI, and CMDB

The gateway ensures that onboarding decisions are based on real operational context rather than assumptions about the device’s location or accessibility.

### 6.4 APilot Automation Engine

The automation engine is the orchestration component responsible for:

- join mode selection
- domain join execution
- Entra registration or hybrid join orchestration
- policy application
- rollback
- quarantine
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
- support and incident attributes

The CMDB is not a passive record. It is the operational source of truth for device lifecycle state and is used to validate whether the device is allowed to advance in the trust flow.

### 6.6 APilot PKI

The PKI model includes:

- temporary onboarding certificates
- permanent device certificates
- certificate templates
- revocation and renewal
- certificate-based trust for network and access validation

The PKI layer is essential to the method because trust cannot be established on device identity alone. Certificate state must be validated in relation to the join operation, the network context, and the compliance state.

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

The DIT is necessary because a simple device registration or inventory record is not sufficient evidence of trust. Device trust must be grounded in an identity record that includes the hardware, software, and policy context relevant to onboarding.

## 8. Security Model

### 8.1 Identity Security

The APilot process requires:

- MFA at critical steps
- user identity verification before trust is granted
- device and user validation before onboarding continues
- Conditional Access enforcement during trust stages

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

This ensures the device is not treated as trusted simply because it has a valid user session or has been registered in a management platform.

### 8.3 Certificate Security

APilot requires:

- temporary onboarding certificates for remote bootstrap and device trust staging
- permanent device certificates for final trust
- certificate validation before productivity
- revocation and renewal controls
- expiry and CRL/OCSP enforcement
- automatic quarantine on certificate failure

Certificate validation is necessary because a device may be joined and enrolled but still not be safe to use if its trust state, certificate lifecycle, or compliance posture is invalid.

### 8.4 Network Security

The method requires:

- secure network reachability validation
- VPN bootstrap where required
- identification of corporate vs non-corporate context
- validation of path to AD DS, Entra ID, PKI, and CMDB
- network-aware decisioning before join

This is essential in remote or hybrid environments in which a device may be unable to reach the required identity and trust services without first establishing a secure network path.

### 8.5 Operational Security

APilot requires:

- backend-driven privileged operations
- auditable operations
- no manual local admin execution by default
- restricted or sandboxed plugin execution
- rollback support
- explicit quarantine eventing

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
- rolled back

This state model reduces ambiguity and makes the process observable. Each state has a distinct operational meaning, and transition to the next stage requires a validation gate. This eliminates the common enterprise failure mode where a device appears to be “joined” but is not actually safe, compliant, or trusted.

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

## 11. Administrator Runbook

### 11.1 Administrator Responsibilities

Administrators are responsible for:

- APilot portal configuration
- onboarding approval
- CMDB validation
- PKI and certificate lifecycle
- policy and baseline management
- join mode approvals
- escalation and rollback
- quarantine handling
- operation monitoring

Administrators are the operational owners of the lifecycle and are responsible for validating that trust decisions are aligned with governance, policy, and identity posture.

### 11.2 Monitoring

Administrators monitor:

- onboarding session status
- plugin health
- DIT validation
- CMDB state
- certificate issuance
- VPN state
- policy status
- compliance results
- join results
- support ticket and incident state

Operational monitoring is necessary because device onboarding is not considered successful just because a path was executed. Success must be validated in operational terms: identity, posture, reachability, compliance, and service readiness.

### 11.3 Failure Handling

If any required step fails:

- device enters quarantine
- temporary cert is revoked or disabled
- VPN access is restricted
- user is notified
- incident is logged
- support ticket is created
- technician or admin is assigned
- rollback is initiated if possible

This is essential because failed onboarding must not leave a device in an ambiguous or partially trusted state.

### 11.4 Rollback Procedure

Rollback must restore the device to a safe state.

Rollback actions include:

- revoke temporary or final certificates
- remove trust state
- remove partial join state
- remove VPN profile if applicable
- clear or restore CMDB state
- remove plugin artifacts if required
- update audit record
- create incident trail

Rollback is a key design requirement. Without rollback, an onboarding failure can remain operationally dangerous even after the join attempt fails.

## 12. Technician Runbook

### 12.1 Technician Responsibilities

Technicians validate:

- physical device identity
- serial number
- BIOS UUID
- TPM presence
- OS edition and version
- disk health
- network adapter validation
- ability to communicate with required services
- no unsupported local workaround if not approved

Technicians are required to validate the device at the physical and operational level before the workflow proceeds.

### 12.2 User Assistance

Technicians may assist with:

- MFA issues
- plugin installation or reinstallation
- VPN authentication problems
- onboarding invitation issues
- reboot actions
- manual fallback steps
- support ticket documentation

### 12.3 Manual Fallback

If APilot automation fails at a critical stage, a technician may perform a controlled manual fallback.

Fallback rules:

- no local admin credential bypass unless explicitly approved
- all actions logged
- all failures recorded in the incident record
- admin approval required for exception scenarios
- no partial trust state allowed to remain active

Fallback actions must remain controlled. The objective is to recover safely, not to bypass governance.

### 12.4 Incident Documentation

Technicians must record:

- session ID
- device identity
- failure point
- relevant logs
- action taken
- time to resolution
- final state
- whether rollback or quarantine was used

Operational evidence is required for support continuity and for validating whether the onboarding lifecycle remains safe and governable.

## 13. Server and VM Onboarding

### 13.1 Purpose

Servers and virtual machines require a different onboarding flow because they do not follow the same user-driven pattern.

### 13.2 Scope

This track applies to:

- physical servers
- virtual servers
- virtual machines

### 13.3 Key Differences

The server track differs in several important ways:

- no user email onboarding
- no user MFA workflow
- no productivity app installation
- stricter certificate and policy requirements
- different baseline model
- admin-driven approval flow

This is necessary because server workloads require a stronger operational posture and higher governance thresholds than normal user endpoints.

### 13.4 Server Onboarding Sequence

1. CMDB validation
2. admin triggers onboarding
3. server receives plugin through admin portal
4. DIT collected
5. CMDB validation performed
6. admin approves hostname and role assignment
7. temporary certificate issued
8. VPN bootstrap if remote
9. join or registration executed
10. permanent certificate issued
11. required policies applied
12. CMDB updated
13. server enters compliant state

### 13.5 Server Policy Baseline

Server baseline includes:

- strict value assignments
- restricted local administration
- certificate enforcement
- no user productivity baseline
- no user app rollout
- strict compliance and security posture

Server workloads require a more controlled and policy-driven onboarding model because they are critical to operational continuity and are typically subject to stricter security, patch, and access controls.

## 14. Quarantine Handling

The device is placed in quarantine when:

- DIT mismatch exists
- CMDB mismatch exists
- temporary certificate fails
- permanent certificate fails
- VPN bootstrap fails
- join fails
- policy validation fails
- security anomaly is detected
- device is partially trusted

Quarantine actions:

- block continued onboarding
- restrict VPN
- block certificate retrieval
- block domain or cloud trust assignment
- notify admin
- notify user or service owner
- trigger support workflow
- maintain full audit record

Quarantine is a necessary control. A failed onboarding event must not leave a device in an ambiguous operating state. This is one of the principal operational safeguards required for enterprise adoption.

## 15. Compliance Verification

A device is considered compliant only when:

- user and device identity are validated
- DIT and CMDB are aligned
- join state is complete
- certificate state is valid
- policy baselines are applied
- security tools are installed and healthy
- no quarantine flags remain
- no drift is detected
- endpoint is ready for production access

This is the point at which the device transitions from “onboarded” to “ready to use.” Compliance is not a single event or a final state check; it is an operational condition that must be continuously validated in context.

## 16. CMDB Governance

### 16.1 CMDB Requirements

CMDB must store:

- DIT
- assigned user
- device type
- join mode
- compliance state
- certificate state
- join state
- policy state
- quarantine state
- service owner or department
- last seen and last updated timestamps

The CMDB is not a reporting archive. It is the authoritative state model that enables operational trust decisions.

### 16.2 CMDB Write-Back Events

APilot must write back the following events:

- device validated
- temporary certificate issued
- VPN connected
- domain join completed
- permanent certificate issued
- policies applied
- cleanup complete
- device compliant
- device quarantined
- rollback completed

These events provide the operational evidence necessary for correct support handling and for verifying that trust was granted only under valid conditions.

### 16.3 CMDB Governance Rules

- CMDB is authoritative
- no device may proceed to join without a valid CMDB entry
- drift or mismatch triggers quarantine
- CMDB is the source of truth for lifecycle state
- device history is retained for audit and support

Operational governance depends on authoritative records. Without this, support teams cannot determine whether the device state is accurate or whether the trust decision was valid.

## 17. PKI Design

### 17.1 Temporary Certificates

Temporary certificates are used for onboarding trust and remote connectivity. They are required to support early trust operations and network validation before final trusted state is established.

Requirements:

- short-lived
- non-exportable
- bound to DIT
- issued only after MFA and CMDB validation
- auto-revoked on failure

Recommended TTL:

- 15 minutes or an equivalent short-lived policy

Temporary certificates are designed to support rapid and controlled trust establishment, not long-term operational identity.

### 17.2 Permanent Certificates

Permanent certificates are used for:

- VPN access
- Wi-Fi access
- device trust
- Conditional Access decisions
- certificate-based validation of device health and compliance

Permanent certificates are issued only after the onboarding process passes the required trust gates. This allows the trust lifecycle to remain controlled and auditable.

### 17.3 PKI Infrastructure

The PKI model should support:

- certificate templates for temporary and permanent trust
- SCEP or equivalent enrollment design
- NDES or Intune PKI connector deployment if used
- template separation between onboarding and permanent trust
- certificate revocation and renewal automation
- integration with CMDB state tracking

PKI is a critical control layer. Without valid certificate lifecycle management, a secure onboarding method cannot be considered complete.

## 18. Balance of Automation and Human Intervention

APilot is intentionally designed to reduce human error by automating:

- DIT collection
- join path decision
- VPN bootstrap
- certificate issuance
- policy assignment
- compliance checks
- cleanup
- rollback
- CMDB write-back

Human intervention remains necessary for:

- exception handling
- asset or identity discrepancy review
- policy approval
- security review
- break-glass cases
- escalations

This reflects a modern enterprise operating model in which automation handles repeatable technical enforcement while human intervention remains focused on exceptions, governance, and escalation.

## 19. Supporting Official References

The APilot method is grounded in established enterprise and security principles reflected in official documentation from Microsoft, IBM, AWS, and NinjaOne. These references support the following ideas:

- identity-first access and trust decisions
- zero-trust verification before access is granted
- device identity and certificate-based trust
- compliance-before-access models
- endpoint health and operational telemetry
- automation and remediation in enterprise environments
- CMDB and ITSM integration for service and governance

### Microsoft Official Documentation

- Windows Autopilot hybrid join:
  https://learn.microsoft.com/en-us/autopilot/windows-autopilot-hybrid

- User-driven Microsoft Entra hybrid join workflow:
  https://learn.microsoft.com/en-us/autopilot/tutorial/user-driven/hybrid-azure-ad-join-workflow

- Intune Windows enrollment guide:
  https://learn.microsoft.com/en-us/intune/device-enrollment/windows/guide

- Windows enrollment methods:
  https://learn.microsoft.com/en-us/mem/intune/enrollment/windows-enrollment-methods

- Intune certificates configuration:
  https://learn.microsoft.com/en-us/mem/intune/protect/certificates-configure

- Intune compliance policies:
  https://learn.microsoft.com/en-us/mem/intune/protect/protect-devices

- Conditional Access overview:
  https://learn.microsoft.com/en-us/entra/identity/conditional-access/overview

- Microsoft Entra device join concepts:
  https://learn.microsoft.com/en-us/entra/identity/devices/concept-directory-join

### IBM Official Documentation

- IBM Zero Trust implementation guide:
  https://www.ibm.com/think/topics/zero-trust-implementation

- IBM Zero Trust solutions:
  https://www.ibm.com/solutions/zero-trust

- IBM Verify adaptive access:
  https://docs.verify.ibm.com/verify/docs/adaptive-access

### AWS Official Documentation

- AWS Zero Trust principles:
  https://docs.aws.amazon.com/prescriptive-guidance/latest/strategy-zero-trust-architecture/zero-trust-principles.html

- AWS Zero Trust components:
  https://docs.aws.amazon.com/prescriptive-guidance/latest/strategy-zero-trust-architecture/components.html

- AWS device manufacturing and provisioning with X.509 certificates:
  https://docs.aws.amazon.com/whitepapers/latest/device-manufacturing-provisioning/reference-architectures.html

- AWS IoT device onboarding guidance:
  https://docs.aws.amazon.com/iot-mi/latest/devguide/managedintegrations-sdk-v2-device-onboarding.html

### NinjaOne Documentation

- NinjaOne automation and remediation documentation:
  https://www.ninjaone.com/

- NinjaOne help center topics on automation, patching, monitoring, scripting, and ITSM integration:
  https://help.ninjaone.com/

These sources support the following enterprise principles:

- identity-first access and trust decisions
- zero-trust verification before access is granted
- device identity and certificate-based trust
- compliance-before-access models
- endpoint health and operational telemetry
- automation and remediation in enterprise environments
- CMDB and ITSM integration for service and governance

## 20. Final Positioning

APilot is a secure enterprise onboarding and domain join method designed to unify identity validation, device trust, certificate-based onboarding, compliance gating, and rollback in a single operational model.

It is not a standalone Microsoft product. It is a Microsoft-aligned operating model built on native Microsoft capabilities and supported by enterprise zero-trust principles used across modern endpoint and identity architectures.

The method is intended to reduce manual technician dependency, prevent partial trust states, improve onboarding consistency, and provide operational traceability for security, support, and governance teams.

## 21. Closing Statement

APilot defines a new operational model for enterprise onboarding and secure device trust. It is designed to close the gaps that remain after enrollment, registration, and policy assignment are complete.

The goal is straightforward:

- no untrusted device becomes productive
- no partial trust state remains hidden
- no device joins without identity, policy, compliance, and certificate validation
- no onboarding event is left without traceability

The result is a more secure, governable, and operationally consistent onboarding process that works across internal, remote, hybrid, and cloud-managed enterprise environments.
