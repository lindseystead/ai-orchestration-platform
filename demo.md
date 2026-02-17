# End-to-End Walkthrough (Sanitized)

This walkthrough describes the real workflow without exposing proprietary
customer data or internal secrets.

## 1) Lead Discovery
Operator triggers a Google Places search from the dashboard with a location and
category. The system converts results into normalized lead candidates and
deduplicates against existing records.

## 2) Lead Ingestion
Candidates are stored with source metadata, external IDs, and baseline contact
fields. The orchestrator initializes a workflow instance for each lead.

## 3) Intelligence Enrichment
The research agent selects crawl targets and the web executor fetches static
HTML with robots.txt compliance. Evidence is extracted into structured signals
(technology, features, business hours, team details).

## 4) Intelligence Synthesis
LLMs convert evidence into structured business intelligence and compute AI
readiness scoring with confidence, rationale, and recommendations.

## 5) Policy Evaluation
Policies are evaluated deterministically (DNC, frequency, allowed channels,
time rules, readiness thresholds). Any block or pause is logged with rationale.

## 6) Outreach Dispatch
For approved workflows, the orchestrator triggers Vapi voice calls and an n8n
follow-up workflow. Jobs are queued to avoid blocking API responses.

## 7) Audit and Oversight
Every action produces audit events. The dashboard displays lead intelligence,
workflow state, and policy outcomes for operator visibility.
