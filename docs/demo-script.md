# Field Sales Agent Demo Script

## Purpose

This is a 5-minute demo narrative for the DecisionTrace — Quote Assist Agent reference workflow.

## 1. Opening

Field Sales Agent is a DecisionTrace AI reference app for governed field-sales workflows. The first workflow is Quote Assist Agent, which helps a mobile rep prepare a grounded draft quote without creating a final order or unsupported commitment.

## 2. Control Model

This app uses the same DecisionTrace control model as the warranty workflow, but with a different tool access pattern:

- LangGraph controls workflow state and routing.
- MCP-style tools/resources provide bounded access to account, inventory, pricing, quote, and product/spec capabilities.
- LlamaIndex backs the bounded MCP `retrieve_product_specs` capability for cited product/spec evidence.
- Guardrails decide whether draft quote creation is allowed, blocked, or requires approval.
- Cost telemetry is captured from real execution metadata.

## 3. Scenario

A rep says:

> “I’m meeting Westlake High School. Prepare a quote for 50 varsity helmets in matte navy.”

## 4. Walkthrough

1. Show natural-language intake and required-facts extraction.
2. Explain that the router interprets the request but does not create quotes.
3. Show that the governed workflow starts only when required facts are present and valid.
4. Show account context retrieval from synthetic CRM data.
5. Show `retrieve_product_specs` as a LlamaIndex-backed MCP retrieval capability.
6. Explain the pattern: LangGraph node -> MCP tool wrapper -> LlamaIndex retrieval service -> product/spec docs -> cited evidence -> guardrail evaluation.
7. Emphasize that retrieval provides evidence only; it does not approve quotes or create actions.
8. Show inventory and pricing checks through MCP-style tools.
9. Show quote guardrail decision.
10. Show draft quote created only if controls pass.
11. Show Audit Trace: business event, LangGraph node, MCP tool, enterprise adapter, endpoint, evidence, and guardrail decision.
12. Show Cost Telemetry: intake LLM usage separated from workflow LLM usage, with no fabricated token or dollar values.

## 5. Key Message

The AI assists the rep, but the control model governs the workflow. The app is draft-only, evidence-grounded, approval-aware, auditable, and cost-visible.

## 6. Closing

Field Sales Agent demonstrates how DecisionTrace AI can extend from support workflows into revenue workflows while preserving the same governance pattern: route, retrieve, validate, guard, audit, measure, and improve.

## Phase 1 Validation Status

The deployed Phase 1 release candidate has been validated across the valid quote, inventory unavailable, unsupported product/configuration, final order/shipment request, and missing details scenarios. The demo remains draft-only: no final order, payment, shipment, or fulfillment is created.
