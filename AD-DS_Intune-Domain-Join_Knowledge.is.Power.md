# APilot: Automated Domain Join and Device Enrollment Framework

This document was drafted and refined with assistance from GitHub Copilot to help structure a clear, practical, and enterprise-ready onboarding model for Windows devices. It is intended to support real operational use by IT, security, and engineering teams with a strong emphasis on identity validation, policy enforcement, and reducing human error during onboarding.

## Executive summary

APilot is a modern onboarding framework for device enrollment and domain join across legacy AD DS, hybrid AD DS + Entra ID, and cloud-only Entra ID environments. The design is built around one core principle: no device should be granted trust until its identity, hardware posture, security state, and compliance state are validated.

The framework brings together the critical requirements of modern enterprise operations:

- secure identity-first onboarding
- consistent device readiness checks
- automated domain join and certificate trust
- policy-driven compliance and auditability
- safe rollback and quarantine when the process fails

APilot is designed for real-world conditions where users may be remote, networks may be unreliable, and support teams must be able to intervene without compromising security or creating operational drift.

## Scope

APilot applies to:

- laptops and desktops
- physical servers and virtual machines
- hybrid and cloud-managed environments
- remote and on-premises onboarding scenarios

APilot explicitly excludes:

- phones and tablets
- unmanaged or personal devices
- environments that cannot validate identity or policy state

## Audience

This document is intended for:

- IT architects
- identity and access engineers
- endpoint engineering teams
- helpdesk and field technicians
- security operations teams
- administrators managing AD DS, Entra ID, Intune, PKI, and compliance

## Design goals

APilot is built to achieve the following:

- reduce onboarding complexity
- improve consistency across environments
- eliminate manual misconfiguration risk
- enforce strong identity validation before trust is granted
- support both remote and local onboarding flows
- integrate with CMDB and ITSM systems
- support rollback, quarantine, and safe recovery

## Core concept

APilot is more than a script or a join workflow. It is an operational model for how a device is validated, enrolled, trusted, and brought into a compliant state.

The model includes the following key services:

- APilot Web Portal: invitation management, MFA validation, and orchestration entry
- APilot Plugin: local device validation and endpoint setup tasks
- APilot Gateway: network posture detection and join routing decisions
- APilot Automation Engine: join orchestration and policy enforcement
- APilot CMDB: authoritative device identity and lifecycle state
- APilot PKI: temporary and permanent device certificate issuance
- Directory Services: AD DS and Entra ID
- Policy and compliance services: GPO, Intune, Conditional Access, and compliance evaluation

## Decision framework

The correct onboarding path depends on the device, environment, and user location.

| Join Type | Best For | Key Requirements | Management Model | Notes |
| --- | --- | --- | --- | --- |
| Modern AD DS join | Classic AD environments | Windows Pro/Ent/Edu, DC reachable | GPO, SCCM | Traditional domain join model |
| Hybrid Azure AD / Entra join | Enterprises using AD + cloud | AD DS + Entra ID + Entra Connect | GPO + Intune | Best for mixed environments |
| Entra ID join | Cloud-first organizations | Entra ID tenant | Intune | No AD DS dependency |
| Offline domain join | Imaging and provisioning | djoin.exe, offline bootstrap | GPO | Useful for mass provisioning |

### Join decision matrix

| Scenario | Recommended mode | Discussion |
| --- | --- | --- |
| Device is on corporate LAN and AD DS is authoritative | Legacy AD DS join | Use local AD trust model |
| Company uses AD DS and Microsoft 365 with cloud identity | Hybrid join | Use AD DS + Entra ID |
| Company is cloud-first and has no AD DS dependency | Entra ID join | Use Intune-driven enrollment |
| User is remote and has never joined the domain before | Remote onboarding flow | Use secure bootstrap, VPN, certificate, and join workflow |
| Server or VM needs privileged onboarding | APilot-Server flow | Different identity and policy model |

## Architectural principles

APilot follows seven core principles:

1. Identity-first onboarding
2. Device Identity Token (DIT) as the foundation of trust
3. Zero Trust enforcement
4. Network-aware routing decisions
5. Certificate-based trust
6. Vendor-neutral integration
7. Full observability and rollback

## High-level architecture

### 1. APilot Web Portal

Responsibilities:

- MFA authentication
- invitation validation
- hostname confirmation
- VPN bootstrap initiation
- certificate issuance requests
- join orchestration approval

