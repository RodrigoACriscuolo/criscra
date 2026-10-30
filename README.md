# APilot

Secure onboarding and trust orchestration for enterprise Windows devices.

APilot is a Microsoft-aligned concept for ensuring that a Windows device is only trusted and productive after identity, device posture, certificate validity, compliance, and asset ownership have all been validated together.

## Why this matters

Most enterprise onboarding problems are not caused by a single failure. They are the result of fragmented controls across identity, device management, certificate trust, CMDB governance, and network access.

In many environments, the join event is treated as a technical step rather than a trust decision. That leads to:

- failed joins and delayed productivity
- CMDB drift and incorrect ownership records
- expired or misissued certificates
- partially trusted devices left in an inconsistent state
- manual workarounds and helpdesk escalation
- inconsistent security posture during onboarding

## What APilot does

APilot brings these pieces together into a single lifecycle model. Instead of asking only, “Did the device join?” it asks a more complete set of questions:

- Is the user valid and authorized?
- Is the device compliant and healthy?
- Is the asset correctly identified and owned?
- Are the certificates valid and trusted?
- Is the network context appropriate for onboarding?
- Should the device be allowed to become productive, or should it be quarantined?

## Trust model

The model is built around a simple principle: trust before productivity.

A device should not be considered trusted just because it has enrolled or joined a domain. Trust should be granted only after the relevant checks pass across the identity, device, certificate, asset, and access layers.

## Architecture view

APilot is structured around a few core trust planes:

- Identity Plane: validates user identity and entitlement
- Device Plane: validates posture, health, and compliance
- PKI Plane: validates certificates and trust artifacts
- CMDB / Asset Plane: validates ownership and lifecycle state
- Network / Access Plane: validates the appropriate trust context for onboarding
- Control Plane: orchestrates the decisions and remediation workflow

## Scope of this branch

This branch focuses on the APilot concept and the supporting technical knowledge base.

### Included documents

- [APilot-Professional-Concept.md](./APilot-Professional-Concept.md)
- [Domain-Join_Knowledge.is.Power.md](./Domain-Join_Knowledge.is.Power.md)

## Why it is useful

APilot is especially relevant for hybrid and Microsoft-centric enterprise environments where identity, endpoint management, and security operations are distributed across teams. It provides a clearer and more governable way to manage device trust before a machine becomes productive.

## Suggested next steps

1. Review the concept and validate the trust lifecycle against your current onboarding model.
2. Identify where the real trust gaps exist in your environment.
3. Decide whether this should remain a concept document or evolve into a practical onboarding model.
4. Use the technical document as the basis for architecture review, pilot design, or stakeholder discussion.

---

APilot is best understood as a practical governance model: it does not replace Microsoft services, but makes them work together more consistently and with better security discipline.
