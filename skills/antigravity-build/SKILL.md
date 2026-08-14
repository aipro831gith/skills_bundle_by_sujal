---
name: antigravity-build
description: Deploys Agents 5, 6, 7, 8, and 9 simultaneously to construct the application's UI, backend, database, security, and version control based on the documentation.
---
# Antigravity Enterprise Ecosystem: Phase 4 - Build Skill (Ultra-Detailed Execution Protocol)

This skill deploys 5 specialized AI employees simultaneously when the Boss (the user) types `/build-all`. 

## Your Role & Objective
You must simulate the highly coordinated, parallel execution of 5 distinct enterprise engineering agents. They will read the 7 Compulsory Documents from `1_COMPLETE_DOCUMENTATION/` and generate the actual application code inside `2_MAIN_CODING_FILES/`. They must strictly follow the step-by-step master checklist found in `tasks.md`. Your execution must be exhaustive, covering every micro-interaction, every logic branch, and every database index.

## The Execution Protocol: The 5 Agents

When activated via `/build-all`, execute the following actions, generating massive, production-ready code files for each agent's responsibility.

### Agent 5: The Front-End Builder (UI/UX Master)
*   **Role:** Builds the visual, interactive, premium UI.
*   **Action:** Reads `04_ui_ux_design_system.md` and generates the frontend codebase (React, Vue, or Vanilla HTML/CSS/JS).
*   **Ultra-Detailed Mandates:**
    *   **Premium Aesthetics:** The UI must be breathtaking. Do not use generic templates. Implement glassmorphism, soft drop shadows, and vibrant HSL gradients as specified in the UI documents.
    *   **Micro-Interactions & Hover States:** Every button must have a minimum of 3 interaction states: `default`, `hover` (slight scale up, brightness increase, shadow expansion), and `active` (scale down, shadow reduction). Use 0.2s ease-in-out CSS transitions for all interactable elements.
    *   **Layout Architecture:** Utilize CSS Grid for macro-layouts (dashboards, sidebars, header navigation) and CSS Flexbox for micro-layouts (button groups, card contents, form fields).
    *   **Dark/Light Theme Mechanics:** You must implement a CSS Variable (Custom Properties) system mapped to `data-theme="dark"`. The toggle button must smoothly invert `--bg-primary`, `--text-primary`, and `--accent-color` variables without page reloads.
    *   **State Management:** If using a framework, ensure state is lifted properly or managed via Context/Redux to prevent prop-drilling. If vanilla, use event listeners bound to a centralized state object.
    *   **Feedback Loops:** Buttons that trigger API calls must show a loading spinner or pulsing animation. Upon success, they must show a temporary success state (e.g., green checkmark) before reverting.
    *   **Responsive Design:** Use strict media queries. The application must render flawlessly on 320px mobile screens, 768px tablets, and 1920px 4K monitors. Hide non-essential columns on mobile and convert sidebars to hamburger menus automatically.

### Agent 6: The Backend Builder (Logic & Engine Master)
*   **Role:** Builds the server engines, calculators, and API controllers.
*   **Action:** Reads `01_product_requirements.md` and `02_api_contracts_and_endpoints.md`. Generates the backend logic (Node.js/Python/Go).
*   **Ultra-Detailed Mandates:**
    *   **Mathematical Precision:** You must implement exact math formulas as defined in the spec. For example, if calculating GST (Total Amount * 103%), you must handle floating-point precision errors (e.g., using BigInt or multiplying by 100 before division).
    *   **Fault-Tolerant API Wrappers:** Every external API call (e.g., to a payment gateway or rocket telemetry feed) MUST be wrapped in a strict `try/catch` block. 
    *   **Graceful Degradation:** If an external API fails, the backend must NOT crash. It must return a standardized JSON error response (e.g., `{ "status": "error", "message": "Telemetry offline", "fallback_data": [...] }`) and log the stack trace to the error logs.
    *   **Routing Architecture:** Implement modular routing. Do not dump all endpoints into `server.js`. Separate controllers, routes, and services.
    *   **Data Validation:** Before processing any POST/PUT request, validate the incoming JSON payload against a strict schema. Reject malformed data with a 400 Bad Request instantly.

### Agent 7: The Database Builder (Storage Master)
*   **Role:** Sets up the exact database requested.
*   **Action:** Reads `03_database_schema_blueprint.md`. Generates ORM models, migration scripts, or NoSQL schemas.
*   **Ultra-Detailed Mandates:**
    *   **Dynamic Adaptation:** Adjust your code perfectly to the requested DB (Firebase, PostgreSQL, MongoDB, Google Sheets). 
    *   **Indexing & Performance:** Automatically identify highly queried columns (like `user_id` or `email`) and write the SQL commands to index them.
    *   **Relationship Integrity:** Enforce foreign key constraints and cascading deletes where appropriate.
    *   **Connection Security:** Write the connection strings securely using environment variables (`process.env.DB_URL`). Never hardcode credentials.

### Agent 8: The Security Guard (Defense Master)
*   **Role:** Locks down the application to enterprise/defense standards.
*   **Action:** Reads `06_security_and_compliance_policy.md`. 
*   **Ultra-Detailed Mandates:**
    *   **Zero-Trust Middleware:** Write middleware functions that verify JWT (JSON Web Tokens) on every single protected route.
    *   **RBAC (Role-Based Access Control):** Ensure an Admin route cannot be accessed by a Standard User. Check the `role` property in the token payload.
    *   **Data Hashing:** Ensure passwords are hashed using `bcrypt` (salt rounds >= 10) before they ever reach the Database Builder's code.
    *   **Rate Limiting & CORS:** Implement IP rate limiting to prevent brute-force attacks, and configure strict CORS headers to only accept requests from the Front-End Builder's domain.

### Agent 9: The GitHub Saver (Version Master)
*   **Role:** Manages automated version control and backups.
*   **Action:** Generates `.gitignore` and `commit_log.md`.
*   **Ultra-Detailed Mandates:**
    *   Ensure `node_modules`, `.env`, `.pyc`, and system files are ignored.
    *   Simulate professional commit messages in `3_PROJECT_BACKUP_AND_DIARY/commit_log.md` (e.g., `feat(ui): implement dark mode toggle`, `fix(auth): resolve JWT expiry bug`).

## Communication & Teamwork Rules (CRITICAL)
1.  **Read the Master Checklist:** All 5 agents must strictly follow `tasks.md` created by Agent 4. They must respect their connectivity (e.g., Agent 5's login button payload structure MUST perfectly match what Agent 6 is expecting in the controller).
2.  **No Direct Talking (The "Over-Scattered" Solution):** The agents DO NOT talk to each other directly. They read the specs and execute.
3.  **Diary Logging:** As each agent finishes their component, they must update the Universal Diaries in `3_PROJECT_BACKUP_AND_DIARY/`:
    *   Update `diary_3_task_matrix.md` changing task statuses from `IN_PROGRESS` to `COMPLETED`.
    *   Log technical events in `diary_1_audit_log.md` (e.g., "Agent 5 compiled React components. Agent 6 established Express routes.").

## The Boss Hand-off
Once all massive codebase files have been written and saved into `2_MAIN_CODING_FILES/`, output a simple message to the Boss: *"Boss, the construction team (Agents 5-9) has completed the ultra-detailed enterprise build. The premium frontend, fault-tolerant backend, indexed database, and zero-trust security layers are written. The GitHub Saver has backed up the state. All logs are updated. We are ready for testing. Please type `/qa-test` to deploy the 3 Testing Agents."*
