# Workflows

## End-to-End Flow (Current)
1. **Lead Discovery**: Google Places search yields candidate businesses.
2. **Lead Ingestion**: Candidates are normalized and deduplicated.
3. **Enrichment**: Ethical web crawl gathers evidence and signals.
4. **Intelligence Synthesis**: LLMs produce structured intelligence and
   readiness scoring.
5. **Policy Evaluation**: Deterministic policies validate eligibility.
6. **Outreach Dispatch**: Vapi calls and n8n follow-ups are executed.
7. **Audit Logging**: All actions are recorded for replayability.

Each step is mediated by the orchestrator so LLM outputs cannot trigger side
effects directly. This preserves deterministic control at scale.

## End-to-End Flow (Planned)
1. **Client Onboarding**: automated intake, policy configuration, and document
   collection.
2. **Integration Provisioning**: connect CRM, email, and data sources.
3. **Operations & Finance**: invoicing, payment tracking, and reporting.

Planned steps are gated by explicit policies and are not enabled without human
approval and audit logging.

## Workflow State Model
Workflows are stateful and time-aware. The system is designed for:
- pause and resume,
- deterministic retries,
- escalation to human approval when policies require it.

## Workflow Governance
Every step is policy-gated and logged. If a policy blocks or pauses a step, the
workflow records the rationale and moves to a safe state rather than continuing
silently.

## Squad Ownership
Each squad owns its workflow instances, enabling isolation of logic and
experiments across multiple go-to-market tracks.