### 2. APilot Plugin

Responsibilities:

- device inventory and hardware validation
- CMDB validation
- VPN package and temporary certificate installation
- join execution coordination
- cleanup and hardening tasks
- status reporting back to the control plane

### 3. APilot Gateway

Responsibilities:

- detect corporate vs non-corporate network context
- validate connectivity to AD DS, PKI, and CMDB
- decide join route
- issue short-lived onboarding trust tokens

### 4. APilot Automation Engine

Responsibilities:

- orchestrate the domain join and enrollment process
- apply GPO and Intune policies
- write state back to CMDB
- trigger rollback or quarantine when required

### 5. APilot CMDB

Responsibilities:

- authoritative device identity
- lifecycle state tracking
- assigned user mapping
- compliance state
- join mode and certificate state
- quarantine and recovery tracking

### 6. APilot PKI

Responsibilities:

- temporary onboarding certificates
- permanent device certificates
- certificate validation and revocation
- support for SCEP/NDES or Intune connectors

## Security model

### Identity security

- MFA is required for critical onboarding steps
- identity validation occurs before trust is granted
- Conditional Access is enforced during onboarding
- account and device trust are evaluated separately

### Device security

The Device Identity Token (DIT) should include:

- serial number
- BIOS UUID
- TPM identity
- hardware hash
- VM GUID, if applicable
- MAC addresses, used as secondary identifiers only
- operating system build and edition

The DIT must be treated as the device’s primary trust anchor before any certificate or join action is performed.

### Certificate security

Temporary onboarding certificates:

- short-lived, typically 15 minutes
- non-exportable
- bound to DIT
- issued only after MFA and CMDB validation
- revoked automatically when onboarding fails

Permanent certificates:

- issued after successful join and validation
- used for VPN, Wi-Fi, device trust, and Conditional Access
- managed through the standard certificate lifecycle process

### Network security

- VPN profiles are signed and validated
- pre-logon connectivity may be enabled when required
- cloud-only mode can skip VPN when appropriate
- routing decisions are based on validated connectivity and trust checks

### Operational security

- plugin execution should be constrained and auditable
- privileged actions should be orchestrated centrally, not by local user-operated scripts alone
- all join and policy events must be logged
- failures must trigger quarantine and review by an administrator

## APilot lifecycle phases

### Phase 1: Pre-registration

Purpose: bind device identity to the correct user and policy baseline.

Tasks:

- validate CMDB presence
- collect hardware identity
- assign join mode
- prepare OU structure, PKI, and VPN profile
- ensure policy baselines are ready

### Phase 2: User invitation and validation

Purpose: prove the correct user is acting on the correct device.

Tasks:

- issue invitation
- require MFA
- validate user UPN
- confirm device association

### Phase 3: Connectivity bootstrap

Purpose: establish secure connectivity before granting trust.

Tasks:

- install plugin
- validate DIT
- install temporary certificate
- install VPN package
- connect to VPN
- confirm network context

### Phase 4: Domain and identity trust

Purpose: perform the join and establish permanent trust.

Tasks:

- perform AD DS and/or Entra join
- issue permanent certificate
- validate policies
- apply compliance controls

### Phase 5: Cleanup and productivity setup

Purpose: complete the baseline and prepare the endpoint for use.

Tasks:

- harden OS
- remove unwanted applications
- install required business software
- finalize user login experience
- record successful completion in CMDB and ITSM

## Component model

### Core components

- User device
  - APilot Plugin
  - VPN client
  - certificate store
- APilot Web Portal
  - identity validation
  - plugin delivery
  - VPN bootstrap
- APilot Gateway
  - network detection
  - certificate issuance coordination
- APilot Automation Engine
  - join operations
  - policy enforcement
  - rollback logic
- CMDB
  - device identity
  - state tracking
- PKI
  - temporary and permanent certificates
- Directory Services
  - AD DS
  - Entra ID

## CMDB integration specification

### Requirements

The CMDB must contain:

- Device Identity Token (DIT)
- assigned user
- device type
- join mode
- compliance state
- certificate state
- domain-join state
- policy state
- quarantine state

### Write-back events

APilot should update the CMDB for:

- device validated
- temporary cert issued
- VPN connected
- join completed
- permanent cert issued
- policies applied
- cleanup completed
- device compliant
- device quarantined

### Governance

