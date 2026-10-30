# APilot

A Microsoft-aligned enterprise onboarding and trust orchestration concept for secure Windows device enrollment and domain join.

## Overview

APilot is a secure enterprise onboarding and domain join concept designed for Microsoft-managed Windows environments. It brings together Microsoft Entra ID, AD DS, Intune, Windows Autopilot, Conditional Access, PKI, and CMDB governance into a single zero-trust onboarding lifecycle.

The goal is to ensure that a device is not treated as trusted or productive until identity, asset ownership, posture, and certificate state are all validated together.

## Why this matters

Enterprise onboarding is often fragmented across identity, device management, compliance, PKI, and asset lifecycle systems. That creates:

- failed joins and poor first-time success rates
- partially trusted devices
- stale or orphaned device records
- certificate drift or expired trust artifacts
- inconsistent onboarding experiences across teams
- increased helpdesk and support burden
- higher risk from lost, stolen, or decommissioned devices

APilot addresses these issues by defining a governed lifecycle that includes:

- identity validation
- device identity collection
- CMDB validation
- compliance gating
- temporary trust windowing
- join authorization
- quarantine and rollback
- wipe and decommission controls

## Target audience

This concept is intended for:

- Microsoft ecosystem stakeholders
- endpoint engineering and identity teams
- security and zero-trust architects
- enterprise IT operations leaders
- CMDB and ITSM governance teams
- design and modernization review groups

## Primary document

- Full technical concept: [Domain-Join_Knowledge.is.Power.md](./Domain-Join_Knowledge.is.Power.md)

## Stakeholder summary

- Executive summary for Microsoft-facing review: [docs/microsoft-stakeholder-summary.md](./docs/microsoft-stakeholder-summary.md)

## Shareable outreach

- Short email / outreach note: [docs/shareable-email.md](./docs/shareable-email.md)

## Core Microsoft alignment

APilot aligns with the Microsoft ecosystem through:

- Microsoft Entra ID
- AD DS
- Intune
- Windows Autopilot
- Conditional Access
- certificate-based trust and PKI
- device compliance and security baselines
- CMDB / ITSM asset governance
- VPN / ZTNA / segmentation controls

## Business value

APilot creates value for enterprise buyers and architects by reducing onboarding risk, improving first-time success, and enforcing a governed trust lifecycle across identity, security, and operations.

It helps organizations:

- reduce failed joins and support escalations
- prevent partial trust states
- improve CMDB quality and lifecycle accuracy
- strengthen certificate hygiene and zero-trust enforcement
- reduce risk from lost, stolen, or decommissioned devices
- create a clearer and more auditable onboarding process

## Short concept statement

APilot is a Microsoft-aligned secure onboarding and trust orchestration concept for enterprise Windows devices. It combines identity, device health, policy compliance, certificate state, and asset ownership into a single controlled lifecycle that ensures devices are only trusted after they are validated.

## Repository status

This branch is intended as a concept, design, and review artifact for secure device onboarding and trust lifecycle governance in Microsoft-centric enterprise environments.

## Suggested next steps

1. Validate the architecture with Microsoft-aligned identity and endpoint teams.
2. Refine the state model and trust-window policy.
3. Align the concept to enterprise CMDB and ITSM governance patterns.
4. Convert this into a lightweight pilot or MVP design workshop.
5. Decide whether to keep this as a review branch or publish it as a standalone APilot project repository.
