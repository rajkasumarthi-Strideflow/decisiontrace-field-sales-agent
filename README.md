# Field Sales Agent

**DecisionTrace — Quote Assist Agent**

Field Sales Agent is a DecisionTrace AI reference app for governed field-sales workflows. The first workflow, Quote Assist Agent, helps a mobile sales rep prepare a grounded, approval-aware draft quote using account context, product/spec retrieval, inventory checks, pricing rules, quote guardrails, audit replay, cost telemetry, and monitoring.

Warranty Replacement proved the DecisionTrace control model using governed Python tool wrappers. Field Sales Agent demonstrates the same control model with an MCP-mediated tool/resource abstraction and a LlamaIndex-backed product/spec retrieval capability.


## Live Demo

Live demo: <Railway demo URL>

Phase 1 status: Deployed release candidate validated.

Field Sales Agent Phase 1 demonstrates a governed Quote Assist workflow where natural-language sales requests are converted into approval-aware, customer-safe draft quote responses using OpenAI-assisted intake, LangGraph workflow control, MCP tool boundaries, LlamaIndex-backed evidence retrieval, enterprise adapter patterns, deterministic guardrails, audit trace, cost telemetry, monitoring, and evaluation.

## Control Model Principle

LangGraph controls the workflow.
MCP exposes bounded tools/resources.
LlamaIndex backs the bounded MCP `retrieve_product_specs` capability for grounded product/spec evidence.
Guardrails control quote readiness and approval boundaries.
Cost telemetry measures operating cost from actual execution metadata.
Audit, tracing, monitoring, and evaluations validate the control model.

## Current Public Status

This public repo contains architecture and demo artifacts for the Field Sales Agent reference app. It reflects the current public-safe architecture direction through the validated Phase 1 release candidate:

- Natural-language intake is the primary entry path.
- Intake validates required facts before the governed workflow can start.
- OpenAI may assist with intake extraction when enabled, but deterministic validation remains the readiness gate.
- LangGraph remains the workflow control layer.
- Workflow nodes use bounded MCP-style capabilities.
- `retrieve_product_specs` is a LlamaIndex-backed MCP retrieval capability.
- Product/spec retrieval provides cited evidence only; it does not approve quotes or create actions.
- Optional OpenAI rep-facing response drafting occurs only after the governed outcome is determined.
- Deterministic validation checks any OpenAI-drafted rep response before display.
- Audit Trace and Cost Telemetry are separate concepts.
- Telemetry must come from real workflow execution, provider usage metadata, or configured rates. It should not be fabricated.

## What This Public Repo Contains

This public repo contains architecture and demo artifacts for the Field Sales Agent reference app. It is intended for portfolio review, architecture discussion, implementation planning, and public-safe demonstration of the DecisionTrace control model.

Included artifacts:

- Executive summary for enterprise and architecture audiences
- Lightweight PDR for Quote Assist Agent
- Architecture overview and public/private boundary
- C4 context and container diagrams
- Workflow, retrieval, cost telemetry, and rollout diagrams
- MCP-mediated LlamaIndex retrieval pattern explanation
- Public-safe API contract examples
- Cost telemetry design
- Rollout and rollback design
- Synthetic demo scenarios
- Demo script and public-safe roadmap

## Public / Private Repo Boundary

This public repo contains architecture and demo artifacts. The full runnable implementation, MCP server, LlamaIndex pipeline, database/schema details, cost telemetry logic, and reusable accelerator internals should remain in a private implementation repo.

This repository does not include proprietary implementation code, private database schemas, secrets, reusable accelerator internals, or a full runnable backend implementation.

## Phase 1 Workflow

Quote Assist Agent supports a governed field-sales scenario where a rep asks for a draft quote before a customer meeting. The workflow gathers required facts, retrieves account and product evidence, checks inventory, applies deterministic pricing rules, evaluates quote guardrails, creates a draft quote only when allowed, and records audit and cost telemetry.

The workflow is draft-only. It does not submit final orders, collect payment, trigger shipment, or create unsupported customer commitments.

## Correct Retrieval Pattern

LangGraph does not call LlamaIndex directly. LangGraph calls the bounded MCP capability `retrieve_product_specs`. That MCP capability is backed by LlamaIndex retrieval over product/spec docs.

```text
LangGraph node
-> MCP tool wrapper: retrieve_product_specs
-> LlamaIndex retrieval service
-> product/spec docs
-> cited evidence
-> MCP result
-> LangGraph workflow state
-> guardrail evaluation
```

`retrieve_product_specs` is evidence retrieval only. It does not approve quotes, create draft quotes, calculate pricing, check inventory, or override guardrails.

## Repository Structure

```text
README.md
docs/
  executive-summary.md
  pdr.md
  architecture.md
  diagrams.md
  api-contracts.md
  cost-telemetry.md
  rollout-rollback.md
  demo-script.md
  roadmap.md
examples/
  synthetic-scenarios.md
  sample-api-payloads.json
```

## Documentation Index

- [Executive Summary](docs/executive-summary.md)
- [Product Definition Requirements](docs/pdr.md)
- [Architecture Overview](docs/architecture.md)
- [Architecture Diagrams](docs/diagrams.md)
- [API Contract Examples](docs/api-contracts.md)
- [Cost Telemetry Design](docs/cost-telemetry.md)
- [Rollout and Rollback](docs/rollout-rollback.md)
- [Demo Script](docs/demo-script.md)
- [Phase 1 Public Release Notes](docs/release-notes-phase-1.md)
- [Roadmap](docs/roadmap.md)
- [Synthetic Scenarios](examples/synthetic-scenarios.md)
- [Sample API Payloads](examples/sample-api-payloads.json)
