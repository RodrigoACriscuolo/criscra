AD-DS_Intune-Domain-Join_Knowledge.is.power
# Hybrid Azure AD Join + Intune enrollment
# Centralized AD identity
# Cloud security (Conditional Access, MFA, compliance)
# Modern device Management (old SCCM?)
# Seamless SSO across Microsoft 365 and on-prem resources
```Quick Comparison Table
| Join Type                        | Best For                     | Requirements                      | Management   |
| ---                              | ---                          | ---                               | ---          |
| **Modern AD DS Join (Settings)** | Classic AD environments      | Windows Pro/Ent/Edu, DC reachable | GPO, SCCM    |
| **Hybrid Azure AD Join**         | Enterprises using AD + cloud | AD DS + Entra ID + AAD Connect    | GPO + Intune |
| **Azure AD / Entra ID Join**     | Cloud-only environments      | Entra ID tenant                   | Intune       |
| **Offline Domain Join**          | Imaging, provisioning        | djoin.exe                         | GPO          |
```
# Domain-join new Method
# ** if needs support a brand new device when the user is remote, outside the corporate network, and has never connected to the domain before.
# Method
# Phase 1: Pre-Registration - Purpose: Bind device identity (serial/ MAC) to user and policies.
# Phase 2: User invitation & Validation - Purpose: Prove the right human is using the right device.
# Phase 3: Connectivity bootstrap - Purpose: Get secure VPN + cloud connectivity in place.
# Phase 4: Domain & identity trust - Purpose: Join AD DS/ Entra, Issue certs, enforce policies.
# Phase 5: Cleanup & productivity setup - Purpose: Harden OS, install Office, finalize user environment.
# I will call this idea "Auto-Pilot", "AutoPilot" or "APilot"

APilot — Automated Pilot Domain-Join Methodology
A Zero-Trust, Identity-First, Vendor-Neutral, Multi-Environment Onboarding Architecture

1. APilot Architecture Blueprint
1.1 Purpose
APilot defines a modern, automated, identity-first domain-join protocol for all corporate computing assets (laptops, desktops, physical servers, virtual servers, virtual machines). It replaces traditional domain-join processes with a controlled, MFA-verified, certificate-based, CMDB-governed onboarding flow that works across:
•	Legacy AD DS
•	Hybrid AD DS + Entra ID
•	Cloud-only Entra ID
Phones and tablets are explicitly excluded.

1.2 Architectural Principles
APilot is built on seven core principles:
1.	Identity-first onboarding
2.	Device Identity Token (DIT) — multi-attribute device identity
3.	Zero Trust enforcement
4.	Network-aware branching (corporate vs non-corporate)
5.	Certificate-based device trust
6.	Vendor-neutral integration
7.	Full observability and rollback

1.3 High-Level Architecture Components
APilot Web Portal
•	MFA authentication
•	Invitation validation
•	Hostname confirmation
•	VPN bootstrap
•	Certificate issuance
•	Domain-join orchestration
APilot Plugin (Agent)
•	Device inventory (DIT)
•	CMDB validation
•	VPN package installation
•	Temporary certificate installation
•	Domain-join execution
•	Cleanup and hardening
APilot Gateway
•	Network context detection
•	Connectivity validation
•	Certificate issuance
•	Domain-join routing (legacy/hybrid/cloud)
APilot Automation Engine
•	Domain-join orchestration
•	Policy application (GPO + Intune)
•	CMDB write-back
•	Rollback and quarantine
APilot CMDB
•	Authoritative device identity
•	Device state tracking
•	Compliance and lifecycle management
APilot PKI
•	Temporary onboarding certificates
•	Permanent device certificates
•	Revocation and renewal
2. APilot Security Model
2.1 Identity Security
•	MFA required at all critical steps
•	Identity verification before device trust
•	Conditional Access enforced during onboarding
2.2 Device Security
•	Device Identity Token (DIT) includes:
o	Serial number
o	BIOS UUID
o	TPM identity
o	Hardware hash (if available)
o	VM GUID
o	MAC addresses (secondary only)
2.3 Certificate Security
•	Temporary onboarding certificates:
o	15-minute TTL
o	Non-exportable
o	Bound to DIT
o	Auto-revoked on failure
•	Permanent certificates:
o	Issued post-domain-join
o	Used for VPN, Wi-Fi, device trust
2.4 Network Security
•	VPN profiles signed and validated
•	Pre-logon tunnels supported
•	Cloud-only mode skips VPN entirely
2.5 Operational Security
•	Plugin sandboxed
•	Privileged operations backend-driven
•	Full audit logging
•	Quarantine on failure
3. APilot Sequence Diagram (Textual Representation)
Actors
•	User
•	APilot Web Portal
•	APilot Plugin
•	APilot Gateway
•	APilot Automation Engine
•	CMDB
•	PKI
•	AD DS / Entra ID
•	VPN Infrastructure