- the CMDB is the authoritative state store
- no device can proceed without a valid CMDB match
- drift or mismatch must trigger quarantine
- all state transitions must be auditable

## PKI design

### Temporary certificates

- valid for a limited time
- bound to DIT
- installed only after MFA and CMDB validation
- automatically revoked on failure
- used only for onboarding trust

### Permanent certificates

- issued after successful join and validation
- used for VPN, network access, and device trust
- renewed through the standard certificate lifecycle process

### Infrastructure

- SCEP/NDES or Intune PKI connector
- dedicated onboarding certificate template
- permanent trust certificate template
- CRL/OCSP reachability verification

## Server track specification

Servers and virtual machines require a different onboarding track because they do not follow the user-driven enrollment model.

### Server onboarding flow

1. administrator registers the server in the CMDB
2. administrator triggers APilot-Server onboarding
3. plugin is delivered through the admin portal
4. DIT is collected and validated
5. hostname is approved
6. temporary certificate is issued
7. VPN bootstrap occurs if remote
8. domain join is executed
9. permanent certificate is issued
10. server baseline policies are applied
11. CMDB is updated
12. server enters a compliant state

### Server policy model

- no user-based onboarding policy
- no productivity application baseline
- strict GPO baseline
- strict certificate requirements
- strict CMDB governance

## Roles and responsibilities

### Administrators

- manage the APilot portal
- approve device registrations
- validate CMDB entries
- monitor onboarding sessions
- handle escalations
- manage PKI and certificate lifecycle
- oversee domain join operations
- maintain baseline policies
- manage quarantine and rollback

### Technicians

- assist users during onboarding
- validate physical device identity
- ensure device readiness
- perform manual interventions when required
- execute server or VM onboarding flows
- document incidents and failures

## Pre-onboarding requirements

Before onboarding begins, administrators should ensure that the following are in place:

### CMDB record exists

Each device must have a record containing:

- DIT
- serial number
- BIOS UUID
- TPM identity
- VM GUID where applicable
- assigned user
- device type
- join mode
- compliance requirements
- certificate requirements

### AD DS OU structure prepared

```text
Corp.company.com
    /Users
    /Computers
    /Devices
        /CAN (Local)
            /Laptops
            /Desktops
            /Servers
            /VirtualMachines
```

### PKI ready

- onboarding template configured
- permanent certificate template configured
- SCEP/NDES or Intune connector operational
- CRL/OCSP reachable

### VPN infrastructure ready

- device tunnel profile configured
- user tunnel profile configured
- pre-logon support enabled when required
- signed configuration packages available

### Policy baselines ready

- GPO baseline for legacy or hybrid environments
- Intune baseline for cloud or hybrid environments
- Conditional Access policies configured
- compliance rules enabled

## User onboarding flow

### Step 1: Invitation sent

The administrator triggers an invitation through the APilot portal.

### Step 2: User opens the invitation on the target device

The invitation must be opened on the same device being onboarded.

### Step 3: MFA authentication

The user authenticates using:

- corporate email
- password
- MFA method such as Authenticator, SMS, or FIDO

### Step 4: Plugin download and installation

The portal delivers the APilot plugin. The plugin validates:

- DIT collection
- CMDB validation
- device readiness
- OS compatibility
- disk space
- network access

### Step 5: VPN bootstrap

The portal triggers VPN onboarding and installs a temporary certificate and client package.

### Step 6: VPN connection

The plugin verifies:

- corporate vs non-corporate network
- AD reachability
- PKI reachability
- CMDB connectivity

### Step 7: Join execution

APilot selects the proper path:

- cloud-only → Entra join
- hybrid → AD DS + Entra
- legacy → AD DS only

### Step 8: Permanent certificate issuance

The PKI service issues the permanent device certificate.

### Step 9: Policy application

- legacy or hybrid: GPO baseline applied
- cloud or hybrid: Intune baseline applied
- Conditional Access enforced
- compliance rules evaluated

### Step 10: Cleanup and hardening

The plugin removes unwanted software, installs required tooling, applies hardening, and finalizes the endpoint for use.

### Step 11: Final authentication

The user logs in with corporate credentials and MFA. The device is ready only after compliance checks pass.

## Operational runbook

### Administrator tasks

#### Monitor onboarding session

Administrators review:

- plugin status
- VPN status
- certificate issuance
- domain join logs
- policy application
- CMDB updates

#### Validate CMDB write-back

Confirm that the CMDB includes:

