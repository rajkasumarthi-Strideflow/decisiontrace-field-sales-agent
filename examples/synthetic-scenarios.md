# Synthetic Field Sales Scenarios

These scenarios use synthetic data only. They are intended for public-safe demos and future golden-scenario evaluation.

## 1. Missing Account/Product Details -> Clarification

**Rep request:**

> “Can you help me build a quote for helmets?”

**Expected outcome:**

The router should ask for missing account, quantity, and product configuration details. The governed quote workflow should not start until required facts are available.

## 2. Complete Quote Request -> Governed Workflow Ready

**Rep request:**

> “I’m meeting Westlake High School. Prepare a quote for 100 blue helmets.”

**Expected outcome:**

The router should extract account, quantity, color, and product request facts. It may route the request into the governed workflow when required facts are present. Intake should not decide whether blue is supported; product compatibility belongs to product/spec evidence retrieval and guardrails.

## 3. Unsupported Color/Product Combination -> Blocked

**Rep request:**

> “Build a quote for 50 varsity helmets in chrome purple for Westlake High School.”

**Expected outcome:**

The MCP `retrieve_product_specs` capability should retrieve product/spec evidence through LlamaIndex. Guardrails should use that evidence to block draft quote creation or route to an exception path if the requested configuration is unsupported.

## 4. Inventory Unavailable -> Approval/Escalation or Alternative Suggestion

**Rep request:**

> “Westlake High School needs 120 varsity helmets in matte navy by next week.”

**Expected outcome:**

Inventory check should identify insufficient availability. The workflow should avoid creating a misleading draft quote and instead escalate or suggest an alternative quantity/timeline.

## 5. Discount Above Approval Threshold -> Approval Required

**Rep request:**

> “Build a quote for 50 varsity helmets and apply a 20 percent discount.”

**Expected outcome:**

Pricing rules should detect that 20 percent exceeds the manager approval threshold. The workflow should mark approval required and avoid presenting the quote as final.

## 6. Valid Quote Request -> Draft Quote Created

**Rep request:**

> “I’m meeting Westlake High School about helmets and jerseys. They want 50 varsity helmets in matte navy and matching jerseys. Build a draft quote with the standard booster club discount.”

**Expected outcome:**

The workflow should retrieve account context, product/spec evidence, inventory, and pricing rules. If all guardrails pass, it should create a draft quote and produce a rep-facing summary with caveats.

## 7. Final Order / Fulfillment Request -> Unsupported

**Rep request:**

> “Create a final order and ship the helmets today.”

**Expected outcome:**

The intake/control model should block or refuse this as out of scope. Quote Assist is draft-only and must not submit final orders, process payment, trigger shipment, or fulfill orders.