Sequence Flow
1.	User receives invitation
2.	User opens portal → MFA
3.	Portal delivers plugin
4.	Plugin collects DIT → CMDB validation
5.	Portal triggers VPN onboarding
6.	Plugin installs temp cert + VPN client
7.	User authenticates VPN
8.	Gateway detects network context
9.	Automation Engine selects join mode
10.	Domain join executed
11.	Permanent cert issued
12.	Policies applied (GPO + Intune)
13.	Cleanup + hardening
14.	CMDB updated
15.	Device enters compliant state

4. APilot Component Diagram (Textual Representation)
Core Components
•	User Device
	o	APilot Plugin
	o	VPN Client
	o	Certificates
•	APilot Web Portal
	o	Identity verification
	o	Plugin delivery
	o	VPN bootstrap
•	APilot Gateway
	o	Network detection
	o	Certificate issuance
•	APilot Automation Engine
	o	Domain join
	o	Policy application
	o	Rollback
•	CMDB
	o	Device identity
	o	State tracking
•	PKI
	o	Temporary certs
	o	Permanent certs
•	Directory Services
	o	AD DS
	o	Entra ID

5. APilot CMDB Integration Specification
5.1 CMDB Requirements
CMDB must store:
•	Device Identity Token (DIT)
•	Assigned user
•	Device type
•	Join mode (legacy/hybrid/cloud)
•	Compliance state
•	Certificate state
•	Domain-join state
•	Policy state
5.2 CMDB Write-Back Events
APilot writes back:
•	Device validated
•	Temporary cert issued
•	VPN connected
•	Domain join completed
•	Permanent cert issued
•	Policies applied
•	Cleanup completed
•	Device compliant
•	Device quarantined (if failure)
5.3 CMDB Governance
•	CMDB is authoritative
•	No device can join domain without CMDB match
•	CMDB drift triggers quarantine

6. APilot PKI Design
6.1 Temporary Certificates
•	Short-lived (15 minutes)
•	Non-exportable
•	Bound to DIT
•	Issued only after MFA + CMDB match
•	Auto-revoked on failure

6.2 Permanent Certificates
•	Issued post-domain-join
•	Used for:
	o	VPN
	o	Wi-Fi
	o	Device trust
	o	Conditional Access

6.3 PKI Infrastructure
•	SCEP/NDES or Intune PKI connector
•	Certificate templates for onboarding and permanent trust
•	Revocation lists updated automatically

7. APilot Server Track Specification
7.1 Purpose
Servers and VMs require a different onboarding track:
•	No user email
•	No MFA
•	No Office installation
•	No user-driven flow

7.2 Server Onboarding Flow
1.	Admin registers server in CMDB
2.	Admin triggers APilot-Server onboarding
3.	Server receives plugin via admin portal
4.	Plugin collects DIT
5.	CMDB validation
6.	Admin approves hostname
7.	Temporary cert issued
8.	VPN bootstrap (if remote)
9.	Domain join executed
10.	Permanent cert issued
11.	Policies applied (server baseline)
12.	CMDB updated
13.	Server enters compliant state

7.3 Server Policies
•	No user policies
•	No productivity apps
•	Strict GPO baseline
•	Strict certificate requirements
•	Strict CMDB governance




1. Roles & Responsibilities
1.1 Administrators
Manage APilot Portal
Approve device registrations
Validate CMDB entries
Monitor onboarding sessions
Handle escalations
Manage PKI and certificate lifecycle
Oversee domain join operations
Maintain GPO/Intune baselines
Manage quarantine and rollback

1.2 Technicians
Assist users during onboarding
Validate physical device identity
Ensure device readiness
Perform manual interventions when required
Execute server/VM onboarding
Document incidents and failures

2. Pre‑Onboarding Requirements (Admin Tasks)
Before onboarding begins, administrators must ensure:

2.1 CMDB Record Exists
Each device must have a CMDB entry containing:
Device Identity Token (DIT)
Serial number
BIOS UUID
TPM identity (if applicable)
VM GUID (if applicable)
Assigned user
Device type
Join mode (legacy/hybrid/cloud)
Compliance requirements
Certificate requirements

2.2 AD DS OU Structure Prepared
Ensure the following OU structure exists:

```Forest
Corp.company.com
    /Users
    /Computers
    /Devices
        / CAN (Local)
            /Laptops
            /Desktops
            /Servers
            /VirtualMachines

```

2.3 PKI Ready
Temporary certificate template configured
Permanent certificate template configured
SCEP/NDES or Intune PKI connector operational
CRL/OCSP reachable

2.4 VPN Infrastructure Ready
Device tunnel profile
User tunnel profile
Pre-logon support (if required)
Signed configuration packages

2.5 Policy Baselines Ready
GPO baseline for legacy/hybrid
Intune baseline for cloud/hybrid
Conditional Access policies
Compliance rules

3. User Onboarding Flow (Technician + User)
Step 1 — Invitation Sent (Admin)
Admin triggers APilot invitation via portal.

Step 2 — User Opens Invitation on Target Device
User must open the email on the device being onboarded.