- validated device
- temporary cert issued
- VPN connected
- join completed
- permanent cert issued
- policies applied
- compliant state
- quarantine state when required

#### Handle failures

If any step fails:

- quarantine the device
- revoke temporary certificate
- disable VPN profile
- update CMDB
- notify user and technician
- review logs
- trigger rollback or restart

#### Rollback procedure

Rollback removes or resets:

- certificates
- VPN profiles
- domain join state
- plugin state
- CMDB state
- compliance state

## Technician runbook

### Validate physical device

Technician checks:

- serial number
- BIOS UUID
- TPM presence
- network adapters
- OS version
- hardware readiness

### Assist user

Technicians may help with:

- MFA issues
- plugin installation
- VPN authentication
- hostname confirmation
- reboots
- policy delays

### Manual fallback

If APilot automation is not available or fails, the technician may trigger a manual join through the APilot backend while the plugin continues to manage certificates and cleanup.

### Incident documentation

The technician logs:

- device identity
- failure point
- actions taken
- outcome
- time to onboard

## Troubleshooting guide

### MFA failure

- reset MFA
- validate identity provider state
- check network connectivity

### Plugin failure

- check OS version
- check disk space
- check antivirus interference
- reinstall plugin

### VPN failure

- validate temporary cert
- validate VPN profile
- review firewall and network restrictions

### Domain join failure

- verify DC reachability
- verify DNS health
- validate CMDB record and DIT match
- validate certificate chain and time sync
- trigger rollback

### Policy failure

- force GPO update
- force Intune sync
- confirm compliance rules are valid

## Quarantine handling

A device should enter quarantine when:

- CMDB mismatch occurs
- certificate issuance fails
- VPN connection fails
- domain join fails
- policy enforcement fails
- security anomaly is detected

Quarantine actions include:

- block domain join
- block certificate issuance
- block VPN access
- notify admin and user
- require technician intervention
- require review before authorization is restored

## Compliance verification

A device is considered compliant only when:

- join completes successfully
- the permanent certificate is installed
- policies are applied
- CMDB is updated
- Conditional Access passes
- no quarantine flags remain
- security tools are installed
- Office is installed
- OS hardening is complete

## APilot step-by-step operational flow

The following flow reflects the operational sequence the automation should implement. It prioritizes consistency, validation, and reduction of human error.

### 01 - Start APilot interactive session

Initialize the onboarding script and validate technician identity.

Required inputs:

- ITSM ticket ID
- user UPN
- device type
- technician session identity

### 02 - Validate CMDB record and DIT

Before any join action occurs, validate the device in the CMDB and collect the DIT.

```powershell
function Get-DeviceIdentityToken {
    [CmdletBinding()]
    param()

    $bios = Get-CimInstance -ClassName Win32_BIOS -ErrorAction Stop
    $system = Get-CimInstance -ClassName Win32_ComputerSystemProduct -ErrorAction Stop
    $os = Get-CimInstance -ClassName Win32_OperatingSystem -ErrorAction Stop
    $adapters = @(Get-NetAdapter -ErrorAction SilentlyContinue |
        Where-Object { $_.Status -eq 'Up' } |
        Select-Object -ExpandProperty MacAddress)

    $tpm = $null
    try {
        $tpm = Get-CimInstance -Namespace 'root\cimv2\security\microsofttpm' -ClassName Win32_Tpm -ErrorAction Stop
    }
    catch {
        $tpm = $null
    }

    return [pscustomobject]@{
        SerialNumber = $bios.SerialNumber
        BiosUuid     = $system.UUID
        MacAddresses = $adapters
        TpmPresent   = [bool]$tpm
        OsCaption    = $os.Caption
        OsVersion    = $os.Version
        CollectedAt  = (Get-Date).ToString('o')
    }
}
```

Validation checks:

- DIT must be complete
- DIT must match the CMDB entry
- the device must be assigned to the correct user
- mismatches must stop onboarding and trigger quarantine

### 03 - Trigger invitation and portal link

If there is no valid existing invitation, create one bound to:

- user UPN
- DIT
- ticket ID

The invitation should be logged to the backend and CMDB.

### 04 - Confirm MFA and portal authentication

Check that the user has completed MFA and portal authentication.

Recommended practices:

- enforce a timeout
- stop the process if MFA is not completed
- log failures to ITSM and CMDB

### 05 - Deploy the APilot plugin

