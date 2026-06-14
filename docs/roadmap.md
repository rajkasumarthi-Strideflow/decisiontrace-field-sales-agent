# Field Sales Agent Roadmap

## Phase 1

- Quote Assist Agent MVP
- Natural-language intake readiness gate
- Optional OpenAI-assisted intake extraction with deterministic validation
- MCP-style tools/resources
- LlamaIndex-backed MCP `retrieve_product_specs` capability
- Cost telemetry with intake/workflow drafting usage separation
- Optional validated OpenAI rep-facing response drafting
- Draft quote workflow
- Audit Trace and Cost Telemetry UI concepts
- Audit/monitoring/evaluation architecture
- Synthetic account/product/inventory/pricing data
- Public-safe API contract examples
- Controlled rollout/rollback design

## Phase 1 Validation Status

Phase 1 is a deployed release candidate validated against the public demo scenarios. The validated release confirms the Quote Assist control model, audit trace, cost telemetry, monitoring, and evaluation narrative while preserving the public/private implementation boundary.

## Next Priority: Agentforce Implementation Mapping

Field Sales Agent remains tool-agnostic, but the next portfolio step is to map the architecture into an Agentforce implementation model. This will show how the same enterprise AI control patterns — workflow orchestration, bounded actions, data grounding, policy guardrails, audit trace, cost telemetry, monitoring, and evaluation — can accelerate Salesforce and Agentforce delivery without hard-coding the architecture to one platform.

Planned Phase 2B outputs:

- Agentforce mapping diagram
- Field Sales Agent node-to-Agentforce concept mapping
- action/tool boundary mapping
- Flow/Apex/MuleSoft integration boundary mapping
- policy guardrail placement
- auditability and evaluation checklist
- implementation planning checklist
- demo talk track for Salesforce / Agentforce audiences

Agentforce mapping is planned as an architecture and implementation planning artifact. It does not imply a live Salesforce integration in the current demo.

## Reordered Phase 2 Roadmap

1. **Phase 2B — Agentforce Implementation Mapping**
2. **Phase 2C — Evaluation and Release Quality Gates**
3. **Phase 2D — Cost Rate Configuration and Cost-per-Outcome Analytics**
4. **Phase 2E — Human Approval Handoff Pattern**
5. **Later — Policy Version Promotion / Policy Workbench**

## Later Enhancements

- LangSmith evaluation
- LLM-as-judge evaluation
- CrewAI review crew if needed
- Persistent production database
- Production authentication/security hardening
- Persistent intake funnel metrics
- Production hardening for workflow response drafting
- Dynamic model routing
- Prompt caching
- Semantic caching
- Voice interface
- Tau-bench-inspired dynamic evaluation
- Persistent trace-to-evaluation improvement loop
- Enterprise API adapter framework

## Public-Safe Boundary

The roadmap describes architecture direction. Implementation details, internal accelerator logic, reusable private components, and proprietary schemas should remain in a private implementation repo.

## Executive Summary

For an executive and architecture-leader overview of the reference app, see [Field Sales Agent Executive Summary](executive-summary.md).
