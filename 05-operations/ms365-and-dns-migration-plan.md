# Microsoft 365 And DNS Migration Plan

- Owner: CTO
- Status: draft
- Last Updated: 2026-04-12
- Related Docs: `../04-architecture/system-landscape.md`, `../06-security/security-program.md`, `./runbooks-index.md`

## Objective

Establish company email and identity on `Microsoft 365` while the domain is still managed in `Loopia`, then migrate authoritative DNS to `Cloudflare` after mail flow is working and validated.

## Planning Assumptions

- The business has not started product development yet.
- There is no production workload that would make provider migration risky.
- `Loopia` is the current DNS host for the company domain.
- `Microsoft 365` will be the first core business platform for identity, email, and collaboration.
- `Cloudflare` will become the long-term DNS provider and likely the edge platform for future applications.

## Recommended Sequence

1. Keep authoritative DNS at `Loopia` during Microsoft 365 setup.
2. Verify the domain in `Microsoft 365`.
3. Configure email-related DNS records in `Loopia`.
4. Create user accounts, aliases, groups, and baseline security controls in `Microsoft 365`.
5. Validate inbound and outbound email flow.
6. Lower DNS TTL values before the planned DNS migration window.
7. Recreate the full DNS zone in `Cloudflare`.
8. Change nameservers from `Loopia` to `Cloudflare`.
9. Re-validate email, web, and domain control records after cutover.

## Why This Sequence

This order reduces risk.

- `Microsoft 365` setup is easier while the current DNS provider remains authoritative.
- Email should be proven first before moving DNS ownership.
- `Cloudflare` migration becomes a controlled infrastructure move instead of mixing identity setup and DNS cutover into one change.

## Phase 1: Microsoft 365 Foundation

## Scope

- Tenant creation
- Domain verification
- User identity setup
- Email routing
- Basic security baseline

## Initial Decisions

- Use `Microsoft 365` as the system of record for business identity and email.
- Use company-domain email from the start instead of consumer inboxes.
- Keep DNS changes minimal until email is verified.

## Required DNS Records At Loopia

Expected categories:

- Domain verification TXT record
- MX record for Microsoft 365
- SPF TXT record
- CNAME records for autodiscover and Microsoft service discovery where needed
- DKIM records after mail flow is live
- DMARC TXT record after SPF and DKIM are in place

## Microsoft 365 Baseline Setup

- Create named admin accounts separate from day-to-day user accounts
- Enable MFA for all admin accounts immediately
- Enable MFA for all users as part of onboarding
- Define shared mailboxes and role-based addresses early
- Create distribution lists only if they are actually needed
- Standardize naming for users, aliases, and groups

## Suggested Foundational Mailboxes

- `hello@`
- `support@`
- `billing@`
- `security@`
- `admin@`

## Phase 2: Security Baseline

Before broad usage, implement these controls:

- MFA for all users
- Break-glass account with tightly controlled credentials
- Least-privilege admin roles
- Password manager for shared operational secrets
- DKIM enabled
- DMARC policy started at monitoring mode, then tightened later
- Audit logging enabled

## Phase 3: DNS Migration To Cloudflare

## Preconditions

- Microsoft 365 email works for sending and receiving
- All current DNS records are inventoried from `Loopia`
- TTL values have been reduced in advance where practical
- Domain registrar access is confirmed
- Rollback owner is assigned for the migration window

## Migration Approach

1. Export or manually inventory all records in `Loopia`
2. Create the same zone records in `Cloudflare`
3. Double-check mail, verification, and service records
4. Change nameservers at the registrar to the `Cloudflare` nameservers
5. Wait for propagation and test all critical services

## Records That Must Be Checked Carefully

- MX
- SPF
- DKIM
- DMARC
- Autodiscover
- TXT ownership verification records
- Any future web or API records

## Risks

- Missing a mail-related DNS record during migration
- Accidentally breaking SPF during record consolidation
- Turning on a strict DMARC policy too early
- Using a single global admin account for normal work
- Migrating DNS before mail validation is complete

## Recommended Operational Policy

- No nameserver migration on the same day as first-time email setup
- No strict DMARC enforcement until legitimate senders are confirmed
- Keep a written record of all DNS changes during setup
- Use one change owner and one verifier for production DNS updates

## Open Decisions

- Which Microsoft 365 license tier fits the business at launch?
- Will endpoint/device management be handled in Microsoft from day one?
- Will Cloudflare be DNS-only initially, or also WAF / Zero Trust / edge platform?
- Which shared mailboxes are required at launch versus later?

## Immediate Next Actions

1. Inventory the company domain, registrar access, and current `Loopia` DNS records.
2. Decide the initial Microsoft 365 license level and admin model.
3. Create the Microsoft 365 tenant and verify the domain while DNS stays on `Loopia`.
4. Configure mail DNS records in `Loopia` and validate email flow.
5. Plan a separate DNS migration window to move authoritative DNS to `Cloudflare`.
