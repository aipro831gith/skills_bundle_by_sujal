# ANTIGRAVITY 2.0 ENTERPRISE MULTI-AGENT SYSTEM GUARDRAILS (AGENTS.md)
# GitHub Repository: https://github.com/aipro831gith/skills_bundle_by_sujal

Purpose: Enforces workspace rules, 3-folder layout, 3 diaries, zero-vulnerability security, and modular front-end design skills.

## 1. THE 3-TIER AGENT HIERARCHY
- TIER 1: SALESMAN / ACCOUNT MANAGER (Primary User-Facing AI) - Speaks in plain English to the Boss; hands off commands via `invoke_subagent`.
- TIER 2: TEAM LEADER / ENGINEERING MANAGER (Sub-Agent: Agent 00) - `skills/antigravity-team-leader/SKILL.md`. Asynchronous, non-blocking supervision & loop-engineering.
- TIER 3: SPECIALIZED WORKER SUB-AGENTS (19 Standalone Sub-Agents) - Isolated 1M-token context windows.

## 2. WORKSPACE STRUCTURE & THE 3 UNIVERSAL DIARIES
All files MUST strictly live within 3 root directories:
- `1_COMPLETE_DOCUMENTATION/`: Houses `master_spec.md` and ALL 7 Compulsory Documents:
  1. `01_product_requirements.md` (PRD)
  2. `02_api_contracts_and_endpoints.md`
  3. `03_database_schema_blueprint.md`
  4. `04_ui_ux_design_system.md`
  5. `05_hardware_and_sensor_protocols.md`
  6. `06_security_and_compliance_policy.md`
  7. `07_testing_and_qa_strategy.md`
- `2_MAIN_CODING_FILES/`: Houses application source code (`frontend/`, `backend/`, `database/`).
- `3_PROJECT_BACKUP_AND_DIARY/`: Houses surgical backups and 3 Universal Diaries:
  - `diary_1_audit_log.md`: Chronological action log of all agent dispatches & audits.
  - `diary_2_api_registry.md`: Active registry of APIs, WebSockets, & hardware baud rates.
  - `diary_3_task_matrix.md`: Real-time status tracker (PENDING | IN_PROGRESS | COMPLETED).

## 3. IMMUTABLE VERSIONING & PRE-CHANGE SURGICAL BACKUPS
- Immutable Spec Locking: If the Boss requests changes, create `master_spec_v2.md` (or `_v2.md`) and list numbered changes (1, 2, 3...) at the top, demarcating MVP features from Future Roadmap items. NEVER overwrite existing spec files.
- Pre-Change Surgical Insurance: Before modifying code during `/surgical`, automatically copy the target file to `3_PROJECT_BACKUP_AND_DIARY/backup_[file]_[timestamp]` BEFORE making edits.

## 4. ANTI-VIBE-CODING ZERO-TRUST SECURITY GUARDRAILS
- 100% Parameterized DB Queries: All DB queries MUST use parameter bindings (`$1`, `?`) to guarantee SQL injection immunity. Raw SQL concatenation is forbidden.
- Zero Hardcoded Secrets: Prohibit hardcoded API keys, JWT secrets, or DB passwords. Extract into `process.env` variables and provide `.env.template`.
- PII Leak Prevention: Never log passwords, auth tokens, or Personally Identifiable Information (PII) to stdout/stderr or console logs.
- DevSecOps Gate (`/sec-ops`): Auth pathways MUST pass DevSecOps scanning before deployment.
- Hardware Compliance Gate (`/hardware-compliance`): Embedded C/C++/Rust code MUST enforce MISRA guidelines, explicit pointer bounds checking, and zero-panic runtime safety.

## 5. EXTENSIBLE FRONT-END BUILDER ARCHITECTURE (`skills/antigravity-frontend-builder/`)
When `/build-all` is triggered, Agent 05 (Front-End Builder) loads `frontend_builder.md` and dynamically selects modular design skills:
- `1_MD_Design_Skill.md`: Web SaaS Layout (Collapsible sidebar, top header profile/theme switch, CSS grid).
- `2_MD_Design_Skill.md`: Mobile Flutter/Android Layout (Bottom navigation bar, sliver headers, card feeds).
- `3_MD_Design_Skill.md`: Enterprise ERP Layout (Multi-Tab views, dense data tables, pagination, sortable columns).
- `N_MD_Design_Skill.md`: Extensible pattern for future design skills (4_MD, 5_MD, etc.).
- 3-State Interactive Buttons: All buttons implement default, hover (0.2s scale 1.03x), and active (0.98x compress) states with automated loading spinners on click.

## 6. SCHEDULED MEMORY PRUNING (ANTI-CONTENT-ROT)
Integrate Antigravity Scheduled Tasks (Cron: `0 0 * * *`) via Agent 15 (Memory Keeper) to compress daily work into `context_snapshot.md` inside folder 3 nightly.
