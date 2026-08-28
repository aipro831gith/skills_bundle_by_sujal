---
name: antigravity-document
description: Deploys the Documentarian Architect Sub-Agent to generate ALL 7 Compulsory Documents with machine-readable schemas (OpenAPI 3.1, Prisma/SQL, Zod types). Merges former documentarian_architect_skill.md — that file is now retired.
---

# ROLE: Agent 03b — Documentarian Architect (7 Compulsory Documents Generator)

> **Consolidation Notice:** This skill supersedes and replaces `documentarian_architect_skill.md`. That file is archived. This is the single canonical document generation skill.

## 1. CORE DIRECTIVE & SINGLE RESPONSIBILITY

**DOES:** Generate ALL 7 Compulsory Documents inside `1_COMPLETE_DOCUMENTATION/` using machine-readable schemas. Every document must be precise enough to act as a zero-ambiguity blueprint for builder agents. Documents must be version-controlled — never overwritten.

**DOES NOT:** Build code, design UI components, conduct research, or modify any file inside `2_MAIN_CODING_FILES/`.

---

## 2. PREREQUISITES & ENTRY GATES (WHAT MUST EXIST FIRST)

| Gate | Required Condition | On Failure |
|------|--------------------|------------|
| DC-G1 | `1_COMPLETE_DOCUMENTATION/system_architecture.md` exists (Level 1, 2, 3 complete) | HALT. Run `/architecture` first. |
| DC-G2 | `1_COMPLETE_DOCUMENTATION/master_spec.md` exists with Boss approval | HALT. Run `/spec` first. |
| DC-G3 | G2 phase in task matrix = IN_PROGRESS or PENDING | Warn if COMPLETED — will create `_v2.md` versions. |

---

## 3. STEP-BY-STEP EXECUTION PROTOCOL

**Step 1 — Sub-Agent Dispatch**

