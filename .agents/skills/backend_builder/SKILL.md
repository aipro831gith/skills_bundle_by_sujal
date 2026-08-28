---
name: antigravity-backend-builder
description: Backend API Builder (Group 3, Agent 06). Builds Express/Node.js backend with strict TypeScript, ACID-SERIALIZABLE transactions, Zod input validation on all routes, centralized error middleware, connection pool throttling, and typed AppError responses. Consumes arch_handoff.json.
---

# ROLE: Agent 06 — Backend API Builder (Zero-Trust, Strict-Typed)

## 1. CORE DIRECTIVE & SINGLE RESPONSIBILITY

Build ALL backend source files in `2_MAIN_CODING_FILES/backend/`. Every route validates input via Zod before touching DB. Every financial DB write uses SERIALIZABLE isolation. Every response uses the standard `ApiResponse<T>` envelope. Connection pool is a singleton with throttling.

**DOES NOT:** Build frontend, write Prisma schema, handle deployment config.

---

## 2. PREREQUISITES & ENTRY GATES

| Gate | Condition | Failure Action |
|------|-----------|---------------|
| BB-G1 | `.gate/arch_handoff.json` exists | HALT: architecture not complete |
| BB-G2 | `1_COMPLETE_DOCUMENTATION/03_boundary_schemas.ts` exists | HALT: boundary contracts missing |
| BB-G3 | `2_MAIN_CODING_FILES/database/schema.prisma` exists (from Agent 07) | Wait 5 min, retry × 3, then flag |

---

## 3. MANDATORY CODE PATTERNS (use verbatim)

### 3.1 — AppError Type & Response Envelope

```typescript
// backend/types/errors.ts
export type ErrorCode =
  | 'ERR_VALIDATION'
  | 'ERR_NOT_FOUND'
  | 'ERR_UNAUTHORIZED'
  | 'ERR_FORBIDDEN'
  | 'ERR_CONFLICT'
  | 'ERR_INTERNAL'
  | 'ERR_DB_TIMEOUT'
  | 'ERR_INVALID_PRICE'
  | 'ERR_EMPTY_INVOICE'
  | 'ERR_CLIENT_NOT_FOUND';

export class AppError extends Error {
  constructor(
    public readonly code: ErrorCode,
    public readonly message: string,
    public readonly statusCode: number,
    public readonly field?: string,
  ) {
    super(message);
    this.name = 'AppError';
  }
}

export type ApiResponse<T> = {
  data: T | null;
  error: { code: ErrorCode; message: string; field?: string } | null;
};

export function ok<T>(data: T): ApiResponse<T> {
  return { data, error: null };
}

export function fail<T>(error: AppError): ApiResponse<T> {
  return {
    data: null,
    error: { code: error.code, message: error.message, field: error.field },
  };
}
```

### 3.2 — Integer-Cent Math Service (mathService.ts)

```typescript
// backend/services/mathService.ts

/** Convert display price string to integer cents. Throws on invalid input. */
export function toCents(display: string): number {
  const match = display.match(/^(\d+)(?:\.(\d{1,2}))?$/);
  if (!match) throw new AppError('ERR_INVALID_PRICE', 'Price must have at most 2 decimal places', 422);
  const dollars = parseInt(match[1], 10);
  const cents = match[2] ? parseInt(match[2].padEnd(2, '0'), 10) : 0;
  return dollars * 100 + cents;
}

/** Convert integer cents to display string. */
export function fromCents(cents: number): string {
  if (!Number.isInteger(cents) || cents < 0)
    throw new AppError('ERR_INTERNAL', 'Invalid cents value', 500);
  return `${Math.floor(cents / 100)}.${String(cents % 100).padStart(2, '0')}`;
}

/** Calculate line item total in cents. */
export function lineItemTotal(qty: number, unitPriceCents: number): number {
  if (!Number.isInteger(qty) || qty <= 0) throw new AppError('ERR_VALIDATION', 'Quantity must be a positive integer', 422);
  if (!Number.isInteger(unitPriceCents) || unitPriceCents <= 0) throw new AppError('ERR_VALIDATION', 'Price must be a positive integer (cents)', 422);
  return qty * unitPriceCents;
}

/** Apply GST. Returns new total including GST, in cents. */
export function applyGST(subtotalCents: number, gstRatePercent: number): number {
  if (!Number.isInteger(subtotalCents) || subtotalCents < 0) throw new AppError('ERR_INTERNAL', 'Invalid subtotal cents', 500);
  const gstCents = Math.round(subtotalCents * gstRatePercent / 100);
  return subtotalCents + gstCents;
}

/** Sum invoice line items and apply GST. Returns total in cents. */
export function calculateInvoiceTotal(
  lineItems: { qty: number; unitPriceCents: number }[],
  gstRatePercent: number,
): number {
  if (lineItems.length === 0) throw new AppError('ERR_EMPTY_INVOICE', 'Invoice must have at least one line item', 422);
  const subtotal = lineItems.reduce((sum, item) => sum + lineItemTotal(item.qty, item.unitPriceCents), 0);
  return applyGST(subtotal, gstRatePercent);
}
```

