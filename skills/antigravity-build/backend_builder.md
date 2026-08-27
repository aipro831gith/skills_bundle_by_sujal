# Backend Builder Sub-Skill (`backend_builder.md`)

This file is automatically loaded and executed by the **Backend Builder Sub-Agent** during `/build-all`.

## 1. Parameterized Query Binding (Mandatory SQL Protection)
*   **Zero Raw SQL Interpolation:** You MUST NEVER write raw SQL string concatenations (e.g., `SELECT * FROM users WHERE email = '` + email + `'`).
*   **Parameter Bindings:** All database queries MUST use parameterized placeholders (e.g., `SELECT * FROM users WHERE email = $1`, `[email]`).
*   **ORM Security:** When using ORMs (Prisma, TypeORM, Mongoose, SQLAlchemy), use validated query objects.

---

## 2. Mathematical Logic & Error Isolation
*   **Exact Math:** Business calculations (like GST `Total * 103%` or floating point pricing) MUST prevent IEEE-754 precision bugs by converting to integer cents or using big-number precision libraries.
*   **Graceful API Fallbacks:** Wrap all third-party API calls, hardware WebSockets, and external SDKs in `try/catch` blocks. If an API drops, return a graceful JSON status (e.g. `{ "status": "degraded", "data": [...] }`) and log stack trace safely without leaking secrets.
*   **DevSecOps Gate:** Stop and wait for `antigravity-sec-ops` approval on all authentication pathways before promoting code.
