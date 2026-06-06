# Field Sales Agent Cost Telemetry

Phase 1 includes cost telemetry, not full cost-optimized orchestration. The goal is to make operating cost visible from day one so teams can understand the cost of workflow execution before adding dynamic routing, caching, or optimization.

## Telemetry Integrity Principle

Cost telemetry should be backed by real runtime metadata. The system should not fabricate token counts, dollar costs, latency values, trace links, or model usage.

If OpenAI intake is enabled and provider usage metadata is returned, intake token counts can be displayed. If workflow response drafting is not implemented, workflow LLM usage should remain zero or unavailable. If pricing rates are not configured, estimated dollar cost should be marked unavailable or not configured.

## Telemetry Fields

Track:

- `workflow_id`
- `correlation_id`
- `workflow_type`
- Intake LLM calls and usage when configured
- Workflow LLM calls and usage when implemented
- RAG calls
- Retrieved chunks
- MCP tool calls
- Input tokens when returned by provider metadata
- Output tokens when returned by provider metadata
- Total tokens when returned by provider metadata
- Estimated cost if configured rates are available
- Cost per draft quote attempt
- Cost by outcome

## Cost per Draft Quote Attempt

```text
cost_per_attempt = llm_cost + rag_cost + mcp_tool_cost + storage_cost + monitoring_cost
```

If pricing rates are not configured, the system should report usage metrics without estimating dollars.

## Cost by Outcome

Cost should be compared across outcomes:

- Clarification required
- Invalid product or identifier
- Blocked by policy
- Approval required
- Draft quote created

This helps distinguish cheap failed attempts from more expensive successful workflows and identifies where clarification, retrieval, or guardrail behavior may need improvement.

## Intake vs Workflow LLM Usage

Intake LLM usage and workflow LLM usage should be reported separately.

- Intake LLM usage supports intent/entity extraction when OpenAI intake is enabled.
- Workflow LLM usage should remain zero or unavailable until controlled workflow response drafting is implemented.
- LLM token values should appear only when provider usage metadata is available.

## Phase 1 Boundary

Do not claim these are implemented in Phase 1 unless the private implementation explicitly adds them:

- Dynamic model routing
- Semantic caching
- Prompt caching
- Cost-aware orchestration
- Automated model downgrade/upgrade decisions
- Real dollar cost estimation without configured rates

## Future Cost-Optimized Orchestration

Future enhancements may include:

- Dynamic model routing by risk and complexity
- Prompt caching for stable system instructions
- Semantic caching for repeated low-risk product explanations
- Retrieved chunk limits
- Tool-call caps
- Retry caps
- Escalation instead of repeated expensive loops
- Cost-per-outcome analytics
