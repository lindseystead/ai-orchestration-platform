# Auditability and Observability

## Audit-First Design
Every decision and side effect is recorded, including:
- policy evaluations and outcomes,
- workflow state transitions,
- memory updates,
- executor dispatches.

This provides a full forensic trail for compliance, debugging, and QA.

## Replayability
Because workflows are deterministic and memory is versioned, it is possible to
reconstruct the exact decision path for any lead.

## Data Hygiene
Sensitive values are sanitized before being logged. The system uses safe URL
validation to prevent SSRF and related issues.

## Safety Through Observability
Every policy decision is logged with rationale and confidence, enabling review
of why an action was allowed, paused, or blocked. This makes it practical to
audit AI-driven workflows at scale and intervene when policies require it.

Research outputs are stored with confidence and crawl metadata, creating a
clear trail for spotting weak or speculative signals.

## Evidence in Code
- Audit logging: `src/lib/audit/auditLog.ts`
- Policy logging: `src/lib/orchestration/policyEvaluator.ts`
- Memory updates: `src/lib/orchestration/memoryAssembler.ts`
