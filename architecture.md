# Architecture

## Architectural Style
Agentic, orchestrated workflow architecture (planner-executor pattern) with a
deterministic control plane.

Core principle: LLMs never own side effects. They produce structured outputs,
and the orchestrator decides what happens next.

## Module Boundaries (Current Repo)
- `src/domain/` - core types, invariants, and schemas.
- `src/lib/orchestration/` - workflow engine, transition validation, policy
  evaluation, memory assembly.
- `src/lib/llm/` - prompts, reasoning, and schema-constrained outputs.
- `src/lib/executors/` - side-effect adapters (Vapi, Google APIs, web fetch).
- `src/lib/audit/` - append-only audit log.
- `src/lib/queue/` - async job queue for outreach dispatch.
- `src/app/api/` - HTTP transport only.
- `src/features/` - feature modules and UI components.

## Orchestrator Responsibilities
- Validate workflow transitions.
- Enforce policy decisions (allow, pause, escalate, block).
- Run agents to generate structured intelligence.
- Trigger executors for real-world side effects.
- Record audit events for every decision and action.
- Update workflow memory and state.

## Separation of Concerns
**Reasoning** (LLMs) and **execution** (tools) are split by design. This yields:
- deterministic behavior,
- explainable decisions,
- safer compliance posture,
- testable and isolated components.

## Safety Architecture (Current)
- **Policy gate before side effects:** every research/outreach action is
  evaluated by deterministic policies before execution.
- **Single control plane:** the orchestrator is the only component allowed to
  mutate workflow state or trigger executors.
- **Audit trail as default:** decisions, transitions, and side effects are
  recorded as immutable audit events.
- **Safe web access:** enrichment respects robots.txt and blocks unsafe URLs.
- **Queue isolation:** outreach dispatch happens asynchronously to avoid
  unsafe retries or blocking API responses.
- **Hallucination resistance:** intelligence is evidence-based, typed, and
  confidence-scored before influencing downstream actions.

## Policy-Gated Autonomy
Policies are deterministic and explicit:
- DNC and legal blocks,
- frequency and time-of-day controls,
- allowed channels,
- readiness thresholds.

Policies are evaluated before any outreach execution, and the result is logged.

## Agent Domains (Current + Planned)
The system is organized into agent domains with strict boundaries:

- **Intelligence & qualification agents (current):** research, enrichment, and
  readiness scoring. These agents output structured intelligence only and never
  trigger side effects.
- **Outreach & sales agents (current):** policy-gated voice and follow-up
  execution via Vapi and n8n, with escalation paths when policies require it.
- **Client onboarding agents (planned):** intake, documentation, integration
  setup, and handoff workflows.
- **Finance & operations agents (planned):** invoicing, payment follow-ups, and
  reporting. These workflows remain isolated from outreach logic.

All agents operate under a single orchestrator that enforces policy, audit, and
escalation rules. Agents execute actions; the orchestrator remains the sole
authority.

## Squads and Agent Composition
The system organizes agents into **squads**. A squad is a bounded execution
unit with its own agents, workflows, policies, and memory scope. Agents are
instantiated within squads and do not operate globally.

This allows multiple independent AI-operated teams to run in parallel across
industries, geographies, or internal functions. Each squad maintains its own
leads, workflows, and audit trail while sharing the same orchestration and
governance layer.

## Memory and Auditability
Workflow instances maintain structured memory snapshots. All memory updates are
written through the orchestrator and logged as audit events. This provides
replayability and forensic traceability.

## Evidence in the Codebase
The architecture is reflected directly in code:
- **Orchestrator control plane:** `src/lib/orchestration/orchestrator.ts`
- **Policy evaluation:** `src/lib/orchestration/policyEvaluator.ts`
- **Memory assembly:** `src/lib/orchestration/memoryAssembler.ts`
- **Executors (side effects):** `src/lib/executors/`
- **Audit log:** `src/lib/audit/auditLog.ts`
- **Queueing:** `src/lib/queue/jobQueue.ts`
