# Field Sales Agent API Contracts

These are proposed API contracts for the private implementation. They are public-safe examples, not runnable implementation code.

## POST /api/intake/quote-assist/analyze

Analyzes a natural-language quote request before the governed workflow starts. OpenAI may assist with extraction when enabled, but deterministic validation remains the readiness gate.

```json
{
  "message": "Westlake High School wants 50 blue helmets. Create a quote."
}
```

Response:

```json
{
  "correlation_id": "corr_demo_001",
  "intent": "quote_assist_request",
  "account_name": "Westlake High School",
  "product_request": "50 blue helmets",
  "requested_quantity": 50,
  "requested_color": "blue",
  "requested_products": [
    {
      "product_family": "helmet",
      "requested_quantity": 50,
      "requested_color": "blue"
    }
  ],
  "requires_clarification": false,
  "missing_fields": [],
  "invalid_fields": [],
  "can_start_workflow": true,
  "routing_status": "ready",
  "extraction_source": "deterministic_or_openai",
  "validation_source": "deterministic",
  "readiness_decision_source": "deterministic"
}
```

## POST /api/workflows/quote-assist/start

Starts the governed quote workflow after required facts are collected.

```json
{
  "correlation_id": "corr_demo_001",
  "customer_request": "Westlake High School wants 50 blue helmets. Create a quote.",
  "account_name": "Westlake High School",
  "product_request": "50 blue helmets",
  "requested_quantity": 50,
  "requested_color": "blue",
  "requested_products": [
    {
      "product_family": "helmet",
      "requested_quantity": 50,
      "requested_color": "blue"
    }
  ]
}
```

## GET /api/workflows/{workflow_id}

Returns final workflow state and outcome.

## GET /api/workflows/{workflow_id}/audit

Returns audit events for replay.

## GET /api/monitoring/summary

Returns workflow outcome counts, guardrail outcomes, and system health indicators.

## GET /api/cost/{correlation_id}

Returns cost telemetry for a workflow correlation.

## Draft Quote Response Example

```json
{
  "workflow_id": "wf_quote_demo_001",
  "correlation_id": "corr_demo_001",
  "workflow_type": "quote_assist",
  "workflow_status": "completed",
  "outcome": "draft_quote_created",
  "draft_quote_id": "dq_demo_001",
  "guardrail_decision": "allow",
  "requires_manager_approval": false,
  "rep_summary": "Draft quote prepared for 50 helmets. Confirm final configuration before customer signature."
}
```

## Audit Event Example

```json
{
  "event_id": "evt_demo_001",
  "workflow_id": "wf_quote_demo_001",
  "correlation_id": "corr_demo_001",
  "event_type": "product_spec_retrieved",
  "node_name": "retrieve_product_specs",
  "tool_name": "retrieve_product_specs",
  "architecture_path": "LangGraph node -> MCP tool wrapper -> LlamaIndex retrieval service -> cited evidence -> guardrail evaluation",
  "evidence_reference": "helmet_catalog.md",
  "output_summary": {
    "retrieval_role": "evidence_only",
    "decision_authority": "guardrails"
  }
}
```

## Cost Telemetry Event Example

```json
{
  "correlation_id": "corr_demo_001",
  "workflow_id": "wf_quote_demo_001",
  "workflow_type": "quote_assist",
  "intake_llm_calls": 1,
  "workflow_llm_calls": 0,
  "rag_calls": 1,
  "retrieved_chunks": 4,
  "mcp_tool_calls": 5,
  "input_tokens": null,
  "output_tokens": null,
  "total_tokens": null,
  "estimated_cost_usd": null,
  "estimated_cost_status": "not_configured"
}
```

Token values should be populated only when provider usage metadata is returned. Dollar estimates should be populated only when pricing rates are configured.

## Monitoring Summary Example

```json
{
  "workflow_type": "quote_assist",
  "total_runs": 25,
  "draft_quote_created": 14,
  "clarification_required": 4,
  "blocked_by_policy": 3,
  "approval_required": 4,
  "average_estimated_cost_usd": null,
  "estimated_cost_status": "not_configured"
}
```