Install the plugin silently and validate successful execution.

```powershell
function Install-ApilotPlugin {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [string]$DownloadUrl,

        [Parameter()]
        [string]$InstallArgs = '/qn /norestart'
    )

    $tempDir = Join-Path $env:TEMP 'APilot'
    New-Item -ItemType Directory -Force -Path $tempDir | Out-Null

    $packagePath = Join-Path $tempDir 'APilotPlugin.msi'
    Invoke-WebRequest -Uri $DownloadUrl -OutFile $packagePath -UseBasicParsing

    $process = Start-Process -FilePath 'msiexec.exe' -ArgumentList "/i `"$packagePath`" $InstallArgs" -Wait -PassThru -NoNewWindow

    if ($process.ExitCode -ne 0) {
        throw "APilot plugin installation failed with exit code $($process.ExitCode)."
    }

    return $true
}
```

### 06 - Re-validate DIT after plugin install

The plugin should perform a deeper inventory and verify the endpoint before trust is granted.

### 07 - Bootstrap VPN package and temporary certificate

The system should:

- install the temporary certificate into LocalMachine\My
- install the signed VPN client
- validate installation
- log the outcome

### 08 - Connect VPN and detect network context

The automation should perform network validation and decide whether to use:

- CloudOnly
- Hybrid
- Legacy

This phase must confirm that the endpoint can reach:

- AD DS
- PKI
- CMDB
- required policy services

### 09 - Execute domain join

The automation should trigger join orchestration through the backend, not by manually running local privileged commands.

```powershell
function Invoke-ApilotJoin {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [pscustomobject]$DeviceIdentity,

        [Parameter(Mandatory)]
        [ValidateSet('CloudOnly', 'Hybrid', 'Legacy')]
        [string]$JoinMode,

        [Parameter()]
        [string]$HostName = $env:COMPUTERNAME
    )

    $payload = [ordered]@{
        serialNumber = $DeviceIdentity.SerialNumber
        biosUuid     = $DeviceIdentity.BiosUuid
        hostName     = $HostName
        joinMode     = $JoinMode
        timestamp    = (Get-Date).ToString('o')
    }

    # Placeholder for a real backend API call
    # Invoke-RestMethod -Method Post -Uri 'https://apilot-backend.local/api/join/execute' -Body ($payload | ConvertTo-Json)

    Write-Host "Join orchestration invoked for mode '$JoinMode'."
    return $true
}
```

### 10 - Issue permanent certificate

After successful join, request and install the permanent device certificate. Remove the temporary certificate only after validation completes.

### 11 - Apply policies and compliance controls

The automation should enforce:

- GPO baseline for AD-based environments
- Intune baseline for hybrid and cloud-managed environments
- Conditional Access rules
- device compliance evaluation

### 12 - Perform cleanup and OS hardening

The script should:

- remove OEM bloatware
- uninstall stale Office versions
- install required business software
- deploy approved security agents
- apply the OS hardening baseline
- schedule or perform reboot

### 13 - Final reboot and compliance verification

The device should only be marked compliant after:

- domain membership is validated
- certificate presence is confirmed
- policy state is verified
- security tools are active
- user is ready to sign in

### 14 - Notify user and close ticket

Send a final onboarding email and attach a completion checklist to the ticket.

## Production-ready PowerShell skeleton

The following PowerShell skeleton is written to reduce common human error. It uses explicit validation, consistent logging, centralized API calls, and a safe failure path.

```powershell
# APilot.psm1
# Production-oriented skeleton for APilot onboarding automation.

$ErrorActionPreference = 'Stop'
Set-StrictMode -Version Latest

$script:APilotConfig = @{
    ApiBaseUrl      = 'https://apilot-backend.local/api'
    CMDBBaseUrl     = 'https://cmdb.local/api'
    ITSMBaseUrl     = 'https://itsm.local/api'
    PKIBaseUrl      = 'https://pki.local/api'
    LogPath         = 'C:\APilot\Logs\apilot.log'
    VPNProfileName  = 'APilot-VPN'
    MaxMfaWait      = 1800
    JoinTimeout     = 900
}