### 3.3 — ACID-SERIALIZABLE Financial Transaction Pattern

```typescript
// REQUIRED pattern for ALL financial writes (create invoice, record payment, etc.)
import { prisma } from '../db/prismaClient';

async function createInvoiceWithItems(
  data: CreateInvoiceInput,
): Promise<Invoice> {
  return prisma.$transaction(
    async (tx) => {
      // Verify client exists inside transaction (reads are repeatable under SERIALIZABLE)
      const client = await tx.client.findUnique({ where: { id: data.clientId } });
      if (!client) throw new AppError('ERR_CLIENT_NOT_FOUND', 'Client not found', 404);

      const totalCents = calculateInvoiceTotal(data.lineItems, data.gstRatePercent);

      const invoice = await tx.invoice.create({
        data: {
          clientId: data.clientId,
          totalCents,
          gstRatePercent: data.gstRatePercent,
          status: 'DRAFT',
          lineItems: {
            createMany: {
              data: data.lineItems.map((item) => ({
                description: item.description,
                quantity: item.quantity,
                unitPriceCents: item.unitPriceCents,
                totalCents: lineItemTotal(item.quantity, item.unitPriceCents),
              })),
            },
          },
        },
        include: { lineItems: true },
      });

      return invoice;
    },
    { isolationLevel: 'Serializable' },  // MANDATORY for financial writes
  );
}
```

### 3.4 — Centralized Error Middleware (Required in app.ts)

```typescript
// backend/middleware/errorMiddleware.ts
import { Request, Response, NextFunction, ErrorRequestHandler } from 'express';
import { AppError, fail } from '../types/errors';
import { logger } from '../utils/logger';
import { ZodError } from 'zod';

export const errorMiddleware: ErrorRequestHandler = (
  err: unknown,
  req: Request,
  res: Response,
  _next: NextFunction,
) => {
  // Structured internal logging (never exposed to client)
  logger.error({
    requestId: req.headers['x-request-id'],
    path: req.path,
    method: req.method,
    error: err instanceof Error ? err.message : String(err),
    stack: process.env.NODE_ENV === 'development' ? (err instanceof Error ? err.stack : undefined) : undefined,
  });

  if (err instanceof AppError) {
    return res.status(err.statusCode).json(fail(err));
  }

  if (err instanceof ZodError) {
    const firstIssue = err.issues[0];
    return res.status(422).json(
      fail(new AppError('ERR_VALIDATION', firstIssue.message, 422, firstIssue.path.join('.'))),
    );
  }

  // PrismaClientKnownRequestError: P2002 = unique constraint
  if ((err as { code?: string }).code === 'P2002') {
    return res.status(409).json(
      fail(new AppError('ERR_CONFLICT', 'A record with this value already exists', 409)),
    );
  }

  // Connection timeout
  if ((err as { code?: string }).code === 'P1001') {
    return res.status(503)
      .set('Retry-After', '30')
      .json(fail(new AppError('ERR_DB_TIMEOUT', 'Database temporarily unavailable', 503)));
  }

  // Fallback — never expose details in production
  return res.status(500).json(
    fail(new AppError('ERR_INTERNAL', 'An unexpected error occurred', 500)),
  );
};
```

### 3.5 — Route Handler Pattern (Zod-First)

```typescript
// backend/routes/invoiceRoutes.ts
import { Router } from 'express';
import { CreateInvoiceSchema } from '../../1_COMPLETE_DOCUMENTATION/03_boundary_schemas';
import { requireAuth } from '../middleware/authMiddleware';
import { createInvoiceWithItems } from '../services/invoiceService';
import { ok } from '../types/errors';

const router = Router();

router.post('/', requireAuth, async (req, res, next) => {
  try {
    // Step 1: Validate BEFORE touching database
    const parsed = CreateInvoiceSchema.safeParse(req.body);
    if (!parsed.success) return next(parsed.error);  // Routes to Zod branch in errorMiddleware

    // Step 2: Business logic
    const invoice = await createInvoiceWithItems({
      ...parsed.data,
      userId: req.user.id,  // Injected by requireAuth middleware
    });

    // Step 3: Standard response envelope
    return res.status(201).json(ok(invoice));
  } catch (err) {
    next(err);  // All errors route to centralized errorMiddleware
  }
});

export { router as invoiceRouter };
```

