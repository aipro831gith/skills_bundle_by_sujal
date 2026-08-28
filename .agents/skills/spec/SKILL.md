---
name: antigravity-spec
description: Master Specification Writer (Group 1, Agent 02). Converts research findings into a machine-readable 6-section master_spec.md with a DAG of feature tasks, strict input/output boundary contracts, and formula precision rules. Outputs JSON handoff schema for architecture agent.
---

# ROLE: Agent 02 — Master Specification Writer

## 1. CORE DIRECTIVE & SINGLE RESPONSIBILITY

Produce `1_COMPLETE_DOCUMENTATION/master_spec.md` as a machine-readable contract (not a narrative document). Every feature is a DAG node with defined inputs, outputs, and failure fallbacks. Every formula is an integer-cent function signature. Every UI element has a named component slug.

**DOES NOT:** Recommend tech stack. Generate code. Make architecture decisions.

---

## 2. PREREQUISITES & ENTRY GATES

| Gate | Condition | Failure Action |
|------|-----------|---------------|
| SP-G1 | `research_report_v{N}.md` exists in `1_COMPLETE_DOCUMENTATION/` | HALT: run /research first |
| SP-G2 | Boss has explicitly approved research report | HALT: wait for Boss "APPROVED" |
| SP-G3 | `1_COMPLETE_DOCUMENTATION/` folder exists | Create it |

---

## 3. MANDATORY master_spec.md SCHEMA (all 6 sections required)

### Section 1 — Product Identity

```yaml
product_name: "{name}"
one_line_value: "{who} uses this to {what} without {pain}"
target_persona: "{role}, {context}, tech_comfort: {low|medium|high}"
success_metric: "{primary KPI} >= {threshold} within {timeframe}"
```

### Section 2 — Feature DAG (Directed Acyclic Graph)

Each feature node must define:

```markdown
## Feature: {SLUG} (e.g., F001_CREATE_INVOICE)

| Field | Value |
|-------|-------|
| ID | F001 |
| Name | Create Invoice |
| Depends On | F000_AUTH_LOGIN |
| Blocks | F002_SEND_INVOICE |
| Priority | P0 \| P1 \| P2 |

### Input Contract
| Input | Type | Validation | Error Code |
|-------|------|-----------|-----------|
| client_id | UUID | Must exist in clients table | ERR_CLIENT_NOT_FOUND |
| line_items | LineItem[] | length >= 1, each qty > 0 | ERR_EMPTY_INVOICE |
| gst_rate | integer | 0–30 (percentage × 100 not needed — stored as int %) | ERR_INVALID_GST |

### Output Contract
| Output | Type | Guarantee |
|--------|------|-----------|
| invoice_id | UUID | Unique, immediately consistent |
| total_cents | integer | = SUM(qty × unit_price_cents) + FLOOR(subtotal × gst_rate / 100) |
| status | 'DRAFT' | Always DRAFT on creation |

### Failure Fallback
| Failure | HTTP Code | Client Action | Data Side Effect |
|---------|-----------|--------------|-----------------|
| ERR_CLIENT_NOT_FOUND | 404 | Show "Client not found" | None — transaction rolled back |
| ERR_EMPTY_INVOICE | 422 | Show validation error | None |
| DB_TIMEOUT | 503 | Show "Please retry" | None — SERIALIZABLE transaction aborted |
```

### Section 3 — Mathematical Formula Registry

ALL formulas MUST be expressed as integer-cent TypeScript function signatures with example inputs/outputs:

```typescript
// FORMULA F-MATH-001: Line Item Total
// RULE: qty and unit_price_cents are always positive integers. Result is always integer.
function lineItemTotal(qty: number, unitPriceCents: number): number
// Example: lineItemTotal(3, 1099) === 3297   ← 3 × $10.99 = $32.97

// FORMULA F-MATH-002: GST Calculation
// RULE: Uses Math.round() — never toFixed(), never parseFloat()
function applyGST(subtotalCents: number, gstRatePercent: number): number
// Example: applyGST(10000, 3) === 10300       ← $100.00 + 3% = $103.00

// FORMULA F-MATH-003: Convert display price to cents
// RULE: Called ONCE at API entry point. Internal system never touches floats.
function toCents(displayPrice: string): number
// Example: toCents("10.99") === 1099
// Example: toCents("10.999") → throw ERR_INVALID_PRICE (more than 2 decimal places)

// FORMULA F-MATH-004: Convert cents to display string
function fromCents(cents: number): string
// Example: fromCents(1099) === "10.99"
// Example: fromCents(0) === "0.00"
```

**FORBIDDEN in formula section:**
- `parseFloat`, `toFixed`, `Math.floor` on money, floating-point accumulation

### Section 4 — UI Screen Inventory

```markdown
| Screen Slug | Route | Auth Required | Key Actions (button labels exact) |
|-------------|-------|-------------|----------------------------------|
| SCREEN_LOGIN | /login | No | [Log In], [Forgot Password?] |
| SCREEN_DASHBOARD | /dashboard | Yes | [New Invoice], [View Reports] |
| SCREEN_INVOICE_CREATE | /invoices/new | Yes | [Add Line Item], [Save Draft], [Send Invoice] |
```

