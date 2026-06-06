# Field Sales Agent Diagrams

These Mermaid diagrams are public-safe architecture diagrams for the DecisionTrace — Quote Assist Agent reference workflow.

## 1. C4 Context Diagram

```mermaid
flowchart LR
  Rep["Field Sales Rep"] --> UI["Field Sales Agent UI"]
  UI --> API["DecisionTrace API Layer"]
  API --> Workflow["Quote Assist LangGraph Workflow"]
  Workflow --> MCP["MCP Tool/Resource Layer"]
  MCP --> CRM["Synthetic CRM Data"]
  MCP --> Inventory["Synthetic Inventory Data"]
  MCP --> Pricing["Pricing Rules"]
  MCP --> RAG["LlamaIndex Product/Spec Retrieval"]
  Workflow --> Audit["Audit Trace / Monitoring"]
  Workflow --> Cost["Cost Telemetry"]
  Reviewer["Manager / Reviewer"] --> UI
```

## 2. C4 Container Diagram

```mermaid
flowchart TB
  subgraph App["Field Sales Agent Reference App"]
    UI["Browser Demo UI"]
    API["FastAPI-style API Contracts"]
    Router["Intake Router"]
    Graph["LangGraph Quote Workflow"]
    Guardrails["Quote Guardrails"]
    Audit["Audit Event Store"]
    Cost["Cost Telemetry Store"]
    Monitor["Monitoring Summary"]
  end

  subgraph MCPZone["MCP-style Access Layer"]
    Client["MCP Client Interface"]
    Server["MCP Server"]
    Resources["Tools and Resources"]
  end

  subgraph Retrieval["Product Evidence"]
    Index["LlamaIndex Retrieval Service"]
    Specs["Synthetic Product Specs"]
  end

  UI --> API --> Router --> Graph
  Graph --> Guardrails
  Graph --> Client --> Server --> Resources
  Resources --> Index --> Specs
  Graph --> Audit
  Graph --> Cost
  Audit --> Monitor
  Cost --> Monitor
```

## 3. Phase 1 Workflow Sequence

```mermaid
sequenceDiagram
  participant Rep as Field Sales Rep
  participant Router as Intake Router
  participant Graph as LangGraph Workflow
  participant MCP as MCP Tools/Resources
  participant RAG as LlamaIndex Retrieval
  participant Guard as Quote Guardrails
  participant Audit as Audit/Cost Telemetry

  Rep->>Router: Natural-language quote request
  Router->>Router: Extract account, products, quantities, discount intent
  Router-->>Rep: Clarification if required facts missing
  Router->>Graph: Start quote assist workflow with validated fields
  Graph->>MCP: Retrieve synthetic account context
  Graph->>MCP: retrieve_product_specs(query, filters)
  MCP->>RAG: Retrieve product/spec evidence
  RAG-->>MCP: Cited evidence
  MCP-->>Graph: MCP result with evidence IDs
  Graph->>MCP: Check inventory
  Graph->>MCP: Calculate pricing and discount
  Graph->>Guard: Evaluate quote readiness
  Guard-->>Graph: allow / block / approval_required
  Graph->>MCP: Create draft quote only if allowed
  Graph->>Audit: Record events, trace, cost telemetry
  Graph-->>Rep: Rep-facing summary and caveats
```

## 4. MCP + LlamaIndex Retrieval Sequence

```mermaid
sequenceDiagram
  participant Node as LangGraph Node
  participant MCP as MCP Tool Wrapper
  participant Index as LlamaIndex Retrieval Service
  participant Corpus as Product/Spec Docs
  participant Guard as Guardrail Evaluation
  participant Audit as Audit/Cost

  Node->>MCP: retrieve_product_specs(query, filters)
  MCP->>Index: Query with metadata filters
  Index->>Corpus: Retrieve candidate chunks
  Corpus-->>Index: Product/spec evidence
  Index-->>MCP: Ranked evidence with citations
  MCP-->>Node: Evidence summary, citations, metadata
  Node->>Guard: Evaluate state using cited evidence
  Node->>Audit: Record retrieval event, chunks, evidence references
```

## 5. Cost Telemetry Flow

```mermaid
flowchart LR
  Workflow["Workflow Execution"] --> IntakeLLM["Intake LLM Usage When Configured"]
  Workflow --> WorkflowLLM["Workflow LLM Usage When Implemented"]
  Workflow --> RAG["RAG Call Metrics"]
  Workflow --> MCP["MCP Tool Call Metrics"]
  IntakeLLM --> Cost["Cost Telemetry Event"]
  WorkflowLLM --> Cost
  RAG --> Cost
  MCP --> Cost
  Cost --> Store["Telemetry Store"]
  Store --> Summary["Cost by Workflow / Outcome"]
  Summary --> Dashboard["Monitoring Dashboard Design"]
```

## 6. Controlled Rollout / Rollback Flow

```mermaid
flowchart TD
  Local["Local Validation"] --> Golden["Golden Scenario Evaluation"]
  Golden --> Stage["Staging Deployment"]
  Stage --> Demo["Limited Demo Rollout"]
  Demo --> Monitor["Monitoring Checks"]
  Monitor --> Decision{"Health OK?"}
  Decision -- Yes --> Expand["Expand Rollout"]
  Decision -- No --> Trigger["Rollback Trigger"]
  Trigger --> Rollback["Rollback to Previous Version"]
  Rollback --> Review["Post-Rollback Audit Review"]
  Review --> Fix["Fix and Regression Test"]
  Fix --> Golden
```


## 7. Post-Outcome Response Drafting Boundary

```mermaid
sequenceDiagram
  participant Graph as LangGraph Workflow
  participant Guard as Quote Guardrails
  participant Draft as Draft Quote Tool
  participant OpenAI as OpenAI Drafting
  participant Validator as Deterministic Validator
  participant UI as Rep UI

  Graph->>Guard: Determine allow / approval_required / block
  alt Guardrail allows draft quote
    Graph->>Draft: Create draft quote
    Draft-->>Graph: Draft quote artifact
  else Approval required or blocked
    Guard-->>Graph: No draft quote created
  end
  Graph->>OpenAI: Optional response drafting after outcome
  OpenAI-->>Validator: Draft response
  Validator-->>Graph: pass or fallback required
  Graph-->>UI: Validated OpenAI summary or deterministic fallback
```
