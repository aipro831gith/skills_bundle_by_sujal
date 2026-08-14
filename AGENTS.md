LEVEL 2: PROJECT MASTER GUARDRAILS
File Configuration: Save verbatim as AGENTS.md in your Project Root Folder

Purpose: Enforces code quality, company rules for sub-agents, directory layouts, and sub-agent coordination for this workspace.

1. Strict 3-Folder Architecture Rule
The agent team must strictly build, organize, and read the project within the following three root directories:
• Folder 1: `1_COMPLETE_DOCUMENTATION/` - Houses the PRD, system design blueprints, interface specifications, and API data menus (All 7 Compulsory Documents live here).
• Folder 2: `2_MAIN_CODING_FILES/` - Houses the lightweight application engine, UI source components, backend modules, and cross-device compilation files.
• Folder 3: `3_PROJECT_BACKUP_AND_DIARY/` - Houses old critical code block backups, pre-change surgical snapshots, changelogs, and rolling laboratory diary entries.

2. Dedicated Team Leader Sub-Agent (`skills/antigravity-team-leader/SKILL.md`)
• Agent 00 (The Team Leader Sub-Agent) is a dedicated, standalone Tier 2 sub-agent.
• Spawned via `/start` by Tier 1 (Salesman AI) using `invoke_subagent`.
• Responsibilities: Loop-engineering supervision, task matrix management, error correction, re-prompting hallucinating workers, enforcing company rules, and managing `/start` & `/end`.

3. Universal Company Rules for ALL Sub-Agents
Regardless of an individual sub-agent's role or personality, ALL sub-agents dispatched via `invoke_subagent` MUST strictly follow these enterprise rules:
• Company Rule 1 (Zero Direct Chat): Sub-agents DO NOT communicate directly with the Boss. Sub-agents execute tasks in isolated background threads and report outputs to the Team Leader or update Universal Diaries.
• Company Rule 2 (Diary Compliance): Every sub-agent MUST log its completed work in `diary_1_audit_log.md` and update status in `diary_3_task_matrix.md`.
• Company Rule 3 (No Overwriting Specs): Never overwrite existing spec/architect documents. Always spawn `_v2.md` files with numbered change logs.
• Company Rule 4 (Surgical Insurance): Back up files to `3_PROJECT_BACKUP_AND_DIARY/` before executing code modifications.
• Company Rule 5 (Graceful API Fallbacks): Wrap all external third-party tools, hardware connections, and database calls in `try/catch` statements with fallback handling.

4. The 3 Universal Diaries
• `diary_1_audit_log.md`: Master chronological log of all agent actions, sub-agent dispatches, and code events.
• `diary_2_api_registry.md`: Active registry of external APIs, hardware baud rates, WebSockets, and pipeline contracts.
• `diary_3_task_matrix.md`: Real-time status tracker for all tasks (`PENDING` | `IN_PROGRESS` | `COMPLETED` | `BLOCKED`).

5. 7 Compulsory Documents Rule
All 7 documents are MANDATORY for all applications regardless of size:
1. `01_product_requirements.md` (PRD)
2. `02_api_contracts_and_endpoints.md`
3. `03_database_schema_blueprint.md`
4. `04_ui_ux_design_system.md`
5. `05_hardware_and_sensor_protocols.md`
6. `06_security_and_compliance_policy.md`
7. `07_testing_and_qa_strategy.md`

6. Parallel Worker Sub-Agent Spawning
During `/build-all` and `/qa-test`, the Team Leader MUST invoke multiple specialized sub-agents simultaneously (e.g., Front-End Builder, Backend Builder, DB Builder, Security Guard, GitHub Saver) to execute true concurrent background building.
