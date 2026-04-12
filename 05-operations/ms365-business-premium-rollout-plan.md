# Microsoft 365 Business Premium Rollout Plan

- Owner: CTO
- Status: draft
- Last Updated: 2026-04-12
- Related Docs: `./ms365-and-dns-migration-plan.md`, `../04-architecture/system-landscape.md`, `../06-security/security-program.md`

## Objective

Stand up `Microsoft 365 Business Premium` as the company's first production business platform for:

- Identity and access
- Email and calendaring
- Collaboration and file storage
- Device management
- Endpoint security
- Baseline compliance and data protection

This plan assumes `Svenskmat` is pre-launch and can adopt secure defaults from day one.

## Design Principles

- Use company-owned identities from the start.
- Keep administrative access separate from day-to-day work.
- Use `Conditional Access`, not just security defaults, because `Business Premium` includes `Microsoft Entra ID P1`.
- Keep authoritative DNS at `Loopia` until Microsoft 365 mail flow is working.
- Treat DNS provider migration to `Cloudflare` as a separate change after Microsoft 365 is stable.
- Roll out email authentication in stages: `SPF`, then `DKIM`, then `DMARC`.
- Use `Intune` and `Defender for Business` as the default endpoint management and protection stack.

## Svenskmat Default Decisions

These defaults should be treated as the working standard unless changed by an explicit later decision.

### Identity

- One named daily account per employee.
- Separate named admin accounts for privileged work.
- Two cloud-only emergency access accounts.
- No shared admin credentials.
- MFA required for all users.

### Email

- `Exchange Online` is the authoritative mail platform.
- All staff use company-domain mailboxes, not consumer email.
- Shared mailboxes are allowed for role addresses.
- SMTP/POP/IMAP legacy access should remain disabled unless a business system requires an exception.

### Files And Collaboration

- `OneDrive` for individual work files.
- `SharePoint Online` for company and team document storage.
- `Teams` for internal chat, meetings, lightweight collaboration, and channel-based file access.
- Avoid using personal OneDrive locations as the primary location for shared company documents.

### Device Management

- All company-managed Windows devices enroll into `Intune`.
- Require device compliance for access to core company apps.
- New Windows hardware should be prepared for `Windows Autopilot`.
- Personal device access should be minimized at launch and only allowed where policy explicitly permits it.

### Endpoint Security

- `Defender for Business` is the default endpoint protection platform.
- `Defender for Office 365 Plan 1` protections should be enabled and reviewed early.
- Server protection is out of scope unless Svenskmat introduces servers; if needed later, evaluate the `Defender for Business servers` add-on.

## Phase 1: Tenant Foundation

### Goals

- Create the tenant correctly the first time.
- Prevent single-admin lockout.
- Establish the identity model before broad user onboarding.

### Actions

1. Create the `Microsoft 365 Business Premium` tenant.
2. Record the initial `onmicrosoft.com` fallback domain.
3. Create the first named global admin account.
4. Create two cloud-only emergency access accounts and assign `Global Administrator`.
5. Create a separate named admin account for day-to-day administration.
6. Confirm billing ownership, support access, and tenant recovery contacts.

### Configuration Standards

- Emergency accounts must not be used for daily operations.
- Emergency accounts should use strong, distinct authentication methods and have credentials stored offline in a controlled process.
- Regular administrators should use least-privilege roles where possible instead of permanent global admin access.

## Phase 2: Domain And DNS Preparation

### Goals

- Verify domain ownership.
- Prepare Microsoft 365 mail routing while DNS stays on `Loopia`.

### Actions

1. Inventory all current DNS records in `Loopia`.
2. Add the company domain in the Microsoft 365 admin center.
3. Verify ownership with the recommended `TXT` record.
4. Add all required Microsoft 365 DNS records in `Loopia`.
5. Add users and mailboxes before changing the domain MX record to Microsoft 365.

### Required DNS Record Categories

- TXT domain verification
- MX for Exchange Online
- SPF TXT
- Autodiscover / service discovery CNAMEs as required by the Microsoft setup wizard
- DKIM records after initial mail flow is confirmed
- DMARC TXT record after SPF and DKIM are in place

### Rule

Do not move nameservers to `Cloudflare` until Microsoft 365 send/receive testing is complete.

## Phase 3: User, Group, And Mailbox Model

### User Standards

