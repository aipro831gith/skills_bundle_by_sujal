# GEMINI.md — Project Governance & Execution Rules
# Platform: Google Antigravity 2.0 / Gemini Models

## 1. WORKSPACE ARCHITECTURE RULES
- `1_COMPLETE_DOCUMENTATION/`: Specifications, blueprints, OpenAPI 3.1 YAML, Prisma schemas, policies. NO source code.
- `2_MAIN_CODING_FILES/`: Source code only (`frontend/`, `backend/`, `database/`). NO documentation.
- `3_PROJECT_BACKUP_AND_DIARY/`: Audit logs, API registries, task status matrices, context snapshots, surgical backups.

## 2. INVARIANT TECHNICAL CONSTRAINTS
- **Currency & Money:** Always integer cents. Never float, never `parseFloat`, never `toFixed` on financial arithmetic.
- **Passwords:** Argon2id (`m=65536, t=3, p=4`).
- **JWT:** RS256 algorithm via `jose`. Never HS256.
- **Sessions:** `HttpOnly; SameSite=Strict; Secure` cookies. Never localStorage.
- **SQL:** Parameterized bindings only. Never template string interpolation in SQL.
- **Safety:** Zero `eval()` calls. Zero hardcoded secrets.
- **TypeScript:** Strict mode enabled. Zero `any` types in production code.

## 3. AGENT EXECUTION GATES
- All code modifications must be verified with `npx tsc --noEmit --strict`.
- Every unit test suite must pass with ≥85% branch and line coverage.
- Group 8 (`/deploy`) requires explicit manual Boss confirmation.
