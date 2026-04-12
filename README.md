# Svenskmat Documentation Hub

This repository is the central document hub for `Svenskmat`.

It is intended to hold the working documentation for company direction, product planning, engineering execution, architecture, operations, security, and delivery.

## Table Of Contents

- [Purpose](#purpose)
- [Structure](#structure)
- [Recommended Operating Model](#recommended-operating-model)
- [Suggested First Documents To Complete](#suggested-first-documents-to-complete)
- [Ownership](#ownership)
- [Change Discipline](#change-discipline)
- [Core Planning Docs](#core-planning-docs)

## Purpose

This hub should answer the following questions quickly:

- What are we building and why?
- What are the current priorities?
- How is the platform structured?
- How does engineering operate?
- How do we run, secure, and ship the business?

## Structure

- [`00-governance`](B:\.dev\Svenskmat\Documentation\00-governance): Documentation standards, ownership, and operating rules.
- [`01-strategy`](B:\.dev\Svenskmat\Documentation\01-strategy): Company direction, mission, goals, and planning assumptions.
- [`02-product`](B:\.dev\Svenskmat\Documentation\02-product): Product roadmap, customer problems, and prioritization.
- [`03-engineering`](B:\.dev\Svenskmat\Documentation\03-engineering): Engineering plans, standards, and team operating model.
- [`04-architecture`](B:\.dev\Svenskmat\Documentation\04-architecture): System landscape, service boundaries, and technical decisions.
- [`05-operations`](B:\.dev\Svenskmat\Documentation\05-operations): Runbooks, incident handling, and operational readiness.
- [`06-security`](B:\.dev\Svenskmat\Documentation\06-security): Security posture, controls, and risk management.
- [`07-delivery`](B:\.dev\Svenskmat\Documentation\07-delivery): Quarterly planning, milestones, and execution tracking.
- [`_templates`](B:\.dev\Svenskmat\Documentation\_templates): Reusable templates for repeated documentation patterns.

## Recommended Operating Model

- Keep strategic documents short and current.
- Prefer one canonical document per topic.
- Link related documents instead of duplicating content.
- Record important technical and product decisions explicitly.
- Update this hub during planning, delivery, and incident response, not after the fact.

## Suggested First Documents To Complete

1. `01-strategy/vision-and-goals.md`
2. `02-product/product-roadmap.md`
3. `04-architecture/system-landscape.md`
4. `03-engineering/engineering-plan.md`
5. `06-security/security-program.md`

## Ownership

Suggested default owners:

- Strategy: CTO / Leadership
- Product: Product + Leadership
- Engineering: Engineering leadership
- Architecture: CTO / Principal engineering
- Operations: Engineering + Operations
- Security: CTO / Security owner

## Change Discipline

For meaningful technical or product decisions, create an ADR from the template in [`_templates/decision-record.md`](B:\.dev\Svenskmat\Documentation\_templates\decision-record.md).

## Core Planning Docs

- [Microsoft 365 And DNS Migration Plan](B:\.dev\Svenskmat\Documentation\05-operations\ms365-and-dns-migration-plan.md)
- [Microsoft 365 Business Premium Rollout Plan](B:\.dev\Svenskmat\Documentation\05-operations\ms365-business-premium-rollout-plan.md)
- [System Landscape](B:\.dev\Svenskmat\Documentation\04-architecture\system-landscape.md)
