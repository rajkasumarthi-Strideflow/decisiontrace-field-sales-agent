# Phase 2A Release Notes: Discount Policy-as-Code Guardrail

## Release Summary

Phase 2A adds a policy-as-code discount approval guardrail to the DecisionTrace — Quote Assist Agent reference app. The implementation demonstrates how a business control can be externalized into a versioned policy artifact while the governed workflow, guardrails, audit trace, and response validation remain intact.

This is a public-safe architecture note. The runnable implementation remains in the private implementation repo.

## What Phase 2A Demonstrates

- Discount authority is evaluated through a policy-as-code layer.
- Standard discounts can proceed to governed draft quote creation.
- Manager-level discounts require approval before draft quote creation.
- Excessive discounts are blocked by policy.
- OpenAI does not decide discount approval.
- LangGraph remains the workflow control layer.
- Guardrails enforce whether draft quote creation is allowed.
- Audit Trace records the policy decision path.

## Demo Scenarios

| Scenario | Prompt | Expected Outcome |
| --- | --- | --- |
| Standard Discount | `Create a draft quote for Westlake High School for 50 varsity helmets in matte navy with a 5% discount.` | Draft quote created |
| Discount Approval Scenario | `Create a draft quote for Westlake High School for 50 varsity helmets in matte navy with a 15% discount.` | Approval required, no draft quote |
| Discount Blocked | `Create a draft quote for Westlake High School for 50 varsity helmets in matte navy with a 25% discount.` | Blocked by policy, no draft quote |

## Audit Trace Evidence

Audit Trace now includes policy evaluation evidence for discount authority. The policy event separates:

- node input
- external request
- external response
- node output
- audit metadata

This lets reviewers see the matched policy rule, policy version, decision, reason, and explicit evidence that the decision was not made by the LLM.

## Control Model Message

OpenAI assists with language. LangGraph controls workflow. MCP bounds enterprise capability access. LlamaIndex retrieves evidence. Policy-as-code evaluates discount authority. Guardrails enforce decision. Audit Trace proves decision path.

## Boundaries

Phase 2A does not implement final order creation, payment, shipment, or fulfillment. It does not imply production readiness or a full policy management platform. Production usage would require policy ownership, approval workflow, versioning, rollout controls, rollback controls, and regression validation.

## Next Priority: Agentforce Implementation Mapping

Phase 2B will map the Field Sales Agent architecture into an Agentforce implementation model. The goal is to show how the same enterprise AI control patterns — workflow orchestration, bounded actions, data grounding, policy guardrails, audit trace, cost telemetry, monitoring, and evaluation — can accelerate Salesforce and Agentforce implementation planning while keeping the architecture tool-agnostic.

Planned outputs include an Agentforce mapping diagram, node-to-Agentforce concept mapping, action/tool boundary mapping, Flow/Apex/MuleSoft integration boundary mapping, policy guardrail placement, auditability and evaluation checklist, implementation planning checklist, and demo talk track for Salesforce / Agentforce audiences.

Agentforce mapping is planned as an architecture and implementation planning artifact. It does not imply a live Salesforce integration in the current demo.
