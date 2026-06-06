# Field Sales Agent Architecture

## Purpose

Field Sales Agent demonstrates how the DecisionTrace control model can be applied to governed field-sales workflows. The reference workflow, Quote Assist Agent, helps a rep prepare a grounded draft quote while keeping high-risk business actions under deterministic control.

## Architecture Pattern

Field Sales Agent is an architecture/demo repository, not a full public implementation. It describes a public-safe implementation pattern where LangGraph manages stateful workflow execution, MCP-style interfaces expose bounded tools and resources, LlamaIndex retrieves product/spec evidence behind the MCP boundary, and deterministic guardrails control draft quote readiness.

## Same Control Model, New Tool Access Pattern

Warranty Replacement:

```text
LangGraph node -> governed Python tool wrapper -> simulated/future enterprise API adapter
```

Field Sales Agent:

```text
LangGraph node -> MCP tool wrapper -> enterprise API adapter / LlamaIndex-backed retrieval -> synthetic systems and product/spec docs
```

The control principle stays the same: workflow state and guardrails control execution. The access pattern changes from direct Python tool wrappers to MCP-style tools/resources and a retrieval layer.

## Core Components

- **Natural-language intake router:** Interprets rep requests, extracts required facts, asks clarifying questions, and gates workflow start.
- **LangGraph workflow:** Controls state, node execution, routing, outcomes, and correlation.
- **MCP client/tool interface:** Gives workflow nodes bounded access to tools and resources.
- **MCP server/tool layer:** Public-safe pattern for account, inventory, pricing, quote, and product/spec resource access.
- **LlamaIndex-backed retrieval capability:** Powers the bounded MCP `retrieve_product_specs` tool for grounded product/spec evidence.
- **Quote guardrails:** Determine whether draft quote creation is allowed, blocked, or requires approval.
- **Audit trace:** Records workflow events, evidence references, decisions, and outcomes.
- **Cost telemetry:** Tracks intake LLM usage, workflow LLM usage, MCP calls, RAG calls, retrieved chunks, and estimated cost only when rates are configured.
- **Monitoring/evaluation layer:** Validates workflow health, control outcomes, and golden scenarios.

## Intake Readiness Gate

Natural-language intake is the primary entry path. The router may classify intent and extract account, product, quantity, color, discount, and requested product details. It can use OpenAI-assisted extraction when explicitly enabled, but deterministic normalization and validation decide readiness.

The intake router does not retrieve account context, query product/spec evidence, check inventory, calculate pricing, evaluate guardrails, approve quotes, or create draft quotes. Those responsibilities remain inside the governed LangGraph workflow.

## MCP Tool/Resource Layer

The MCP-style layer separates workflow control from enterprise data access. Tools and resources should be bounded, typed, and auditable.

Example public-safe interfaces:

- `get_account_context(account_name_or_id)`
- `retrieve_product_specs(query, filters)`
- `search_products(query, filters)`
- `check_inventory(product_id, quantity, configuration)`
- `calculate_quote_lines(items, discount_code)`
- `create_draft_quote(account_id, quote_lines, guardrail_result)`
- `get_quote_policy(policy_version)`

Tools should return summarized, minimum necessary data. They should not expose private schemas or sensitive customer records in public artifacts.

## LlamaIndex-Backed MCP Retrieval

LangGraph does not call LlamaIndex directly. LangGraph calls the bounded MCP capability `retrieve_product_specs`. That MCP capability is backed by LlamaIndex retrieval over product/spec documents.

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

The retrieval layer should support:

- Product family filters
- Spec/version references
- Compatibility evidence
- Unsupported claim detection
- Evidence IDs for audit replay
- Retrieved chunk counts for cost telemetry

LlamaIndex helps organize retrieval, metadata filtering, citation-ready evidence, and future evaluation workflows.

## Audit Trace UI Concepts

Audit Trace should explain what happened and why. Public-safe trace displays should distinguish:

- Business event
- Raw event type
- LangGraph node
- MCP tool
- Enterprise adapter
- Mock enterprise endpoint
- Correlation ID
- Safe metadata
- Cited evidence
- Guardrail decision

For `retrieve_product_specs`, the UI should clearly label it as a LlamaIndex-backed MCP retrieval capability:

> LlamaIndex-backed MCP tool: retrieves product/spec evidence with citations. It does not approve quotes or create actions.

## Cost Telemetry Layer

Cost telemetry is included in Phase 1 design, even though cost-optimized orchestration is future work. The platform should track:

- Intake LLM calls and token usage when provider metadata is returned
- Workflow LLM calls and token usage when response drafting is implemented
- RAG calls and retrieved chunks
- MCP tool calls
- Estimated cost when configured rates are available
- Cost per draft quote attempt
- Cost by workflow outcome

If provider usage metadata or pricing rates are not available, the UI should show that values are unavailable or not configured rather than inventing numbers.

## Audit / Trace / Monitoring / Evaluation

The control model is validated through multiple evidence layers:

- **Audit:** Business-readable event history for replay.
- **Trace:** Technical execution path and spans.
- **Monitoring:** Operational and control health metrics.
- **Evaluation:** Golden scenarios that validate required behavior before rollout.

Production issues should eventually become regression tests, but this public repo describes that architecture rather than implementing it.

## Public vs Private Repo Boundary

This public repo documents the architecture and demo design. A private implementation repo should hold runnable backend code, MCP server logic, LlamaIndex ingestion/retrieval code, private schema details, credentials, reusable accelerator internals, and deployment-specific configuration.