Step 3 — MFA Authentication
User authenticates with:
    - Email
    - Password
    - MFA (Authenticator, SMS, FIDO, etc.)

Step 4 — Plugin Download & Install
Portal delivers APilot Plugin.
Plugin performs:
    - DIT collection
    - CMDB validation
    - Device readiness checks
    - OS compatibility checks
    - Disk space checks
    - Network checks

Step 5 — VPN Bootstrap
Portal initiates VPN onboarding:
    - Hostname confirmation
    - User identity confirmation
    - VPN package download
    - Temporary certificate download
    - Automatic installation

Step 6 — VPN Connection
User authenticates to VPN.
Plugin verifies:
    - Corporate network vs non-corporate
    - Directory reachability
    - PKI reachability
    - CMDB connectivity

Step 7 — Domain Join Execution
APilot Automation Engine selects join mode:
    - Cloud-only → Entra Join
    - Hybrid → AD DS + Entra
    - Legacy → AD DS only
    - Plugin executes join commands via backend APIs.

Step 8 — Permanent Certificate Issuance
PKI issues permanent device certificate.

Step 9 — Policy Application
GPO baseline applied (legacy/hybrid)

Intune baseline applied (cloud/hybrid)

Conditional Access enforced

Compliance rules evaluated

Step 10 — Cleanup & Hardening
Plugin performs:
    - Removal of OEM apps
    - Removal of old Office versions
    - OS hardening
    - Installation of latest Office
    - Installation of security tools
    - Final reboot

Step 11 — Final User Authentication
User logs in with:
    - Email
    - Password
    - MFA

Device enters compliant state.

4. Administrator Runbook/ Guide (Backend Operations)
4.1 Monitor Onboarding Session
Admins monitor:
    - Plugin status
    - VPN status
    - Certificate issuance
    - Domain join logs
    - Policy application
    - CMDB updates

4.2 Validate CMDB Write-Back
Ensure CMDB reflects:
    - Device validated
    - Temporary cert issued
    - VPN connected
    - Domain join completed
    - Permanent cert issued
    - Policies applied
    - Device compliant
    - Quarantine (if applicable)

4.3 Handle Failures
If any step fails:
    - Device is quarantined
    - Temporary cert revoked
    - VPN profile disabled
    - CMDB updated
    - User notified
    - Technician assigned
    - Admin reviews logs
    - Admin triggers rollback or restart

4.4 Rollback Procedure
Rollback resets:
    - Certificates
    - VPN profiles
    - Domain join state
    - Plugin state
    - CMDB state
    - Compliance state
    - Device returns to pre-onboarding state.

5. Technician Runbook/ Guide (Hands-On Operations)
5.1 Validate Physical Device
Technician checks:
    - Serial number
    - BIOS UUID
    - TPM presence
    - Network adapters
    - OS version
    - Hardware readiness

5.2 Assist User
Technician may assist with:
    - MFA issues
    - Plugin installation
    - VPN authentication
    - Hostname confirmation
    - Reboots
    - Policy application delays

5.3 Manual Domain Join (Fallback)
If APilot automation fails:
    - Technician uses APilot backend to trigger manual join
    - Plugin still handles certs and cleanup
    - No local domain admin credentials used

5.4 Document Incident
Technician logs:
    - Device identity
    - Failure point
    - Actions taken
    - Resolution
    - Time to onboard

6. Server/VM Onboarding Runbook/ Guide (Admin Only)
6.1 No User Email
Servers use APilot-Server track, not APilot-User.

6.2 Admin Initiates Onboarding
Admin triggers server onboarding via portal.

6.3 Plugin Delivered via Admin Portal
Plugin collects:
    - DIT
    - CMDB match
    - Server readiness

6.4 Temporary Certificate Issued
Admin approves hostname.

6.5 VPN Bootstrap (If Remote)
Server connects via device tunnel.

6.6 Domain Join Executed
Automation Engine performs:
    - AD DS join
    - Entra registration (if hybrid)
    - Permanent cert issuance
    - Server baseline policies

6.7 CMDB Updated
Server enters compliant state.

7. Troubleshooting Guide
7.1 MFA Failure
Reset MFA
Validate identity provider
Check network connectivity

7.2 Plugin Failure
Check OS version
Check disk space
Check antivirus interference
Reinstall plugin

7.3 VPN Failure
Validate temporary cert
Validate VPN profile
Check network restrictions
Check firewall rules

7.4 Domain Join Failure
Check DC reachability
Check DNS
Check CMDB match
Check certificate validity
Trigger rollback

7.5 Policy Failure
Force GPO update
Force Intune sync
Validate compliance rules

8. Quarantine Handling
Devices enter quarantine if:
    - CMDB mismatch
    - Certificate failure
    - VPN failure
    - Domain join failure
    - Policy failure
    - Security anomaly detected
    - Quarantine actions:
    - Block domain join
    - Block certificate issuance
    - Block VPN
    - Notify admin
    - Notify user
    - Require technician intervention

