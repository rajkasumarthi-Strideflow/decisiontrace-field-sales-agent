# 2-Minute Demo Talk Track

“Field Sales Agent is a governed enterprise AI workflow reference app. The point is not to show another chatbot. The point is to show the control architecture an enterprise needs around AI-assisted work.

A sales rep enters a natural-language request. OpenAI can help extract the account, product, quantity, color, and discount intent, but deterministic validation decides whether the workflow is ready to start.

Once the workflow starts, LangGraph controls the sequence. It calls bounded MCP tools instead of directly coupling the LLM to backend systems. Those tools retrieve account context, product evidence, inventory, pricing, and draft quote capabilities. Product/spec evidence is retrieved through an MCP capability backed by LlamaIndex, but LlamaIndex only provides cited evidence. It does not approve quotes or create actions.

Guardrails decide whether the system can create a draft quote, requires approval, blocks an unsupported product, or asks for clarification. OpenAI can draft the rep-facing response only after the workflow outcome is already determined, and a deterministic validator checks that the response does not overpromise or contradict the workflow state.

The demo separates the business/operator experience from the architecture/governance experience. Workflow Console shows the request, outcome, concise business summary, and validated customer-safe response. Audit Trace shows node input, node output, external request, external response, MCP/tool calls, guardrail decisions, RAG evidence, OpenAI drafting payload, and audit metadata.

The demo also includes Cost Telemetry, Monitoring, and golden-scenario evaluation. That is the core enterprise story: natural language in, governed workflow out.”
