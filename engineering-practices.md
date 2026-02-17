# Engineering Practices

This document outlines the engineering constraints and safety guarantees implemented in this repository.  
Every claim below maps to code paths present in the current codebase.

## Architectural Guardrails
- **Separation of concerns:** LLM reasoning is isolated from tool execution.
- **Single authority:** the orchestrator is the only component allowed to
  mutate workflow state or trigger side effects.
- **Idempotent executors:** side-effect adapters are structured to avoid
  duplicate actions.

## Determinism and Safety
- **Policy gating:** deterministic policies are evaluated before outreach.
- **Bounded retries and pauses:** workflows can pause or escalate rather than
  looping indefinitely.
- **Queue-based dispatch:** outreach jobs are executed asynchronously to avoid
  blocking API responses.
- **Robots.txt compliance:** web research respects published crawl rules.
- **Input safety checks:** URL validation blocks unsafe fetch targets.

## Hallucination and Error Controls
- **Evidence-first extraction:** research uses deterministic HTML signatures and
  extraction rules instead of free-form generation.
- **Structured intelligence:** outputs are stored as typed, schema-shaped data,
  reducing ambiguous text generation.
- **Confidence signals:** research and readiness outputs include confidence and
  review flags to avoid acting on weak signals.
- **Policy thresholds:** confidence and readiness thresholds can pause or block
  automation via deterministic policy gates.
- **Default safety preset:** baseline thresholds are loaded by the control plane
  and can be overridden per squad or lead.

## Auditability and Traceability
- **Append-only audit log:** all policy decisions and memory updates are logged.
- **Replayability:** workflow memory snapshots allow reconstruction of decisions.
- **Sanitized logging:** sensitive values are masked before logging.

## Security and Compliance Posture
- **SSRF-safe fetching:** URLs are validated before web access.
- **Explicit policy enforcement:** DNC, time, and channel rules are enforced in
  deterministic code paths.
- **Minimal PII exposure:** only required fields are processed and logged.

## Type Safety and Domain Modeling
- **Domain types:** structured intelligence and readiness scoring are typed.
- **Schema validation:** lead candidate generation validates schema conformance.
- **Clear module boundaries:** domain, orchestration, executors, and UI are
  cleanly separated.

## Code Evidence
- Orchestrator and policy logic: `src/lib/orchestration/`
- Audit logging: `src/lib/audit/`
- Executor boundaries: `src/lib/executors/`
- Domain modeling: `src/domain/`