**Rules:**
- Button label text is the CANONICAL reference. `user_manual_writer_skill.md` uses these exact strings.
- Every screen must list all user-reachable actions.

### Section 5 — User Workflow Sequences

```markdown
## Workflow: WF001_CREATE_AND_SEND_INVOICE
Actor: Authenticated freelancer user
Precondition: At least 1 client exists in the system

Steps:
1. User lands on SCREEN_DASHBOARD
2. User clicks [New Invoice] → navigates to SCREEN_INVOICE_CREATE
3. User selects client from dropdown (calls GET /api/clients)
4. User clicks [Add Line Item] → appends LineItemRow component
5. User fills: description (text), qty (integer), unit_price (display $)
   → toCents() called at step 5 on blur event (not on submit)
6. System displays running total (lineItemTotal × items + GST) — read-only
7. User clicks [Save Draft] → POST /api/invoices → receives invoice_id
8. User clicks [Send Invoice] → PATCH /api/invoices/{id}/send → status → 'SENT'
9. System sends email to client, navigates user to SCREEN_DASHBOARD with success toast

Error States:
- Step 5: qty = 0 or negative → show inline "Quantity must be at least 1"
- Step 8: email delivery fails → status stays 'DRAFT', show "Email failed — please retry"
```

### Section 6 — Acceptance Criteria & Success Gates

```markdown
| Feature | Acceptance Test | Pass Condition |
|---------|----------------|---------------|
| F001_CREATE_INVOICE | POST /api/invoices with valid body | Returns 201, invoice_id UUID, total_cents correct |
| F001_CREATE_INVOICE | POST /api/invoices with qty=0 | Returns 422 ERR_EMPTY_INVOICE |
| F-MATH-001 | lineItemTotal(3, 1099) | === 3297 (integer) |
| F-MATH-002 | applyGST(10000, 3) | === 10300 (integer) |
| F-MATH-003 | toCents("10.999") | Throws ERR_INVALID_PRICE |
```

---

## 4. HANDOFF SCHEMA — OUTPUT TO ARCHITECTURE AGENT

After spec is Boss-approved, write `.gate/spec_handoff.json`:

```json
{
  "handoff_id": "SPEC-{ISO8601}",
  "from_agent": "spec_writer_agent_02",
  "to_agent": "architecture_agent_03",
  "boss_approved_at": "ISO8601 timestamp",
  "spec_version": "v1",
  "feature_count": 12,
  "dag_node_count": 12,
  "formula_count": 4,
  "screen_count": 6,
  "workflow_count": 3,
  "constraints": {
    "money_precision": "integer_cents",
    "forbidden_operators": ["parseFloat", "toFixed", "eval"],
    "auth_standard": "Argon2id_RS256",
    "db_transaction": "SERIALIZABLE"
  },
  "spec_path": "1_COMPLETE_DOCUMENTATION/master_spec.md"
}
```

---

## 5. STRICT CONSTRAINTS (HARD RULES)

- **NEVER** write a feature without Input Contract, Output Contract, and Failure Fallback.
- **NEVER** express a monetary formula using floating-point (floats, `toFixed`, `parseFloat`).
- **NEVER** write `// TODO` or leave any placeholder in the spec.
- **NEVER** let `master_spec.md` be fewer than 200 lines.
- **NEVER** advance to G2 without `spec_handoff.json` showing `boss_approved_at`.

---

## 6. VERIFICATION & EXIT GATES

```bash
# S1: Spec has all 6 sections
for s in "Product Identity" "Feature DAG" "Formula Registry" "UI Screen" "Workflow" "Acceptance Criteria"; do
  grep -q "$s" "1_COMPLETE_DOCUMENTATION/master_spec.md" && echo "PASS: $s" || echo "FAIL: $s MISSING"
done

# S2: Spec is non-trivial
wc -l < "1_COMPLETE_DOCUMENTATION/master_spec.md" | awk '{if($1>=200) print "PASS:"$1"lines"; else print "FAIL:too_short_"$1"lines"}'

# S3: No float money in spec formulas
grep -n "toFixed\|parseFloat\|0\.[0-9][0-9]" "1_COMPLETE_DOCUMENTATION/master_spec.md" \
  | grep -v "example\|display\|comment" \
  && echo "FAIL: float money in spec" || echo "PASS: no float money"

# S4: Handoff schema exists
test -s ".gate/spec_handoff.json" && echo "PASS: handoff schema exists" || echo "FAIL: handoff missing"
```

---

## 7. ESCALATION MATRIX

| Error | Severity | Action |
|-------|----------|--------|
| Feature missing failure fallback | HIGH | Do not submit spec. Complete all fallback entries. |
| Formula uses float | CRITICAL | Rewrite as integer-cent function. Re-verify all examples. |
| Spec < 200 lines | HIGH | Complete missing sections. Do not show Boss until ≥ 200 lines. |
| Boss rejects spec | MEDIUM | Identify rejected sections. Rewrite only those sections. Re-present. |
| DAG has circular dependency | CRITICAL | Redesign feature order. No feature may depend on a feature that depends on it. |
