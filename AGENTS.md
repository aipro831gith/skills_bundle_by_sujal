LEVEL 2: PROJECT MASTER GUARDRAILS
File Configuration: Save verbatim as AGENTS.md in your Project Root Folder

Purpose: Enforces code quality, company rules for sub-agents, directory layouts, DevSecOps scanning, and hardware safety compliance for this workspace.

1. Strict 3-Folder Architecture Rule
The agent team must strictly build, organize, and read the project within the following three root directories:
• Folder 1: `1_COMPLETE_DOCUMENTATION/` - Houses the PRD, system design blueprints, interface specifications, and API data menus (All 7 Compulsory Documents live here).
• Folder 2: `2_MAIN_CODING_FILES/` - Houses the lightweight application engine, UI source components, backend modules, and cross-device compilation files.
• Folder 3: `3_PROJECT_BACKUP_AND_DIARY/` - Houses old critical code block backups, pre-change surgical snapshots, changelogs, and rolling laboratory diary entries.

2. Dedicated 1-to-1 Sub-Agent Skill Directory Mapping
Every single agent and sub-agent has its own standalone dedicated skill directory in `skills/`:
- `skills/antigravity-start/` (Salesman AI - Tier 1)
- `skills/antigravity-team-leader/` (Agent 00: Team Leader - Tier 2)
- `skills/antigravity-research/` (Agent 01: Researcher)
- `skills/antigravity-spec/` (Agent 02: Spec Writer)
- `skills/antigravity-city-planner/` (Agent 03: City Planner)
- `skills/antigravity-office-manager/` (Agent 04: Office Manager)
- `skills/antigravity-documentarian-architect/` (Agent 03b: Documentarian Architect)
- `skills/antigravity-frontend-builder/` (Agent 05: Front-End Builder)
- `skills/antigravity-backend-builder/` (Agent 06: Backend Builder)
- `skills/antigravity-database-builder/` (Agent 07: Database Builder)
- `skills/antigravity-security-guard/` (Agent 08: Security Guard)
- `skills/antigravity-github-saver/` (Agent 09: GitHub Saver)
- `skills/antigravity-spell-checker/` (Agent 10: Spell Checker)
- `skills/antigravity-math-checker/` (Agent 11: Math Checker)
- `skills/antigravity-human-tester/` (Agent 12: Human Tester)
- `skills/antigravity-polish/` (Agent 13: Polisher)
- `skills/antigravity-surgical/` (Agent 14: Surgeon)
- `skills/antigravity-memory-keeper/` (Agent 15: Memory Keeper)
- `skills/antigravity-user-manual-writer/` (Agent 16: User Manual Writer)
- `skills/antigravity-auditor/` (Agent 17: Auditor Recovery)
- `skills/antigravity-sec-ops/` (Agent 18: DevSecOps Agent)
- `skills/antigravity-hardware-compliance/` (Agent 19: Hardware Compliance Agent)

3. Universal Company Rules for ALL Sub-Agents
• Company Rule 1 (Zero Direct Chat): Sub-agents execute tasks in isolated background threads and report outputs to the Team Leader or update Universal Diaries.
• Company Rule 2 (Diary Compliance): Log work in `diary_1_audit_log.md` and update status in `diary_3_task_matrix.md`.
• Company Rule 3 (No Overwriting Specs): Never overwrite existing spec/architect documents. Always spawn `_v2.md` files with numbered changelogs.
• Company Rule 4 (Surgical Insurance): Back up files to `3_PROJECT_BACKUP_AND_DIARY/` before executing code modifications.
• Company Rule 5 (Graceful API Fallbacks): Wrap all external third-party tools, hardware connections, and database calls in `try/catch` statements with fallback handling.
• Company Rule 6 (Scheduled Context Pruning): Use Scheduled Tasks (Cron) for nightly context summaries to prevent content rot and context drift.

4. The 3 Universal Diaries
• `diary_1_audit_log.md`: Master chronological log of all agent actions, sub-agent dispatches, DevSecOps audits, and code events.
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