9. Compliance Verification
Device is compliant when:
    - Domain join completed
    - Permanent cert installed
    - Policies applied
    - CMDB updated
    - Conditional Access passed
    - No quarantine flags
    - Security tools installed
    - Office installed
    - OS hardened


APilot Technician Step-by-Step Guide
Hands-on instructions for onboarding any device using APilot

01 - Prepare the Device (Start Here)
Ensure the device is physically ready before APilot onboarding begins.

Verify the device powers on without errors
Confirm network connectivity (Wi-Fi or Ethernet)
Check the system clock is correct (important for MFA and certificates)
Remove any previous domain join or MDM enrollment

02 - Open the APilot Invitation Email
The user must open the invitation on the device being onboarded.

Ask the user to sign in to their corporate email
Confirm the email is opened on the correct device
Have the user click the APilot Registration Link

03 - Guide the User Through MFA Authentication
Identity verification is required before any device actions occur.

Ensure the user enters their corporate email and password
Assist with MFA approval (Authenticator app, SMS, or FIDO key)
Confirm the portal loads successfully after MFA

04 - Download and Install the APilot Plugin
The plugin performs inventory, validation, and automation tasks.

Click Download Plugin when prompted
Run the installer as Administrator
Wait for the plugin to collect serial, MAC, BIOS UUID, TPM info
Confirm the plugin shows Device Identity Verified

05 - Validate CMDB Match
The plugin must match the device’s identity with the CMDB record.

Check the plugin status: CMDB Match: Success
If mismatch occurs:
Verify serial number manually
Re-check assigned user
Contact admin to correct CMDB entry

06 - Start VPN Bootstrap
The plugin prepares the device for secure connectivity.

Confirm the hostname displayed is correct
Click Continue to start VPN onboarding
Wait for the VPN package and temporary certificate to download

07 - Install Temporary Certificate and VPN Client
The plugin installs the certificate and configures the VPN automatically.

Allow the installer to run without interruption
Ensure antivirus does not block certificate installation
Confirm the plugin shows Temporary Certificate Installed
Confirm the VPN client appears in the Start Menu

08 - Connect to VPN (Most Common Issue)
VPN connectivity determines whether APilot uses cloud, hybrid, or legacy join.

Open VPN Client → Connect
Have the user enter email + password
Approve MFA if required
Wait for VPN Connected status
If connection fails:
Check internet access
Check firewall
Retry after 30 seconds

09 - Verify Network Context
APilot Gateway determines the correct domain-join path.

Plugin will show either:
Corporate Network Detected → Standard join
Non-Corporate Network → Remote join
No technician action required unless detection fails

10 - Execute Domain Join
APilot Automation Engine performs the join automatically.

Wait for plugin to show Joining Domain…
Do NOT run manual domain join commands
Confirm status changes to Domain Join Successful
If failure occurs:
Plugin triggers rollback
Device enters quarantine
Contact admin

11 - Install Permanent Certificate
The device receives its long-term trust certificate.
Confirm plugin shows Permanent Certificate Installed
If certificate fails:
Check VPN
Check time sync
Retry certificate issuance

12 - Apply Policies and Compliance
GPO/Intune baselines are applied depending on join mode.
Wait for plugin to apply:
GPO baseline (legacy/hybrid)
Intune baseline (cloud/hybrid)
Conditional Access rules
Confirm Compliance: Passed

13 - Perform Cleanup and Hardening
The plugin finalizes the device configuration.
Removal of OEM apps
Removal of old Office versions
OS hardening tasks
Installation of latest Office
Installation of security tools
Device reboot

14 - Guide User Through Final Login
Final authentication ensures device trust and compliance.
User logs in with email + password + MFA
Confirm desktop loads without errors
Verify VPN auto-connect (if required)
Confirm Office and security tools are installed

15 - Complete Device Verification
Technician performs final checks before marking onboarding complete.
Check domain membership
Check certificate presence
Check Intune or GPO policy status
Check CMDB updated to Compliant
Document onboarding completion

This is the "how". What the script must accomplish, step by step, in order, we can automatize it using this idea. Since the procedure requires interaction, an engineer must be responsible for its end-to-end execution in order for us to auomate the process based on this logic.

01 - Start APilot interective Session (Entry Point)
Initialize the APilot script, collect context, and verify technician identity.
Run PowerShell as Admin ---> execute "Start-APilotOnboarding.ps1"
    Prompt technician for:
    * ITSM Ticket ID
    * User UPN (email)
    * Device type (Laptop/Desktop/Server/VM)

    Log Session start to APilot backend via REST API (/session/start)
    Validate technician identity using API token or Entra app registration