### 3.6 — retryFetch for External API Calls

```typescript
// backend/utils/retryFetch.ts

interface RetryOptions {
  retries?: number;      // default: 3
  backoffMs?: number;    // default: 500 (doubles each retry)
  timeout?: number;      // default: 8000ms
}

export async function retryFetch(
  url: string,
  options: RequestInit & RetryOptions = {},
): Promise<Response> {
  const { retries = 3, backoffMs = 500, timeout = 8000, ...fetchOptions } = options;

  for (let attempt = 0; attempt <= retries; attempt++) {
    const controller = new AbortController();
    const timeoutId = setTimeout(() => controller.abort(), timeout);

    try {
      const response = await fetch(url, { ...fetchOptions, signal: controller.signal });
      clearTimeout(timeoutId);
      if (response.ok || attempt === retries) return response;
      throw new Error(`HTTP ${response.status}`);
    } catch (err) {
      clearTimeout(timeoutId);
      if (attempt === retries) throw err;
      await new Promise((r) => setTimeout(r, backoffMs * Math.pow(2, attempt)));
    }
  }
  throw new Error('ERR_INTERNAL: retryFetch exceeded max retries');
}
```

---

## 4. OUTPUT SCHEMA — HANDOFF TO QA AGENT

Write `.gate/backend_handoff.json`:

```json
{
  "handoff_id": "BB-{ISO8601}",
  "from_agent": "backend_builder_agent_06",
  "to_agent": "qa_agent_10",
  "status": "COMPLETED",
  "files_created": ["backend/routes/*.ts", "backend/services/*.ts", "backend/middleware/*.ts"],
  "api_endpoints": [
    { "method": "POST", "path": "/api/invoices", "auth": true, "validation": "CreateInvoiceSchema" }
  ],
  "invariants_verified": {
    "no_float_money": true,
    "serializable_transactions": true,
    "zod_validation_all_routes": true,
    "centralized_error_middleware": true,
    "connection_pool_singleton": true
  }
}
```

---

## 5. STRICT CONSTRAINTS

- **NEVER** use `parseFloat`, `toFixed`, or floating-point math on monetary values.
- **NEVER** write a route handler without Zod input validation as the first operation.
- **NEVER** expose stack traces to API clients in `NODE_ENV === 'production'`.
- **NEVER** create a new PrismaClient per request — singleton only.
- **NEVER** write financial DB operations outside a `prisma.$transaction({ isolationLevel: 'Serializable' })`.
- **NEVER** use string-interpolated SQL — Prisma ORM or `$queryRaw` tagged template only.
- **NEVER** use `eval()` anywhere in backend code.

---

## 6. VERIFICATION & EXIT GATES

```bash
# B1: TypeScript strict compilation
cd 2_MAIN_CODING_FILES && npx tsc --noEmit --strict && echo "TSC:PASS"

# B2: No float money patterns
grep -rn "parseFloat\|toFixed\|Math\.floor.*[Pp]rice\|0\.[0-9][0-9].*[Cc]ents" \
  2_MAIN_CODING_FILES/backend/ \
  | grep -v "test\|spec\|mathService" \
  && echo "FAIL: float money found" || echo "B2:PASS"

# B3: No eval()
grep -rn '\beval(' 2_MAIN_CODING_FILES/backend/ | grep -v "//\|test" \
  && echo "FAIL: eval() found" || echo "B3:PASS"

# B4: All routes import Zod schema
for f in 2_MAIN_CODING_FILES/backend/routes/*.ts; do
  grep -q "Schema\|zod\|safeParse\|parse(" "$f" \
    && echo "B4-PASS: $f has validation" \
    || echo "B4-FAIL: $f has NO Zod validation"
done

# B5: Jest coverage ≥85%
cd 2_MAIN_CODING_FILES && npx jest --coverage \
  --coverageThreshold='{"global":{"lines":85,"branches":85}}' \
  --passWithNoTests=false && echo "B5:PASS"
```

---

## 7. ESCALATION MATRIX

| Error | Severity | Action |
|-------|----------|--------|
| Route missing Zod validation | CRITICAL | Do not mark task complete. Add validation before any other work continues. |
| Financial write without SERIALIZABLE | CRITICAL | Wrap in `prisma.$transaction({ isolationLevel: 'Serializable' })`. Re-verify. |
| Float money discovered post-build | CRITICAL | Dispatch ast_diff_reconciler for targeted fix. Re-run B2 gate. |
| TSC errors | HIGH | Fix every error. Do not ship with TypeScript errors. |
| Jest < 85% coverage | HIGH | Add targeted tests for uncovered branches. Show coverage report to Team Leader. |
| PrismaClient per-request found | HIGH | Refactor to singleton. Connection pool exhaustion risk. |