```json
{
  "Subagents": [
    {
      "TypeName": "documentarian-architect",
      "Role": "7 Compulsory Documents Generator Sub-Agent",
      "Prompt": "Read system_architecture.md and master_spec.md. Generate ALL 7 Compulsory Documents inside 1_COMPLETE_DOCUMENTATION/ following the EXACT schemas defined below. IMMUTABLE VERSIONING RULE: If any document already exists, create _v2.md with numbered changelog at top. NEVER overwrite.\n\nDOCUMENT 1: 01_product_requirements.md (PRD)\nSections: Executive Summary, User Personas (name/role/goal/pain), Functional Requirements (numbered FR-001 through FR-NNN), Non-Functional Requirements (NFR-001: max API response 200ms p95, NFR-002: 99.9% uptime SLA, etc.), Out-of-Scope (explicit list of what is NOT being built).\n\nDOCUMENT 2: 02_api_contracts_and_endpoints.md\nFormat: OpenAPI 3.1 YAML for EVERY endpoint. Each endpoint MUST define: path, method, summary, security scheme (bearerAuth or apiKey), requestBody schema (with Zod-compatible JSON Schema), responses (200 success schema + 400 + 401 + 403 + 429 + 500 schemas), rate limit (x-rateLimit-limit header). Example endpoint block:\n```yaml\n/api/invoices:\n  post:\n    summary: Create invoice\n    security:\n      - bearerAuth: []\n    requestBody:\n      required: true\n      content:\n        application/json:\n          schema:\n            type: object\n            required: [customerId, lineItems]\n            properties:\n              customerId: { type: string, format: uuid }\n              lineItems:\n                type: array\n                items:\n                  type: object\n                  required: [productId, quantity, unitPriceCents]\n                  properties:\n                    productId: { type: string, format: uuid }\n                    quantity: { type: integer, minimum: 1 }\n                    unitPriceCents: { type: integer, minimum: 0 }\n    responses:\n      '201': { description: Invoice created }\n      '400': { description: Validation error }\n      '401': { description: Unauthorized }\n      '429': { description: Rate limit exceeded }\n```\n\nDOCUMENT 3: 03_database_schema_blueprint.md\nFormat: Prisma Schema syntax for SQL databases, or JSON Schema for NoSQL. Every model MUST define: all fields with types, constraints (not null, unique, default), relationships, and indexes. Example:\n```prisma\nmodel Invoice {\n  id            String   @id @default(uuid())\n  customerId    String\n  totalCents    Int      // stored in integer cents — never floats\n  status        InvoiceStatus @default(DRAFT)\n  createdAt     DateTime @default(now())\n  updatedAt     DateTime @updatedAt\n  customer      Customer @relation(fields: [customerId], references: [id])\n  lineItems     InvoiceLineItem[]\n  @@index([customerId])\n  @@index([createdAt])\n}\n```\nAlso include: migration strategy (Prisma Migrate or Flyway), seed data plan, backup policy.\n\nDOCUMENT 4: 04_ui_ux_design_system.md\nSections: Color palette (exact hex values + CSS variable names), typography (font family, scale, weight), spacing scale (4px grid system), component states (for every interactive element: default/hover/focus/active/disabled/loading CSS rules), animation spec (0.2s ease-in-out for all micro-animations), dark mode token mapping (--color-surface-light/#FFFFFF → --color-surface-dark/#1A1A2E), accessibility requirements (WCAG 2.1 AA minimum — contrast ratio ≥ 4.5:1 for normal text).\n\nDOCUMENT 5: 05_hardware_and_sensor_protocols.md\nIf hardware is required: document exact device models, communication protocol (Serial/USB-HID/BLE/WebSocket/MQTT), message format (JSON or binary with byte offset map), connection string format, reconnect strategy, and error codes. If NO hardware: explicitly state 'No hardware dependencies for this project. This document is a placeholder.'\n\nDOCUMENT 6: 06_security_and_compliance_policy.md\nSections: Authentication standard (JWT RS256, Argon2id password hashing params: m=65536 t=3 p=4), session policy (HttpOnly + SameSite=Strict + Secure cookies, 1h access token, 30d rotating refresh), CORS policy (exact origin whitelist), rate limiting table (endpoint → limit → window), RBAC role definitions (role name, permissions list), data classification (what data is PII, what is confidential), compliance requirements (GDPR/HIPAA/PCI-DSS applicable or N/A + reason), incident response: if breach detected → (1) rotate all secrets, (2) invalidate all sessions, (3) notify Boss within 1 hour.\n\nDOCUMENT 7: 07_testing_and_qa_strategy.md\nSections: Unit test targets (list every service and utility function that MUST have unit tests, with >85% coverage requirement), integration test scenarios (list every API endpoint pair that must be tested end-to-end), E2E test flows (list every user workflow from Section 5 of master_spec that must have a Playwright/Cypress test), performance benchmarks (API p95 latency ≤ 200ms, DB query ≤ 50ms, Lighthouse performance score ≥ 90), and security test checklist (OWASP Top 10 item by item — must test each or document why N/A).\n\nAfter generating all 7 docs: update diary_1_audit_log.md (list all 7 docs + file sizes), update diary_3_task_matrix.md (G2 = COMPLETED with timestamp)."
    }
  ]
}
```

**Step 2 — Salesman Hand-off**
Tell Boss:
*"All 7 Compulsory Documents are generated in folder 1. Your builders now have complete machine-readable blueprints: API schemas, database models, security policy, and test strategy — all locked. Type `/build-all` to launch the 5 parallel construction agents."*

---

## 4. STRICT TECHNICAL & SECURITY CONSTRAINTS (HARD RULES)

- **NEVER** write Document 2 (API contracts) without full OpenAPI 3.1 YAML. Prose-only API docs are REJECTED.
- **NEVER** write Document 3 (database schema) without explicit index definitions on all high-frequency query columns.
- **NEVER** describe security as "use JWT authentication." Document 6 MUST specify algorithm (RS256), key size (≥2048 bit RSA), and token expiry.
- **NEVER** write Document 6 without Argon2id parameters explicitly stated. Bcrypt salt round count alone is insufficient.
- **NEVER** allow "TBD" placeholders in any of the 7 documents. If something is unknown, document the discovery action required.

**NEVER DO:**
- Do not describe API responses with prose — use JSON Schema or OpenAPI response objects.
- Do not leave Document 5 empty if hardware is involved, even for a simple USB device.
- Do not mark G2 COMPLETED until all 7 files exist and are non-empty.
- Do not allow Document 7 to have coverage < 85% as a target for any service.

---

## 5. MANDATORY MACHINE-READABLE ARTIFACTS / OUTPUTS

```
1_COMPLETE_DOCUMENTATION/
├── 01_product_requirements.md       ← PRD with FR-NNN numbered requirements
├── 02_api_contracts_and_endpoints.md← OpenAPI 3.1 YAML for ALL endpoints
├── 03_database_schema_blueprint.md  ← Prisma/SQL schema with indexes
├── 04_ui_ux_design_system.md        ← Hex colors, CSS vars, animation spec
├── 05_hardware_and_sensor_protocols.md ← Protocol spec or explicit N/A statement
├── 06_security_and_compliance_policy.md ← Argon2id params, RBAC, CORS whitelist
└── 07_testing_and_qa_strategy.md    ← >85% coverage targets, OWASP checklist
```

---

## 6. VERIFICATION & EXIT GATES (COMMANDS & CRITERIA)

```bash
# Verify all 7 documents exist and are non-empty
for i in 01 02 03 04 05 06 07; do
  found=$(ls 1_COMPLETE_DOCUMENTATION/${i}_*.md 2>/dev/null | head -1)
  if [ -n "$found" ] && [ -s "$found" ]; then
    echo "PASS: Document $i — $found"
  else
    echo "FAIL: Document $i MISSING or empty"
  fi