- Every worker gets a named user account.
- Every privileged admin gets a separate named admin account.
- License assignment should be group-based where practical once the tenant stabilizes.

### Initial Role Mailboxes

- `hello@`
- `support@`
- `billing@`
- `security@`
- `admin@`

### Group Standards

- Use `Microsoft 365 Groups` only where collaboration and shared workspace value is clear.
- Avoid creating distribution lists by default.
- Keep group and mailbox naming explicit and operationally clear.

### Mail Configuration Standards

- Set the primary company domain as default once verified and tested.
- Standardize aliases for public-facing addresses.
- Disable unused legacy mail protocols unless needed for a documented exception.

## Phase 4: Authentication And Access Control

### Decision

Use `Conditional Access` as the long-term baseline instead of relying only on `security defaults`.

Reason:

- Microsoft documents security defaults as a good baseline for simpler or free-tier environments.
- Microsoft also documents that organizations with `Entra ID P1` should consider `Conditional Access`.
- `Business Premium` includes `Entra ID P1`.

### Recommended Conditional Access Baseline

1. Require MFA for all users.
2. Require MFA for all admins with no exceptions other than emergency accounts.
3. Block legacy authentication.
4. Require compliant or managed devices for access to admin portals and core data where practical.
5. Require stronger controls for privileged roles.

### Emergency Access Exception

- Exclude only the emergency access accounts from the minimum necessary policies.
- Monitor and alert on any use of those accounts.

### Authentication Method Standards

- Prefer `Microsoft Authenticator`, `FIDO2/passkeys`, or `Windows Hello for Business` where applicable.
- Register more than one auth method for privileged accounts.
- Document recovery procedures for admin MFA loss.

## Phase 5: Exchange Online And Email Security

### Goals

- Deliver reliable company email.
- Reduce spoofing and phishing exposure early.

### Email Protection Baseline

- Use the default Microsoft 365 cloud mailbox protections.
- Review and tune `Defender for Office 365 Plan 1` capabilities included in `Business Premium`.
- Protect role mailboxes with clear ownership and access controls.

### Authentication Rollout Sequence

1. Publish `SPF`
2. Enable `DKIM`
3. Publish `DMARC` with monitoring policy first
4. Review legitimate senders and alignment results
5. Tighten `DMARC` policy later

### Mail Security Rules

- No strict `DMARC` reject policy on day one.
- No third-party sender should be added to SPF without business justification.
- Every sender service must be documented before being allowed to send as the domain.

## Phase 6: Teams, SharePoint, And OneDrive

### Teams

- Enable `Teams` for internal communication and meetings.
- Start with a small number of teams:
  - Leadership
  - Operations
  - Technology
  - Company-wide
- Avoid channel sprawl early.

### SharePoint

- Use `SharePoint` for shared document libraries and formal company records.
- Create a central company site for operations, policies, and approved templates.
- Use role-based access, not broad ad hoc sharing.

### OneDrive

- Use `OneDrive` for individual work in progress.
- Encourage sharing links instead of sending file attachments by email.
- Define retention and ownership expectations for employees who leave later.

## Phase 7: Intune And Device Management

### Goals

- Make device management part of the initial company standard.
- Avoid unmanaged endpoint sprawl.

### Device Strategy

- Windows is the primary managed platform at launch.
- macOS and mobile support are allowed but should follow documented enrollment flows.
- Corporate-owned devices should be enrolled into `Intune`.

### Intune Baseline

1. Set `Intune` as the MDM authority if required by the tenant state.
2. Define enrollment restrictions.
3. Block or tightly limit personal device enrollment until policy is clear.
4. Create baseline compliance policies.
5. Create baseline configuration policies for Windows.
6. Configure automatic enrollment where applicable.
7. Prepare `Windows Autopilot` for new hardware purchases.

### Windows Baseline Controls

- BitLocker enabled
- Microsoft Defender Antivirus active
- Firewall enabled
- OS update policy configured
- Device compliance required for access to core apps

### App Deployment

- Deploy Microsoft 365 Apps in a controlled way.
- Use `Intune` app deployment for managed devices.
- Pilot app deployment before broad release if specialized add-ins or older Office versions exist.

## Phase 8: Defender For Business

### Goals

- Ensure every managed endpoint is onboarded to `Defender for Business`.
- Use one endpoint security platform from the start.

### Actions