function Write-APilotLog {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [string]$Message,

        [ValidateSet('INFO', 'WARN', 'ERROR', 'DEBUG')]
        [string]$Level = 'INFO'
    )

    $timestamp = Get-Date -Format 'yyyy-MM-dd HH:mm:ss'
    $line = "[$timestamp] [$Level] $Message"

    try {
        New-Item -Path (Split-Path $script:APilotConfig.LogPath -Parent) -ItemType Directory -Force | Out-Null
        Add-Content -Path $script:APilotConfig.LogPath -Value $line
    }
    catch {
        Write-Host $line -ForegroundColor Yellow
    }

    Write-Host $line
}

function Invoke-APilotApi {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [ValidateSet('GET', 'POST', 'PUT', 'DELETE')]
        [string]$Method,

        [Parameter(Mandatory)]
        [string]$Endpoint,

        [Parameter()]
        [object]$Body
    )

    $url = "$($script:APilotConfig.ApiBaseUrl)$Endpoint"
    Write-APilotLog -Message "Calling APilot API: $Method $url" -Level DEBUG

    try {
        if ($null -ne $Body) {
            $json = $Body | ConvertTo-Json -Depth 10
            return Invoke-RestMethod -Method $Method -Uri $url -Body $json -ContentType 'application/json' -ErrorAction Stop
        }

        return Invoke-RestMethod -Method $Method -Uri $url -ErrorAction Stop
    }
    catch {
        $message = $_.Exception.Message
        Write-APilotLog -Message "APilot API call failed: $message" -Level ERROR
        throw
    }
}

function Test-IsAdministrator {
    [CmdletBinding()]
    param()

    $identity = [Security.Principal.WindowsIdentity]::GetCurrent()
    $principal = New-Object Security.Principal.WindowsPrincipal($identity)
    return $principal.IsInRole([Security.Principal.WindowsBuiltInRole]::Administrator)
}

function Test-RequiredEnvironment {
    [CmdletBinding()]
    param()

    if (-not (Test-IsAdministrator)) {
        throw 'APilot onboarding must run as Administrator.'
    }

    if (-not (Get-Command Invoke-RestMethod -ErrorAction SilentlyContinue)) {
        throw 'PowerShell REST support is unavailable.'
    }

    return $true
}

function Get-DeviceIdentityToken {
    [CmdletBinding()]
    param()

    $bios = Get-CimInstance -ClassName Win32_BIOS -ErrorAction Stop
    $computerSystem = Get-CimInstance -ClassName Win32_ComputerSystemProduct -ErrorAction Stop
    $os = Get-CimInstance -ClassName Win32_OperatingSystem -ErrorAction Stop
    $adapters = @(Get-NetAdapter -ErrorAction SilentlyContinue |
        Where-Object { $_.Status -eq 'Up' } |
        Select-Object -ExpandProperty MacAddress)

    $tpm = $null
    try {
        $tpm = Get-CimInstance -Namespace 'root\cimv2\security\microsofttpm' -ClassName Win32_Tpm -ErrorAction Stop
    }
    catch {
        $tpm = $null
    }

    return [pscustomobject]@{
        SerialNumber = $bios.SerialNumber
        BiosUuid     = $computerSystem.UUID
        MacAddresses = $adapters
        TpmPresent   = [bool]$tpm
        OsCaption    = $os.Caption
        OsVersion    = $os.Version
        CollectedAt  = (Get-Date).ToString('o')
    }
}

function Invoke-DITValidation {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [pscustomobject]$Dit
    )

    $response = Invoke-APilotApi -Method POST -Endpoint '/device/validate' -Body $Dit
    if ($response.status -ne 'ok') {
        throw 'DIT validation failed.'
    }

    return $true
}

function Invoke-CMDBCheck {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [pscustomobject]$Dit
    )

    $response = Invoke-APilotApi -Method GET -Endpoint "/cmdb/device?serial=$($Dit.SerialNumber)"
    if (-not $response -or $response.status -ne 'found') {
        throw 'CMDB record not found or invalid for this device.'
    }

    return $true
}

function Request-TemporaryCertificate {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [pscustomobject]$Dit
    )

    $response = Invoke-APilotApi -Method POST -Endpoint '/cert/temp/issue' -Body $Dit
    if ($response.status -ne 'ok') {
        throw 'Temporary certificate issuance failed.'
    }

    return $true
}

function Request-PermanentCertificate {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [pscustomobject]$Dit
    )

    $response = Invoke-APilotApi -Method POST -Endpoint '/cert/permanent/issue' -Body $Dit
    if ($response.status -ne 'ok') {
        throw 'Permanent certificate issuance failed.'
    }

    return $true
}

