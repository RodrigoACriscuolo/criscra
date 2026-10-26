# APilot: Secure Enterprise Onboarding and Domain Join Method
# Identity-First, Zero-Trust, CMDB-Governed Device Trust Orchestration

## 1. Purpose

This document defines a new enterprise onboarding and domain join method named APilot. APilot is intended to standardize the onboarding process for enterprise devices and server workloads across:

- internal corporate devices
- remote and external devices
- hybrid AD DS + Microsoft Entra ID environments
- cloud-only Entra ID environments
- physical servers
- virtual servers
- virtual machines
- legacy and mixed-trust networks

APilot is designed to replace inconsistent, human-dependent onboarding with a controlled, MFA-verified, certificate-based, CMDB-governed process that reduces operational risk and ensures that trust is only granted after identity, posture, compliance, and connectivity are validated.

This method is intentionally broad. It covers devices that require:

- AD DS domain join
- Microsoft Entra join
- hybrid join
- remote bootstrap
- server onboarding
- VM onboarding

Phones and tablets are excluded because they use different identity models, management lifecycles, and compliance enforcement patterns.

## 2. Scope

This method applies to:

- laptops
- desktops
- physical servers
- virtual servers
- virtual machines

It covers:

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
2. Device Identity Token (DIT) as the basis of trust
3. Zero Trust enforcement during onboarding
4. Network-aware routing and join decisions
5. Certificate-based trust
6. Policy-driven decisioning
7. CMDB as the authoritative state source
8. Observability and lifecycle traceability
9. Quarantine and rollback by design
10. Human-error reduction through automation

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

APilot addresses these issues by shifting onboarding from a human-driven process to a governed trust lifecycle.

## 5. APilot Method Definition

APilot is a secure enterprise onboarding and domain join method for enterprise devices operating across AD DS, Microsoft Entra ID, Intune, PKI, VPN, and enterprise compliance frameworks.

It defines a lifecycle from:

- untrusted device
- device identity validation
- trust staging
- join route selection
- certificate and policy enforcement
- compliant, productive state

The method is not tied to a single identity model. It supports:

- AD DS only environments
- hybrid AD DS + Entra ID environments
- cloud-only Entra ID environments
- remote or off-network devices
- server and VM workloads

The key principle is simple:

A device is not considered trusted or productive until identity, posture, network context, compliance state, and certificate validity are all validated.

## 6. High-Level Architecture

### 6.1 APilot Web Portal
The APilot portal provides:

- MFA verification
- invitation and approval flow
- hostname confirmation
- onboarding session control
- certificate issuance workflow
- join mode initiation
- monitoring and escalation

### 6.2 APilot Plugin
The APilot plugin is installed on the target device and performs:

- DIT collection
- CMDB validation
- OS compatibility and hardware validation
- VPN profile installation
- temporary certificate installation
- join execution
- cleanup and hardening
- post-onboarding validation

### 6.3 APilot Gateway
The APilot gateway provides:

- network context detection
- reachability validation
- certificate issuance routing
- join mode routing
- connectivity validation to AD DS, Entra ID, PKI, and CMDB

### 6.4 APilot Automation Engine
The automation engine is responsible for:

- join mode selection
- domain join execution
- Entra registration or hybrid join orchestration
- policy application
- rollback
- quarantine
- CMDB write-back
- audit and event recording

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

### 6.6 APilot PKI
The PKI model includes:

- temporary onboarding certificates
- permanent device certificates
- certificate templates
- revocation and renewal
- certificate-based trust for network and access validation

## 7. Device Identity Model

APilot depends on a Device Identity Token (DIT). The DIT is the minimum structured trust record required before onboarding can continue.

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

## 8. Security Model

### 8.1 Identity Security
The APilot process requires:

- MFA at critical steps
- user identity verification before trust is granted
- device and user validation before onboarding continues
- Conditional Access enforcement during trust stages

This aligns with Microsoft identity-first access principles and Zero Trust enforcement models used by modern enterprise identity and access architectures.

### 8.2 Device Security
The device must satisfy:

- DIT validation
- hardware and identity checks
- posture readiness
- baseline policy readiness
- certificate validation
- no quarantine status
- no partial trust state

### 8.3 Certificate Security
APilot requires:

- temporary onboarding certificates for remote bootstrap and device trust staging
- permanent device certificates for final trust
- certificate validation before productivity
- revocation and renewal controls
- expiry and CRL/OCSP enforcement
- automatic quarantine on certificate failure

### 8.4 Network Security
The method requires:

- secure network reachability validation
- VPN bootstrap where required
- identification of corporate vs non-corporate context
- validation of path to AD DS, Entra ID, PKI, and CMDB
- network-aware decisioning before join

### 8.5 Operational Security
APilot requires:

- backend-driven privileged operations
- auditable operations
- no manual local admin execution by default
- restricted or sandboxed plugin execution
- rollback support
- explicit quarantine eventing

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

This state model reduces ambiguity and makes the process observable.

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

### 11.2 Monitoring
Administrators monitor:

- onboarding session status
- plugin health
- DIT validation
- CMDB state
- certificate issuance state
- VPN state
- policy status
- compliance results
- join results
- support ticket and incident state

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

## 13. Server and VM Onboarding

### 13.1 Purpose
Servers and virtual machines require a different onboarding flow because they do not follow the same user-driven pattern.

### 13.2 Scope
This track applies to:

- physical servers
- virtual servers
- virtual machines

### 13.3 Key Differences

- no user email onboarding
- no user MFA workflow
- no productivity app installation
- stricter certificate and policy requirements
- different baseline model
- admin-driven approval flow

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

## 14. Quarantine Handling

A device enters quarantine when:

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

### 16.3 CMDB Governance Rules

- CMDB is authoritative
- no device may proceed to join without a valid CMDB entry
- drift or mismatch triggers quarantine
- CMDB is the source of truth for lifecycle state
- device history is retained for audit and support

## 17. PKI Design

### 17.1 Temporary Certificates
Temporary certificates are used for onboarding trust and remote connectivity.

Requirements:

- short-lived
- non-exportable
- bound to DIT
- issued only after MFA and CMDB validation
- auto-revoked on failure

Recommended TTL:

- 15 minutes or an equivalent short-lived policy

### 17.2 Permanent Certificates
Permanent certificates are used for:

- VPN access
- Wi-Fi access
- device trust
- Conditional Access decisions
- certificate-based validation of device health and compliance

### 17.3 PKI Infrastructure
The PKI model should support:

- certificate templates for temporary and permanent trust
- SCEP or equivalent enrollment design
- NDES or Intune PKI connector deployment if used
- template separation between onboarding and permanent trust
- certificate revocation and renewal automation
- integration with CMDB state tracking

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

## 19. Microsoft, IBM, AWS, and NinjaOne Support Model

The APilot method is built on established enterprise and security principles reflected in official documentation from Microsoft, IBM, AWS, and NinjaOne. These references support the following ideas:

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

This is the correct foundation for a new Microsoft-aligned onboarding method and a strong basis for enterprise adoption.