1. Open the Microsoft Defender portal.
2. Confirm `Defender for Business` tenant availability.
3. Define endpoint onboarding method by platform.
4. Integrate device onboarding with `Intune` where possible.
5. Validate protection status on pilot devices before broad rollout.

### Baseline Policy Areas

- Antivirus and real-time protection
- Attack surface reduction where supported
- Endpoint detection and response
- Automated investigation and remediation
- Device inventory and alerting review

## Phase 9: Security And Compliance Baseline

### Required Day-1 Controls

- MFA for all users
- Separate admin accounts
- Two emergency access accounts
- Least-privilege role assignments
- Audit log review capability
- Email authentication rollout started
- Managed-device posture established

### Business Premium Controls To Use Early

- `Conditional Access`
- `Intune`
- `Defender for Business`
- `Defender for Office 365 Plan 1`
- Core Microsoft Purview / information protection features included with the plan where appropriate

### Controls To Delay Until There Is Real Operational Need

- Complex DLP rollout
- Large-scale sensitivity labeling
- Aggressive sharing restrictions that would slow early operations

## Phase 10: DNS Migration To Cloudflare

### Preconditions

- Microsoft 365 mail flow is stable
- DNS zone inventory is complete
- SPF, MX, and required verification records are confirmed
- DKIM records are known and documented
- Rollback owner is assigned

### Migration Sequence

1. Recreate the full DNS zone in `Cloudflare`
2. Verify all Microsoft 365-related DNS records
3. Change nameservers from `Loopia` to `Cloudflare`
4. Re-test mail flow, domain verification, and any website records
5. Monitor for propagation issues

## Rollout Sequence

### Week 1

- Tenant creation
- Admin account model
- Emergency access accounts
- License and billing validation

### Week 2

- Custom domain verification
- User creation
- Mailbox creation
- Core mail DNS at `Loopia`
- Pilot send/receive testing

### Week 3

- Conditional Access baseline
- Teams / SharePoint / OneDrive baseline
- Intune baseline
- Pilot device enrollment

### Week 4

- Defender for Business onboarding
- DKIM enablement
- DMARC monitoring policy
- Cloudflare DNS migration planning

### Week 5

- Cloudflare nameserver migration
- Post-cutover validation
- Final hardening pass

## Acceptance Criteria

- All staff have named user accounts on the company domain
- All admins use separate named admin accounts
- Two emergency access accounts exist and are documented
- MFA is required for all users
- Core Conditional Access policies are live
- Microsoft 365 email send/receive works for the company domain
- SPF is published, DKIM is enabled, and DMARC is published in monitoring mode
- Teams, SharePoint, and OneDrive are operational
- Pilot company devices are enrolled into Intune
- Pilot devices are onboarded into Defender for Business
- DNS is migrated to Cloudflare without breaking Microsoft 365

## Open Decisions

- Final user naming standard
- Whether every employee receives Business Premium immediately or some accounts use lower-cost licenses later
- Whether personal mobile access is allowed at launch
- Whether macOS should be a first-wave managed platform
- Which Teams / SharePoint information architecture should be used for non-technology functions

## Source References

Primary Microsoft sources used for this plan:

- Add a domain to Microsoft 365:
  https://learn.microsoft.com/en-us/office365/admin/setup/add-domain
- Sign in and set up Microsoft 365 Business Premium:
  https://learn.microsoft.com/sk-SK/microsoft-365/business-video/set-up
- Security defaults in Microsoft Entra ID:
  https://learn.microsoft.com/en-us/azure/active-directory/fundamentals/concept-fundamentals-security-defaults
- Manage emergency access admin accounts:
  https://learn.microsoft.com/en-us/azure/active-directory/roles/security-emergency-access
- Microsoft 365 for business security overview:
  https://learn.microsoft.com/en-us/microsoft-365/admin/security-and-compliance/m365b-security-overview
- What is Microsoft Defender for Business:
  https://learn.microsoft.com/en-us/microsoft-365/security/defender-business/mdb-overview
- Microsoft Defender service description:
  https://learn.microsoft.com/en-us/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-tenantlevel-services-licensing-guidance/microsoft-defender-service-description
- Windows Autopilot requirements:
  https://learn.microsoft.com/en-us/autopilot/requirements
- Add Microsoft 365 Apps to Windows devices using Intune:
  https://learn.microsoft.com/en-us/mem/intune/apps/apps-add-office365
