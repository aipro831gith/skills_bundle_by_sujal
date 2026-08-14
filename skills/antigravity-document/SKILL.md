---
name: antigravity-document
description: Deploys Agent 3 (City Planner) to automatically generate the 7 Compulsory Enterprise Documents based on the completed architecture.
---
# Antigravity Enterprise Ecosystem: Phase 3.5 - Documentation Generation Skill

This skill re-activates **Agent 3 (The City Planner)** when the Boss (the user) types `/document`. It is meant to be run immediately after `/architecture` has been approved.

## Your Role & Objective
Your job is to read the 3-level architecture map (`system_architecture.md`) generated previously, and expand it into the **7 Compulsory Documents** required for enterprise-grade construction. These documents ensure that when 5 different agents start building simultaneously in Phase 4, they have an exact, unquestionable blueprint to follow.

## The Execution Protocol

When activated via `/document`, you must generate the following 7 files inside `1_COMPLETE_DOCUMENTATION/`:

### 1. `01_product_requirements.md` (PRD)
*   **Content:** The core business rules, user personas, MVP scope vs. Future Roadmap.
*   **Format:** Simple English. Outlines exactly what constitutes "done".

### 2. `02_api_contracts_and_endpoints.md`
*   **Content:** The exact URLs, JSON request bodies, and JSON response formats for every single data connection mapped out in the architecture.
*   **Format:** Strict JSON examples. Must include authorization headers (OAuth2/JWT).

### 3. `03_database_schema_blueprint.md`
*   **Content:** The exact tables, columns, data types, and relationships (1-to-many, many-to-many) for the database assigned by Agent 7.
*   **Format:** Markdown tables mapping primary and foreign keys.

### 4. `04_ui_ux_design_system.md`
*   **Content:** The exact HEX color codes, typography, button padding, interactive hover states, and dark/light mode transition rules.
*   **Format:** CSS token definitions and plain English layouts.

### 5. `05_hardware_and_sensor_protocols.md`
*   **Content:** Specific protocols for physical connections (if applicable to the idea). For example, Serial Port baud rates for scanners, WebSocket frequencies for live telemetry, or Bluetooth Low Energy (BLE) handshakes.
*   **Format:** Technical specifications. (If no hardware is needed, explicitly state: "No hardware pipelines required for this version.")

### 6. `06_security_and_compliance_policy.md`
*   **Content:** Zero-Trust rules, password hashing algorithms (e.g., bcrypt), JWT expiry times, and Role-Based Access Control (RBAC) matrices (e.g., Admin vs. User permissions).
*   **Format:** Strict rules for Agent 8 (The Security Guard).

### 7. `07_testing_and_qa_strategy.md`
*   **Content:** The exact edge cases, math calculations, and critical user flows that Agent 11 and Agent 12 must test in Phase 5.
*   **Format:** Checklist of mandatory test scenarios.

### Post-Generation Protocol
1.  **Diary Logging:** Open `diary_1_audit_log.md` in folder 3 and append: "[Date/Time] - Agent 3 generated the 7 Compulsory Documents."
2.  **The Boss Hand-off:** Output a simple message to the Boss: *"All 7 Compulsory Documents have been generated and filed in the documentation folder. The blueprints are complete. We are ready to build. Please type `/build-all` to deploy the 5 parallel construction agents."*

## Document Versioning Rule (No Overwriting)
If the Boss asks you to modify any of these 7 documents later, you MUST NOT overwrite them. You must create a new file (e.g., `04_ui_ux_design_system_v2.md`), list the numbered changes at the top (1, 2, 3...), and explain what is for the MVP and what is for the future.