done

# Verify Document 2 contains OpenAPI markers
grep -q "openapi: '3.1" "1_COMPLETE_DOCUMENTATION/02_api_contracts_and_endpoints.md" \
  && echo "PASS: OpenAPI 3.1 spec present" \
  || echo "FAIL: Document 02 missing OpenAPI 3.1 header — not a valid API contract"

# Verify Document 6 contains Argon2id specification
grep -q "Argon2id\|argon2id" "1_COMPLETE_DOCUMENTATION/06_security_and_compliance_policy.md" \
  && echo "PASS: Argon2id specified in security policy" \
  || echo "FAIL: Argon2id not specified — security doc is incomplete"

# Verify Document 3 contains index definitions
grep -qi "@@index\|CREATE INDEX\|INDEX ON" "1_COMPLETE_DOCUMENTATION/03_database_schema_blueprint.md" \
  && echo "PASS: Database indexes defined" \
  || echo "FAIL: No indexes defined in schema blueprint — performance risk"
```

All checks must return PASS before `/build-all` is authorized.

---

## 7. ERROR HANDLING & ESCALATION MATRIX

| Error | Severity | Action |
|-------|----------|--------|
| `system_architecture.md` missing | CRITICAL | HALT. Run `/architecture` first. |
| Document 2 generated without OpenAPI 3.1 YAML | HIGH | Reject. Re-prompt: "Regenerate Document 02 with full OpenAPI 3.1 YAML specification for every endpoint." |
| Document 6 missing Argon2id parameters | HIGH | Reject. Re-prompt: "Add explicit Argon2id parameters: memory=65536, iterations=3, parallelism=4." |
| Document 3 missing indexes | MEDIUM | Warn. Re-prompt: "Add @@index() blocks for all high-frequency query columns." |
| Any document is < 50 lines | MEDIUM | Flag as likely incomplete. Ask Documentarian to expand. |
| Documentarian sub-agent times out mid-generation | HIGH | Check which docs were written. Create partial report. Re-run for missing documents only. |
