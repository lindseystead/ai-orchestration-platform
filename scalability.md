# Scalability and Reliability

## Scalability Choices
- **Orchestrator-first design** enables horizontal scaling with stateless
  executors.
- **Workflow instances** allow versioned changes without breaking existing runs.
- **Queue-based dispatch** prevents API blocking and supports burst handling.
- **SQLite today, Postgres tomorrow**: the data layer is structured to migrate.

## Reliability and Safety
- Deterministic policies reduce unsafe automation.
- Idempotent executors prevent double side effects.
- Bounded retries and clear escalation paths avoid runaway workflows.

## Future Scaling Path
The architecture allows:
- external queue integration,
- multi-tenant isolation,
- cross-squad policy segmentation,
- multi-region orchestration if required.

## Evidence in Code
- Job queue: `src/lib/queue/jobQueue.ts`
- Workflow state: `src/db/workflowState.repo.ts`
