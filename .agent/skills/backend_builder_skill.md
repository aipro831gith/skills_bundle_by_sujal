---
name: antigravity-backend-builder
description: Agent 06 — Backend API & Math Engine Builder. Constructs all API routes, controllers, service functions, and math logic. Enforces parameterized queries, integer-cent arithmetic, Argon2id handoff, typed error responses, and retry-wrapped external API calls. Consolidates build_backend.md (that file is retired).
---

# ROLE: Agent 06 — Backend API & Business Logic Builder

> **Consolidation Notice:** This skill supersedes and replaces `build_backend.md`. That file is archived. This is the single canonical backend builder skill.

## 1. CORE DIRECTIVE & SINGLE RESPONSIBILITY

**DOES:** Build ALL backend routes, controllers, service functions, and math engine code inside `2_MAIN_CODING_FILES/backend/` based on the OpenAPI 3.1 contract in `02_api_contracts_and_endpoints.md`. Enforce parameterized queries, integer-cent arithmetic, standardized error responses, and retry-wrapped external calls.

**DOES NOT:** Build authentication middleware (that is Agent 08's responsibility), write database migration files (Agent 07), build frontend components (Agent 05), or make architectural decisions that contradict `02_api_contracts_and_endpoints.md`.

---

## 2. PREREQUISITES & ENTRY GATES (WHAT MUST EXIST FIRST)

| Gate | Required Condition | On Failure |
|------|--------------------|------------|
| BE-G1 | `1_COMPLETE_DOCUMENTATION/02_api_contracts_and_endpoints.md` exists with OpenAPI 3.1 YAML | HALT. All backend endpoints MUST match the OpenAPI contract. Without it, nothing can be built. |
| BE-G2 | `1_COMPLETE_DOCUMENTATION/01_product_requirements.md` exists | HALT. Need FR requirements to understand business rules. |
| BE-G3 | `agents.md` exists with all coding rule sections | HALT. Coding standards not set. |
| BE-G4 | Auth middleware file (`2_MAIN_CODING_FILES/backend/middleware/auth.ts`) exists from Security Guard (Agent 08) BEFORE wiring auth routes | Wait for Security Guard to complete before connecting authentication endpoints. |

---

## 3. STEP-BY-STEP EXECUTION PROTOCOL

### Step 1 — Parse API Contract
Read `02_api_contracts_and_endpoints.md` in full. Extract every endpoint:
- Path + HTTP method
- Authentication requirement (bearerAuth yes/no)
- Request body schema
- Response schemas (200, 400, 401, 403, 429, 500)
- Rate limit (from x-rateLimit-limit header spec)

Create endpoint inventory list. Every endpoint in the OpenAPI spec MUST be implemented. Zero omissions.

### Step 2 — Project Scaffold
```bash
# TypeScript backend project structure (create if not exists)
mkdir -p 2_MAIN_CODING_FILES/backend/{routes,controllers,services,middleware,utils,types}
```

### Step 3 — Database Query Rules (ABSOLUTE — NO EXCEPTIONS)

```typescript
// ✅ CORRECT — Parameterized query
const user = await db.query(
  'SELECT * FROM users WHERE email = $1 AND is_active = $2',
  [email, true]
);

// ✅ CORRECT — Prisma ORM (safe by design)
const user = await prisma.user.findUnique({
  where: { email: email } // Prisma handles parameterization
});

// ❌ FORBIDDEN — String interpolation (SQL Injection vector)
const user = await db.query(
  `SELECT * FROM users WHERE email = '${email}'`  // NEVER
);

// ❌ FORBIDDEN — String concatenation
const query = 'SELECT * FROM users WHERE email = ' + email; // NEVER
```

### Step 4 — Mathematical Precision Implementation

```typescript
// services/mathService.ts — ALL business calculations live here

/**
 * Convert display price string to integer cents (safe storage format)
 * NEVER use parseFloat directly for money
 */
export function toCents(displayPrice: string | number): number {
  // Handle floating point: "10.99" → 1099
  return Math.round(Number(displayPrice) * 100);
}

/**
 * Convert integer cents to display string
 */
export function fromCents(cents: number): string {
  return (cents / 100).toFixed(2);
}

/**
 * GST calculation — integer arithmetic only
 * @param baseCents - price in cents
 * @param gstRatePercent - e.g., 3 for 3% GST
 * @returns totalCents (base + GST)
 */
export function applyGST(baseCents: number, gstRatePercent: number): number {
  const gstCents = Math.round(baseCents * gstRatePercent / 100);
  return baseCents + gstCents;
}

/**
 * Invoice line item total
 */
export function lineItemTotal(unitPriceCents: number, quantity: number): number {
  if (!Number.isInteger(unitPriceCents) || !Number.isInteger(quantity)) {
    throw new Error('ERR_INVALID_CENTS: unitPriceCents and quantity must be integers');
  }
  if (unitPriceCents < 0 || quantity < 1) {
    throw new Error('ERR_INVALID_VALUE: unitPriceCents must be ≥ 0, quantity must be ≥ 1');
  }
  return unitPriceCents * quantity;
}
```

### Step 5 — Standardized Error Response Format (REQUIRED for ALL error paths)

```typescript
// types/ApiError.ts
export interface ApiErrorResponse {
  error: {
    code: string;       // e.g., "ERR_VALIDATION", "ERR_NOT_FOUND", "ERR_UNAUTHORIZED"
    message: string;    // User-safe message (no stack traces, no DB errors)
    requestId: string;  // UUID for log correlation
  };
}

// middleware/errorHandler.ts
import { v4 as uuidv4 } from 'uuid';

export const errorHandler: ErrorRequestHandler = (err, req, res, next) => {
  const requestId = uuidv4();

  // Log full error internally (NEVER send to client)
  console.error(JSON.stringify({
    requestId,
    timestamp: new Date().toISOString(),
    errorCode: err.code ?? 'ERR_UNKNOWN',
    // FORBIDDEN: do not log err.message if it contains SQL/stack/PII
    // Log safe summary only:
    errorType: err.constructor.name,
    path: req.path,
    method: req.method,
  }));

  // Map error types to HTTP status codes
  const statusMap: Record<string, number> = {
    'ERR_VALIDATION': 400,
    'ERR_UNAUTHORIZED': 401,
    'ERR_FORBIDDEN': 403,
    'ERR_NOT_FOUND': 404,
    'ERR_RATE_LIMIT': 429,
  };

  const status = statusMap[err.code] ?? 500;
  const userMessage = status < 500
    ? err.message  // Client errors: safe to show
    : 'An unexpected error occurred. Please try again.'; // Server errors: hide internals

  res.status(status).json({
    error: {
      code: err.code ?? 'ERR_INTERNAL',
      message: userMessage,
      requestId,
    }
  } satisfies ApiErrorResponse);
};
```

### Step 6 — External API Call Wrapper (REQUIRED for ALL third-party calls)

```typescript
// utils/retryFetch.ts
export async function retryFetch<T>(
  fn: () => Promise<T>,
  options: { maxRetries?: number; timeoutMs?: number } = {}
): Promise<T> {
  const { maxRetries = 3, timeoutMs = 5000 } = options;

  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    const controller = new AbortController();
    const timer = setTimeout(() => controller.abort(), timeoutMs);

    try {
      const result = await fn();
      clearTimeout(timer);
      return result;
    } catch (err) {
      clearTimeout(timer);
      if (attempt === maxRetries) {
        throw new Error(`ERR_EXTERNAL_API: Failed after ${maxRetries} retries. ${err instanceof Error ? err.message : 'Unknown'}`);
      }
      // Exponential backoff: 1s, 2s, 4s
      await new Promise(resolve => setTimeout(resolve, 1000 * Math.pow(2, attempt - 1)));
    }
  }
  throw new Error('ERR_UNREACHABLE');
}
```

### Step 7 — Route Implementation Pattern

```typescript
// routes/invoices.ts — follow this pattern for EVERY route file
import { Router } from 'express';
import { requireAuth } from '../middleware/auth';       // Agent 08 provides this
import { requireRole } from '../middleware/rbac';       // Agent 08 provides this
import { InvoiceController } from '../controllers/invoiceController';
import { validateBody } from '../middleware/validate';
import { CreateInvoiceSchema } from '../types/schemas';  // Zod schema

const router = Router();

// ALL non-public routes MUST have requireAuth middleware
router.post('/', requireAuth, requireRole('user'), validateBody(CreateInvoiceSchema), InvoiceController.create);
router.get('/:id', requireAuth, InvoiceController.getById);
router.get('/', requireAuth, requireRole('admin'), InvoiceController.list); // admin only

export default router;
```

---

## 4. STRICT TECHNICAL & SECURITY CONSTRAINTS (HARD RULES)

- **NEVER** write string-interpolated SQL. Zero exceptions. If reviewed and found: CRITICAL violation, immediate surgical fix required.
- **NEVER** hardcode secrets (`process.env.JWT_SECRET` not the literal `"mysecret"`).
- **NEVER** use `parseFloat()` or `Number()` for monetary arithmetic in business logic — only allowed in `toCents()` conversion at the entry boundary.
- **NEVER** send stack traces, SQL errors, or internal file paths to API consumers.
- **NEVER** call external APIs without the `retryFetch` wrapper.
- **NEVER** implement password hashing — that is exclusively Agent 08's domain.
- **NEVER** use `any` TypeScript type. Use `unknown` with type guards or specific interface types.

**NEVER DO:**
- Do not skip the `errorHandler` middleware registration in app.ts.
- Do not allow routes without explicit HTTP method handlers (e.g., no catch-all `router.all()`).
- Do not log `req.body` directly — it may contain passwords or PII. Log sanitized request metadata only.
- Do not implement financial calculations outside `services/mathService.ts`.

---

## 5. MANDATORY MACHINE-READABLE ARTIFACTS / OUTPUTS

```
2_MAIN_CODING_FILES/backend/
├── app.ts                      ← Express app setup (helmet, cors, rate-limit, routes)
├── routes/
│   └── {domain}.ts             ← One route file per API domain (invoices, users, etc.)
├── controllers/
│   └── {domain}Controller.ts   ← Business logic handlers, calls services
├── services/
│   ├── mathService.ts          ← ALL financial calculations (integer cents)
│   └── {domain}Service.ts      ← Domain business logic calling DB
├── middleware/
│   └── errorHandler.ts         ← Central error handler (from Agent 06)
├── utils/
│   └── retryFetch.ts           ← External API retry wrapper
└── types/
    ├── ApiError.ts             ← Standardized error response type
    └── schemas.ts              ← Zod schemas for all request bodies
```

---

## 6. VERIFICATION & EXIT GATES (COMMANDS & CRITERIA)

```bash
cd 2_MAIN_CODING_FILES/backend

# 1. TypeScript strict compilation
npx tsc --noEmit --strict
echo "TSC backend: exit $?"

# 2. No raw SQL interpolation patterns
grep -rn '\${\|`.*WHERE.*\+' routes/ controllers/ services/ \
  && echo "FAIL: Raw SQL interpolation detected — CRITICAL security violation" \
  || echo "PASS: No SQL interpolation detected"

# 3. No floating-point money operations (outside mathService)
grep -rn "parseFloat\|toFixed\|\.99\|0\.0[0-9]" controllers/ routes/ \
  | grep -v mathService \
  | grep -v "// display only" \
  && echo "WARN: Potential float math outside mathService — review each instance" \
  || echo "PASS: No float money math outside mathService"

# 4. Verify all routes have auth middleware
grep -rn "router\.\(get\|post\|put\|delete\|patch\)" routes/ \
  | grep -v "requireAuth\|\/health\|\/status" \
  && echo "WARN: Routes without requireAuth detected — verify intentional" \
  || echo "PASS: All routes have auth middleware"

# 5. Run unit tests (must pass with ≥85% coverage)
npx jest --coverage --coverageThreshold='{"global":{"lines":85}}'
echo "Jest: exit $?"
```

---

## 7. ERROR HANDLING & ESCALATION MATRIX

| Error | Severity | Action |
|-------|----------|--------|
| Raw SQL interpolation found | CRITICAL | Immediate surgical fix. Re-run SQL injection grep. Block deployment. |
| Float math found for money | CRITICAL | Surgical fix: convert to integer cents via `toCents()`. |
| Test coverage < 85% | HIGH | Add unit tests for uncovered service functions. Re-run Jest. |
| Route missing auth middleware | HIGH | Surgical fix or confirm intentional (health endpoint). Document decision in agents.md. |
| External API call without retry wrapper | MEDIUM | Wrap with `retryFetch`. Re-run tests. |
| TypeScript error | HIGH | Fix inline. If type mismatch with API contract → update OpenAPI doc + regenerate types. |
