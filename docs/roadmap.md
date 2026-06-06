# Field Sales Agent Roadmap

## Phase 1

- Quote Assist Agent MVP
- Natural-language intake readiness gate
- Optional OpenAI-assisted intake extraction with deterministic validation
- MCP-style tools/resources
- LlamaIndex-backed MCP `retrieve_product_specs` capability
- Cost telemetry with intake/workflow usage separation
- Draft quote workflow
- Audit Trace and Cost Telemetry UI concepts
- Audit/monitoring/evaluation architecture
- Synthetic account/product/inventory/pricing data
- Public-safe API contract examples
- Controlled rollout/rollback design

## Future Enhancements

- Persistent intake funnel metrics
- Controlled workflow response drafting
- Dynamic model routing
- Prompt caching
- Semantic caching
- Configured rate cards for dollar cost estimation
- Cost-per-outcome analytics
- Rego/OPA policy-as-code
- Voice interface
- Salesforce/CPQ integration
- Agentforce mapping
- CrewAI review crew if needed
- τ-Bench-inspired dynamic evaluation
- Persistent trace-to-evaluation improvement loop
- Production-grade authentication and authorization
- Enterprise API adapter framework
- Human approval workflow integration

## Public-Safe Boundary

The roadmap describes architecture direction. Implementation details, internal accelerator logic, reusable private components, and proprietary schemas should remain in a private implementation repo.