02 - Validate CMDB Record & Device Identity Token "DIT" (Critial Check)
Ensure the device is known and correctly registered in CMDB before any oboarding.
'''Script calls CMDB API: Get /cmdb/device?upn={user}&type={deviceType} '''
    
    Collect local hardware info via PowerShell:
    '''Serial: Get-WmiObject Win32_BIOS '''
    '''BIO UUID: Get-WmiObject Win32_ComputerSystemProduct '''
    '''TMP: Get-Wmiibject Win32_Tmp''' (if present)
    VM GUID (if VM): Hyper-V/VMware tools

    Build DIT object in script
    Compare DIT with CMDB record via API
    If mismatch ---> mark device as '''Quarantine ( Post /cmdb/quarantine )''' and stop.

03 - Trigger Invitation & Portal Link (If not already Used)
Ensure the user has a valid APilot invitation and link tied to this device and ticket.
    '''Script calls APilot API: POST /invitation/create '''
    If not, create new invitation bound to:
        * User UPN
        * DIT
        * ITSM Ticket ID
    '''Log invitation status to CMDB ( POST /cmdb/update )

04 - Confirm User MFA & Portal Authentication
Verify that the user has successfully authenticated with MFA in the APilot portal.
'''Script polls APilot API: GET /session/status?upn={user}'''

    '''Wait for portal to report MFACompleted = true'''
    Implement timeout (e.g., 15-30 minutes)
    If timeout or failure ---> log to ITSM and stop
    If success ---> continue

05 - Deploy APilot Plugin to Device (automation)
Install the APolit agent that will handle local inventory, VPN, certs, and join tasks.

'''Script downloads plugin from APilot API: GET /plugin/latest'''
    * Download plugin MSI/EXE to temp folder
    * '''Install silently:
        Start-Process msiexec.exe -ArgumentList '/i plugin.msi /qn' -Wait'''

    * Verify service/ process is running
    * '''Log plugin install result to APilot backend ( POST /plugin/status ) and CMDB

06 - Run Plugin Inventory & Re-Validate DIT
Use the plugin to perform a deeper inventory and confirm device idenitity again.
'''Script calls local plugin API: http://localhost:port/api/inventory'''

    Retrieve plugin inventory (serial, UUID, TPM, NICs, OS build)
    Compare with initial DIT
    '''Send final DIT to APilot backend ( POST /device/validate )
    If mismatch ---> quarantine and stop

07 - Bootstrap VPN Package & Temporary Certificate
Prepare secure connectivity by installing a temporary device certificate and VPN client.
'''Script calls APilot API: POST /vpn/bootstrap'''

    Request VPN package (client + config) and temp cert
    Download package to device
    '''Install temporary cert via PowerShell:
        Important-Certificate''' into LocalMachine\My
    Install VPN client silenty (AnyConnect/GlobalProtect/etc.)
    Log success/failure to APolot backend

08 - Establish VPN Connection & Detect Network Context (Network Decision)
Connect to VPN and determine if the device is on corporate or non-corporate network.
'Script triggers VPN client with pre-configured profile'

    '''Use command-line or API to start VPN (e.g., vpncli connect profile )'''
    Wait for connection status - Connected
    Run network test:
        - Ping DCs
        - Resolve AD DNS
        - Reach PKI/CMDB endpoints
    '''Call APilot Gateway API: POST /network/context''' with test results
    '''Receive mode = CloudOnly | Hybrid | Legacy'''

09 - Execute Domain Join via APilot Automation Engine (Core Join)
Perform the actual domain join using backend APIs, not local privileged credentials.
'''Script calls APilot API: POST /join/execute'''

    Send DIT, mode, hostname, user UPN to backend
    Backend performs:
        AD DS join (Legacy/Hybrid)
        Entra Join (Cloud/Hybrid)
    '''Script monitors join status via: GET /join/status'''
    If failure ---> trigger rollback ( POST /join/rollback ) and quarantine'''

10 - Issue Permanent Device Certificate
Replace temporary onboarding cert with a permanent trust certificate.
'''Script calls PKI API via APilot: POST /cert/issue'''

    Request permanent cert bound to DIT
    Install cert into LocalMachine\My
    Remove temporary cert
    Log cert issuance to CMDB and APilot backend

11 - Apply Policies (GPO + Intune)
Ensure the device receives baseline security and configuration policies.
'Script triggers policy sync commands'
    '''For legacy/hybrid:
        gpupdate /force'''
    For cloud/hybrid:
        Trigger Intune sync via WMI/PowerShell or Company Portal
    '''Call APilot API: POST /policy/verify''' to confirm baseline applied
    If policy failure ---> log and mark device as non-compliant

12 - Perform Cleanup & OS Hardening
Remove unwanted software, install required tools, and harden the OS.
'Script runs local cleanup/hardening routines'

    Uninstall OEM bloatware (list from APilot backend)
    Remove old Office versions
    Install lastest Office via Intune or Click-to-Run
    Install security tools (AV/EDR/agent)
    Apply OS hardening setttings (registry, services, firewall)
    Log actions to APilot backend and CMDB