function Invoke-DomainJoin {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [pscustomobject]$Dit,

        [Parameter(Mandatory)]
        [ValidateSet('CloudOnly', 'Hybrid', 'Legacy')]
        [string]$JoinMode
    )

    $payload = [ordered]@{
        device = $Dit
        joinMode = $JoinMode
        timestamp = (Get-Date).ToString('o')
    }

    $response = Invoke-APilotApi -Method POST -Endpoint '/domain/join' -Body $payload
    if ($response.status -ne 'ok') {
        throw 'Domain join failed.'
    }

    return $true
}

function Update-CMDBState {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [pscustomobject]$Dit,

        [Parameter(Mandatory)]
        [ValidateSet('Validated', 'Quarantined', 'Compliant', 'Failed')]
        [string]$State
    )

    $payload = [ordered]@{
        device = $Dit
        state = $State
        timestamp = (Get-Date).ToString('o')
    }

    $response = Invoke-APilotApi -Method POST -Endpoint '/cmdb/update' -Body $payload
    if ($response.status -ne 'ok') {
        throw "CMDB state update to '$State' failed."
    }

    return $true
}

function Invoke-APilotOnboarding {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [string]$UserUpn,

        [Parameter(Mandatory)]
        [string]$TicketId,

        [Parameter(Mandatory)]
        [ValidateSet('Laptop', 'Desktop', 'Server', 'VM')]
        [string]$DeviceType,

        [Parameter(Mandatory)]
        [ValidateSet('CloudOnly', 'Hybrid', 'Legacy')]
        [string]$JoinMode
    )

    Test-RequiredEnvironment | Out-Null
    $dit = Get-DeviceIdentityToken

    try {
        Invoke-DITValidation -Dit $dit
        Invoke-CMDBCheck -Dit $dit
        Update-CMDBState -Dit $dit -State 'Validated'

        $ticket = [ordered]@{
            ticketId   = $TicketId
            userUpn   = $UserUpn
            deviceType = $DeviceType
            joinMode   = $JoinMode
        }

        Invoke-APilotApi -Method POST -Endpoint '/session/start' -Body $ticket | Out-Null
        Request-TemporaryCertificate -Dit $dit | Out-Null
        Invoke-DomainJoin -Dit $dit -JoinMode $JoinMode | Out-Null
        Request-PermanentCertificate -Dit $dit | Out-Null
        Update-CMDBState -Dit $dit -State 'Compliant' | Out-Null

        Write-APilotLog -Message 'Onboarding completed successfully.' -Level INFO
        return $true
    }
    catch {
        Write-APilotLog -Message "Onboarding failed: $($_.Exception.Message)" -Level ERROR
        try {
            Update-CMDBState -Dit $dit -State 'Quarantined' | Out-Null
        }
        catch {
            Write-APilotLog -Message 'Unable to update CMDB to Quarantined.' -Level WARN
        }

        throw
    }
}

Export-ModuleMember -Function * -Alias *
```

## Operational recommendations

To reduce human error and improve outcomes, the automation should include the following safeguards:

- require administrator rights for all onboarding operations
- require DIT validation before join
- check for time drift before certificate issuance
- enforce an explicit CMDB match before continuing
- prevent duplicate onboarding sessions for the same device
- track each transition in logs and CMDB state
- require rollback on any failed join or cert operation
- require final compliance validation before marking the device ready
- require technician sign-off for edge-case or exception scenarios

## Glossary

- AD DS: Active Directory Domain Services
- DIT: Device Identity Token
- CMDB: Configuration Management Database
- Entra ID: Microsoft Entra ID
- GPO: Group Policy Object
- Intune: Microsoft Intune device management
- MDM: Mobile Device Management
- OU: Organizational Unit
- PKI: Public Key Infrastructure
- VPN: Virtual Private Network
- Quarantine: temporary hold state preventing trust and access

## Final notes

APilot is intended to be a disciplined operational method, not a one-off script. Real-world execution requires clear validation gates, strong identity controls, and reliable rollback logic. The objective is not simply to automate a device join; the objective is to make onboarding secure, consistent, and easy to audit.

This framework aligns well with Microsoft-centric environments such as AD DS, Entra ID, Intune, and Windows-based endpoint management while staying vendor-neutral in its principles.

This document should be treated as a practical blueprint for an enterprise onboarding architecture and can be adapted to local tooling, automation platforms, or custom ITSM workflows.
