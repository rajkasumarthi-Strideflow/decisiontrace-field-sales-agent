# Field Sales Agent Phase 1 Public Release Notes

## Release Summary

Field Sales Agent Phase 1 is a public architecture/demo package for a governed field-sales Quote Assist workflow. It demonstrates the DecisionTrace control model for converting natural-language sales requests into approval-aware, customer-safe draft quote responses without creating final orders, payments, shipments, or fulfillment actions.

## What the Demo Shows

- natural-language sales request
- OpenAI-assisted intake extraction
- deterministic readiness validation
- LangGraph workflow control
- MCP-mediated enterprise capability boundary
- LlamaIndex-backed evidence retrieval
- enterprise adapter pattern
- quote guardrails
- validated customer-safe response
- audit trace
- cost telemetry
- monitoring dashboard
- golden-scenario evaluation

## Validated Scenarios

- Valid Quote -> `draft_quote_created`
- Inventory Unavailable -> `approval_required`
- Unsupported Product / Configuration -> `blocked_by_policy`
- Final Order / Shipment Request -> blocked at intake
- Missing Details -> `clarification_required`

## Architecture Boundary

LangGraph does not call LlamaIndex directly. LangGraph calls the MCP retrieval capability. The MCP retrieval capability is backed by LlamaIndex. LlamaIndex retrieves product/spec evidence only. Guardrails decide quote readiness. OpenAI drafts language only after the outcome is determined.

Correct evidence path:

```text
LangGraph node
-> MCP tool wrapper: retrieve_product_specs
-> LlamaIndex retrieval service
-> product/spec docs
-> cited evidence
-> guardrail evaluation
```

## Public / Private Boundary

This public repo contains architecture, demo narrative, diagrams, API examples, release notes, and executive summary. The private repo contains the runnable implementation and reusable accelerator internals.

Do not place secrets, API keys, private database files, implementation source, real customer data, real product data, or proprietary accelerator internals in this public repo.

## Demo Experience Separation

The demo separates the business/operator experience from the architecture/governance experience. Workflow Console shows concise outcome summaries and the validated customer-safe response. Audit Trace shows detailed execution evidence, including node input, node output, external request, external response, and audit metadata.

## Release Boundary

Phase 1 is a validated demo release candidate, not a production deployment claim. It uses synthetic data, public-safe architecture descriptions, and demo scenarios to explain the control model.