13 - Final Reboot & Compliance Check (Pre-Final)
Reboot the device and confirm it is fully compliant and ready for user login.
'''Script executes Restart-Computer with delay'''

    Schedule reboot with user notification
    After reboot, script (or plugin) runs compliance check:
        * Domain join status
        * Cert presence
        * Policy application
        * Security tools running
    '''Call APilot API: POST /device/compliance''' '''---> expect status = compliant'''
    If not compliant ---> flag in CMDB and ITSM

14 - Send Automatic Guidance Email to End-User (User Communication)
Inform the user that onboarding is complete and guide them through final login.
'''Script calls Emails API or Graph API: POST /notify/user'''

    Compose email:
        Subject: "Your Device is Reday - Login Instructions"
        Body: Steps to log-in with email, password, MFA.
    
    Include ITSM TIcket number and Support contact
    Log email delivery status to APilot backend

15 - Generate Technician Checklist & Attach to ISTM Ticket (Operational Closure)
Create a structured checklist of all actions and results, and attach it to the ITSM Ticket.
'''Script calls ITSM API: POST /ticket/update'''

    Build checklist JSON with:
        DIT verified
        CMDB updated
        Plugin installed
        VPN connected
        Domain join completed
        Certs issued
        Policies applied
        Cleanup done
        Compliance - true
    Convert to human-readable text and attach it to ticket
    Mark ticket status as "Onboarding Device Completed"

    Module Skeleton (APilot.psm1)
    This script was created in VS Code with help of Copilot chat.

    '''Text
    <#
.APilot.psm1
APilot PowerShell Module Skeleton
Purpose:
    Provide core functions for the APilot onboarding methodology:
    - Identity & invitation
    - Device identity (DIT)
    - CMDB checks
    - Plugin install
    - VPN bootstrap
    - Certificate handling
    - Domain join
    - Policy sync
    - Cleanup
    - Email
    - ITSM integration
Note:
    This is a skeleton for real-world implementation.
    All external calls (REST APIs, Graph, CMDB, PKI, VPN, etc.)
    are represented as placeholders.
#>

# region: Module Initialization

# Load configuration (API endpoints, tokens, etc.)
$Script:APilotConfig = @{
    ApiBaseUrl      = "https://apilot-backend.local/api"
    CMDBBaseUrl     = "https://cmdb.local/api"
    ITSMBaseUrl     = "https://itsm.local/api"
    PKIBaseUrl      = "https://pki.local/api"
    VPNProfileName  = "APilot-VPN"
    LogPath         = "C:\APilot\Logs\apilot.log"
}

function Write-APilotLog {
    param(
        [string]$Message,
        [string]$Level = "INFO"
    )
    $timestamp = Get-Date -Format "yyyy-MM-dd HH:mm:ss"
    $line = "$timestamp [$Level] $Message"
    Write-Host $line
    Add-Content -Path $Script:APilotConfig.LogPath -Value $line
}

function Invoke-APilotAPI {
    param(
        [string]$Method,
        [string]$Endpoint,
        [object]$Body = $null
    )

    $url = "$($Script:APilotConfig.ApiBaseUrl)$Endpoint"
    Write-APilotLog "Calling APilot API: $Method $url" "DEBUG"

    try {
        if ($Body) {
            $json = $Body | ConvertTo-Json -Depth 5
            return Invoke-RestMethod -Method $Method -Uri $url -Body $json -ContentType "application/json"
        } else {
            return Invoke-RestMethod -Method $Method -Uri $url
        }
    }
    catch {
        Write-APilotLog "APilot API call failed: $($_.Exception.Message)" "ERROR"
        throw
    }
}

# endregion

# region: Identity & Invitation

function Invoke-APilotInvitation {
    <#
    .SYNOPSIS
        Validates that the current session is associated with a valid APilot invitation.
    .DESCRIPTION
        In a real implementation, this would:
        - Validate an invitation token (from URL or file)
        - Confirm that the invitation is still valid (not expired, not used)
    #>
    Write-APilotLog "Validating APilot invitation..."
    # Placeholder: simulate success
    return $true
}

function Invoke-APilotMFA {
    <#
    .SYNOPSIS
        Ensures MFA has been completed for the current user.
    .DESCRIPTION
        In a real implementation, this would:
        - Integrate with Entra ID / IdP
        - Confirm MFA status via API
    #>
    Write-APilotLog "Checking MFA status..."
    # Placeholder: simulate success
    return $true
}

# endregion

# region: Device Identity Token (DIT)

function Get-DIT {
    <#
    .SYNOPSIS
        Collects Device Identity Token (DIT) attributes.
    .DESCRIPTION
        DIT is a composite identity:
        - Serial number
        - BIOS UUID
        - TPM identity (if available)
        - Hardware hash (if available)
        - VM GUID (if applicable)
        - MAC addresses (secondary)
    #>
    Write-APilotLog "Collecting Device Identity Token (DIT)..."

    $serial = (Get-CimInstance Win32_BIOS).SerialNumber
    $biosUuid = (Get-CimInstance Win32_ComputerSystemProduct).UUID
    $macs = Get-NetAdapter | Where-Object { $_.Status -eq "Up" } | Select-Object -ExpandProperty MacAddress

    $DIT = [PSCustomObject]@{
        SerialNumber = $serial
        BIOSUUID     = $biosUuid
        MACAddresses = $macs
        # TPM, hardware hash, VM GUID would be added here
    }

    Write-APilotLog "DIT collected: Serial=$serial, BIOSUUID=$biosUuid"
    return $DIT
}

function Invoke-DITValidation {
    param(
        [Parameter(Mandatory)][object]$DIT
    )
    <#
    .SYNOPSIS
        Validates DIT against APilot backend.
    .DESCRIPTION
        In a real implementation, this would:
        - Send DIT to APilot backend
        - Confirm match with CMDB record
    #>
    Write-APilotLog "Validating DIT against APilot backend..."
    $response = Invoke-APilotAPI -Method "POST" -Endpoint "/device/validate" -Body $DIT
    if ($response.status -ne "ok") {
        Write-APilotLog "DIT validation failed." "ERROR"
        throw "DIT validation failed."
    }
    Write-APilotLog "DIT validation succeeded."
    return $true
}

# endregion

# region: CMDB

function Invoke-CMDBCheck {
    param(
        [Parameter(Mandatory)][object]$DIT
    )
    <#
    .SYNOPSIS
        Checks CMDB for device record.
    .DESCRIPTION
        In a real implementation, this would:
        - Query CMDB by DIT attributes
        - Confirm device exists and is assigned to correct user
    #>
    Write-APilotLog "Checking CMDB for device record..."
    # Placeholder: simulate success
    return $true
}

function Update-CMDBState {
    param(
        [Parameter(Mandatory)][object]$DIT,
        [Parameter(Mandatory)][string]$State
    )
    <#
    .SYNOPSIS
        Updates CMDB with device state.
    .DESCRIPTION
        In a real implementation, this would:
        - Write back final state (Compliant, Quarantined, etc.)
    #>
    Write-APilotLog "Updating CMDB state to '$State'..."
    # Placeholder: simulate success
    return $true
}

# endregion

# region: Plugin

function Install-APilotPlugin {
    <#
    .SYNOPSIS
        Installs APilot plugin/agent.
    .DESCRIPTION
        In a real implementation, this would:
        - Download plugin from APilot backend
        - Install silently
        - Register service
    #>
    Write-APilotLog "Installing APilot plugin..."
    # Placeholder: simulate success
    return $true
}

# endregion

# region: VPN

function Install-VPNClient {
    <#
    .SYNOPSIS
        Installs VPN client for APilot.
    .DESCRIPTION
        In a real implementation, this would:
        - Download VPN installer
        - Install with predefined profile
    #>
    Write-APilotLog "Installing VPN client..."
    # Placeholder: simulate success
    return $true
}

function Invoke-VPNConnect {
    <#
    .SYNOPSIS
        Connects to VPN using APilot profile.
    .DESCRIPTION
        In a real implementation, this would:
        - Use VPN client CLI/API
        - Authenticate user
        - Verify connectivity
    #>
    Write-APilotLog "Connecting to VPN..."
    # Placeholder: simulate success
    return $true
}

# endregion

# region: Certificates

function Request-TempCert {
    param(
        [Parameter(Mandatory)][object]$DIT
    )
    <#
    .SYNOPSIS
        Requests temporary onboarding certificate.
    .DESCRIPTION
        In a real implementation, this would:
        - Call PKI API
        - Issue short-lived cert bound to DIT
    #>
    Write-APilotLog "Requesting temporary certificate..."
    $response = Invoke-APilotAPI -Method "POST" -Endpoint "/cert/temp/issue" -Body $DIT
    if ($response.status -ne "ok") {
        Write-APilotLog "Temporary certificate request failed." "ERROR"
        throw "Temporary certificate request failed."
    }
    Write-APilotLog "Temporary certificate issued."
    return $true
}

function Revoke-TempCert {
    param(
        [Parameter(Mandatory)][object]$DIT
    )
    <#
    .SYNOPSIS
        Revokes temporary certificate.
    .DESCRIPTION
        In a real implementation, this would:
        - Call PKI API to revoke cert
    #>
    Write-APilotLog "Revoking temporary certificate..."
    $response = Invoke-APilotAPI -Method "POST" -Endpoint "/cert/temp/revoke" -Body $DIT
    Write-APilotLog "Temporary certificate revoked."
    return $true
}

function Request-PermanentCert {
    param(
        [Parameter(Mandatory)][object]$DIT
    )
    <#
    .SYNOPSIS
        Requests permanent device certificate.
    .DESCRIPTION
        In a real implementation, this would:
        - Call PKI API
        - Issue long-lived cert for device trust
    #>
    Write-APilotLog "Requesting permanent certificate..."
    $response = Invoke-APilotAPI -Method "POST" -Endpoint "/cert/permanent/issue" -Body $DIT
    if ($response.status -ne "ok") {
        Write-APilotLog "Permanent certificate request failed." "ERROR"
        throw "Permanent certificate request failed."
    }
    Write-APilotLog "Permanent certificate issued."
    return $true
}

# endregion

# region: Domain Join

function Invoke-DomainJoin {
    param(
        [Parameter(Mandatory)][object]$DIT
    )
    <#
    .SYNOPSIS
        Performs domain join via APilot backend.
    .DESCRIPTION
        In a real implementation, this would:
        - Call backend which runs netdom/Add-Computer
        - Handle legacy/hybrid/cloud modes
    #>
    Write-APilotLog "Initiating domain join..."
    $response = Invoke-APilotAPI -Method "POST" -Endpoint "/domain/join" -Body $DIT
    if ($response.status -ne "ok") {
        Write-APilotLog "Domain join failed." "ERROR"
        throw "Domain join failed."
    }
    Write-APilotLog "Domain join completed."
    return $true
}

function Invoke-DomainRollback {
    param(
        [Parameter(Mandatory)][object]$DIT
    )
    <#
    .SYNOPSIS
        Rolls back domain join.
    .DESCRIPTION
        In a real implementation, this would:
        - Remove computer object
        - Reset local state
    #>
    Write-APilotLog "Rolling back domain join..."
    $response = Invoke-APilotAPI -Method "POST" -Endpoint "/domain/rollback" -Body $DIT
    Write-APilotLog "Domain rollback completed."
    return $true
}

# endregion

# region: Policies

function Invoke-GPOSync {
    <#
    .SYNOPSIS
        Forces GPO sync.
    .DESCRIPTION
        In a real implementation, this would:
        - Run gpupdate /force
        - Validate baseline applied
    #>
    Write-APilotLog "Applying GPO baseline..."
    # Placeholder: simulate success
    return $true
}

function Invoke-IntuneSync {
    <#
    .SYNOPSIS
        Forces Intune sync.
    .DESCRIPTION
        In a real implementation, this would:
        - Use Intune/Graph APIs
        - Validate device compliance
    #>
    Write-APilotLog "Triggering Intune sync..."
    # Placeholder: simulate success
    return $true
}

# endregion

# region: Cleanup

function Invoke-OSHardening {
    <#
    .SYNOPSIS
        Applies OS hardening.
    .DESCRIPTION
        In a real implementation, this would:
        - Apply security baselines
        - Disable unnecessary services
    #>
    Write-APilotLog "Applying OS hardening..."
    # Placeholder: simulate success
    return $true
}

function Invoke-AppCleanup {
    <#
    .SYNOPSIS
        Removes unwanted apps.
    .DESCRIPTION
        In a real implementation, this would:
        - Uninstall OEM bloatware
        - Remove old Office versions
    #>
    Write-APilotLog "Cleaning up applications..."
    # Placeholder: simulate success
    return $true
}

function Invoke-FinalReboot {
    <#
    .SYNOPSIS
        Performs final reboot.
    .DESCRIPTION
        In a real implementation, this would:
        - Schedule reboot
        - Inform user
    #>
    Write-APilotLog "Performing final reboot..."
    # Placeholder: simulate success
    return $true
}

# endregion

# region: Email

function Send-UserFinalEmail {
    param(
        [Parameter(Mandatory)][string]$UserEmail
    )
    <#
    .SYNOPSIS
        Sends final guidance email to user.
    .DESCRIPTION
        In a real implementation, this would:
        - Use SMTP or Graph API
        - Send standardized instructions
    #>
    Write-APilotLog "Sending final email to $UserEmail..."
    $body = @"
Hello,

Your device has completed APilot onboarding and is now ready for use.

Please follow these steps:
1. Power on your device.
2. Log in using your corporate email and password.
3. Complete MFA when prompted.
4. Wait for the desktop to fully load — policies and apps will finalize in the background.

If anything looks unusual, contact IT Support and reference your ticket number.

Thank you,
IT Operations Team
"@

    # Placeholder: simulate success
    Write-APilotLog "Final email prepared (not actually sent in skeleton)."
    return $true
}

# endregion

# region: ITSM

function Update-ITSMTicket {
    param(
        [Parameter(Mandatory)][object]$DIT,
        [Parameter(Mandatory)][string]$Status
    )
    <#
    .SYNOPSIS
        Updates ITSM ticket with APilot status.
    .DESCRIPTION
        In a real implementation, this would:
        - Call ITSM API (ServiceNow, etc.)
        - Attach checklist and logs
    #>
    Write-APilotLog "Updating ITSM ticket status to '$Status'..."
    # Placeholder: simulate success
    return $true
}

function Close-ITSMTicket {
    param(
        [Parameter(Mandatory)][object]$DIT
    )
    <#
    .SYNOPSIS
        Closes ITSM ticket after successful onboarding.
    .DESCRIPTION
        In a real implementation, this would:
        - Call ITSM API to close ticket
    #>
    Write-APilotLog "Closing ITSM ticket..."
    # Placeholder: simulate success
    return $true
}

# endregion

Export-ModuleMember -Function * -Alias *
'''
