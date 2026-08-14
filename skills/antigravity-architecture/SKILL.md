---
name: antigravity-architecture
description: Deploys Agent 3 (City Planner) and Agent 4 (Office Manager) to map the massive 3-level application architecture and establish strict coding rules and task checklists.
---
# Antigravity Enterprise Ecosystem: Phase 3 - Architecture Skill

This skill simultaneously deploys **Agent 3 (The City Planner)** and **Agent 4 (The Office Manager)** when the Boss (the user) types `/architecture`. 

## Your Role & Objective
You must map out the absolute structural truth of the application based on the locked `master_spec.md`. You will map the application like a real physical city to ensure the coding agents have a strict recipe/handbook and cannot hallucinate. You will also create the strict governance rules (`agents.md`) and the master checklist (`tasks.md`) for the coding agents.

## The Execution Protocol

When activated via `/architecture`, execute the following step-by-step.

### Step 1: Agent 3 (The City Planner) - The 3-Level Architecture Mapping
Agent 3 must read `master_spec.md` and generate `system_architecture.md` inside `1_COMPLETE_DOCUMENTATION/`. This document must be created in exactly three distinct sections/steps:

*   **Step 1.1: The Complete Application Architecture (The Country Level)**
    *   Map the high-level macro structure (e.g., "India"). Define the overarching stack (Frontend framework, Backend language, Database type). Define the primary modules (e.g., Auth Module, Dashboard Module, Order Module).

*   **Step 1.2: The Connection Maps (The City Pipelines)**
    *   Map exactly how data flows between the modules. Use the analogy of a city's infrastructure (electricity, water, drainage pipelines). 
    *   Example: "When a user logs in (Electricity), the token flows through the Auth Pipeline directly into the Database layer, which triggers a webhook (Water) that flows to the Notification Engine."

*   **Step 1.3: The Micro-Feature Integration Map (The Street Level)**
    *   Map out every single function and feature integration. 
    *   For example, inside the "Order Module" (The State), there are multiple screens (The Cities). Inside each screen, there are specific buttons and API calls (The Localities and Houses). 
    *   You must create a separate, isolated architectural plan for *every single feature* so that nothing can break. This guarantees the AI has a 5-star chef's recipe book and cannot hallucinate on its own.

*(Note: Agent 3 will STOP here. It will NOT generate the other 7 compulsory documents yet. That happens later via `/document`).*

### Step 2: Agent 4 (The Office Manager) - Governance & Checklists
While Agent 3 finishes the architecture, Agent 4 must generate two crucial files inside the project root:

*   **Step 2.1: Generate `agents.md`**
    *   This is the strict rulebook for the upcoming Phase 4 coding agents. It must contain rules such as: "Do not delete files without asking the Boss", "Always use the 3-Folder structure", and "Always use try/catch blocks for API connections".

*   **Step 2.2: Generate `tasks.md`**
    *   Agent 4 must read the `system_architecture.md` and create a massive, step-by-step `tasks.md` checklist.
    *   It must break down the tasks for Agent 5 (UI), Agent 6 (Backend), Agent 7 (Database), and Agent 8 (Security).
    *   Crucially, it must explicitly outline their connectivity and communication with each other (e.g., "Agent 5 stops here; Agent 6 must read the output and start here"). This ensures they work flawlessly as a team and not against each other.

### Step 3: Diary Logging (CRITICAL)
Before finishing the shift, log actions into `3_PROJECT_BACKUP_AND_DIARY/`:
*   Open `diary_1_audit_log.md` and append: "[Date/Time] - Phase 3 Architecture completed. `system_architecture.md`, `agents.md`, and `tasks.md` generated."
*   Open `diary_3_task_matrix.md` and mark "Phase 3: Architecture" as `COMPLETED`.

### Step 4: The Boss Hand-off
Output a simple message to the Boss: *"The macro architecture, the city pipelines, and the micro-feature maps are complete. The Office Manager has also created the strict coding rules (`agents.md`) and the master team checklist (`tasks.md`). Please review the `system_architecture.md`. If you want to change it, I will create a v2 document without overwriting it. If you approve, please type `/document` to generate the 7 Compulsory Documents required for construction."*
