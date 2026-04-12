# Documentation Standards

## Objective

Keep documentation useful, current, and easy to trust.

## Core Rules

- Every document should have a clear owner.
- Every document should state its last updated date.
- Prefer concise working documents over long narrative documents.
- Avoid duplicating facts across multiple files.
- If a decision affects architecture, security, operations, or delivery, capture it as an ADR.

## Document Header Standard

Use this header at the top of important documents:

```md
# Title

- Owner:
- Status:
- Last Updated:
- Related Docs:
```

## Status Values

- `draft`
- `active`
- `deprecated`
- `archived`

## Review Cadence

- Strategy and roadmap: monthly
- Engineering plans: biweekly or monthly
- Architecture: when systems change
- Runbooks: after every incident and quarterly
- Security docs: quarterly minimum

## Naming

- Use lowercase file names with hyphens.
- Keep names explicit and searchable.
- One topic per file.
