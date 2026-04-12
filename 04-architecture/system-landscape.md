# System Landscape

- Owner: CTO / Architecture
- Status: draft
- Last Updated: 2026-04-12
- Related Docs: `../03-engineering/engineering-plan.md`, `../06-security/security-program.md`

## Overview

Document the current and target technical landscape for `Svenskmat`.

## Core Systems

| System | Purpose | Owner | Criticality | Notes |
| --- | --- | --- | --- | --- |
| Microsoft 365 | Identity, email, and business collaboration | CTO | High | First core business platform to establish |
| Loopia DNS | Current authoritative DNS hosting | CTO | High | Temporary state during Microsoft 365 setup |
| Cloudflare | Target DNS and edge platform | CTO | High | Planned destination after email is validated |

## Major Domains

- Customer-facing applications
- Internal operations tooling
- Data and analytics
- Integrations and third-party services
- Infrastructure and platform

## Questions To Answer

- What are the primary applications and services?
- Where does source of truth live for key data?
- What are the critical integrations?
- What are the main failure points?

## Architecture Decisions

Link ADRs here as they are created.

Current expected decisions:

- Microsoft 365 will be the primary identity and email platform.
- Loopia remains the DNS provider during the first email setup phase.
- Cloudflare becomes the long-term DNS provider after Microsoft 365 mail flow is stable.

## Current Pain Points

- No production platform is in place yet.
- Foundational business systems and ownership boundaries still need to be defined.

## Target State

Describe the intended platform shape over the next 12 months.

Near-term target state:

- Business identity and email centralized in Microsoft 365
- DNS and future public edge services centralized in Cloudflare
- Foundational security controls established before application development scales
