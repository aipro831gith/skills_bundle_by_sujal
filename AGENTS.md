# AGENTS.md — Master Agent Swarm Registry
# Skills Bundle by Sujal
# Platform: Google Antigravity 2.0 + Antigravity IDE
# Last updated: 2026-08-28

---

## PLATFORM OVERVIEW: HOW THIS REPOSITORY RUNS

This repository is designed for use on **two complementary Antigravity surfaces**:

| Surface | Best For | Discovery Path |
|---------|----------|----------------|
| **Antigravity 2.0** (desktop app) | Long-running agent fleets, background task scheduling, multi-agent orchestration, Cron timers | `.agents/` folder auto-discovered at workspace root |
| **Antigravity IDE** (VS Code fork) | Inline code editing, autocomplete, inline `Ctrl+I` fixes, diagnostic auto-fixes, code lenses | `.agents/` folder auto-discovered from project root |
| **Antigravity CLI (`agy`)** | Terminal-based fast agent tasks, scripted automation, CI pipelines | `.agents/` folder discovered from CWD upward to `.git` root |

All three surfaces share the same `.agents/` customization folder. Skills, rules, MCP configs, and hooks defined here apply everywhere.

---

## ARCHITECTURE: 3-TIER AGENT HIERARCHY

```
┌───────────────────────────────────────────────────────────────────┐
│  TIER 1 — USER-FACING FRONT DESK                                  │
│  Salesman AI / Account Manager                                     │
│  Skill: .agent/skills/start_skill.md                              │
│  Interface: Antigravity 2.0 Chat Canvas (slash: /start, /end)     │
└──────────────────────────┬────────────────────────────────────────┘
                           │ dispatches (send_message)
┌──────────────────────────▼────────────────────────────────────────┐
│  TIER 2 — ENGINEERING MANAGER                                     │
│  Team Leader (Agent 00)                                           │
│  Skill: .agent/skills/team_leader_skill.md                        │
│  Interface: Antigravity 2.0 Background Tasks pane                 │
│  Responsibilities: 8-Group Conveyor Belt, Loop Ceilings, Routing  │
└──────────────────────────┬────────────────────────────────────────┘
                           │ orchestrates (invoke_subagent)
┌──────────────────────────▼────────────────────────────────────────┐
│  TIER 3 — SPECIALIZED WORKER SUB-AGENTS (×15+)                   │
│  Antigravity 2.0 Subagents Pane / Background Tasks               │
│  See Group Registry below                                         │
└───────────────────────────────────────────────────────────────────┘
```

---

## GLOBAL MANDATORY RULES (apply to ALL agents on ALL surfaces)

> Rules in this `AGENTS.md` are loaded by Antigravity using **hierarchical
> directory walking** from the CWD upward to the `.git` root.
> They apply to every agent conversation in this repository.

### Security Standards (Zero-Trust)
- ALL password hashing: **Argon2id** (m=65536, t=3, p=4). `bcrypt` is a
  legacy fallback at ≥12 rounds only. MD5/SHA-1/SHA-256 for passwords: FORBIDDEN.
- ALL JWT: **RS256** (asymmetric). HS256 (symmetric): FORBIDDEN.
- ALL session cookies: `HttpOnly + SameSite=Strict + Secure`. `localStorage`
  for auth tokens: FORBIDDEN.
- ALL SQL queries: parameterized bindings. String interpolation in SQL: FORBIDDEN.
- ALL CORS: exact whitelist. `cors({ origin: '*' })` in production: FORBIDDEN.
- ALL monetary values: integer cents (e.g., \$10.99 = 1099). Float money math: FORBIDDEN.

### Verification Gates (must exit 0 before any pipeline stage advances)
- `npx tsc --noEmit --strict` — TypeScript compilation
- `npx eslint . --max-warnings 0` — Lint gate
- `npm audit --audit-level=high` — CVE gate
- `npx jest --coverage --coverageThreshold='{"global":{"lines":85}}'` — Test gate
- `semgrep --config=p/sql-injection` — SAST gate (or grep fallback)

### Folder Enforcement
- Documentation ONLY: `1_COMPLETE_DOCUMENTATION/`
- Source code ONLY: `2_MAIN_CODING_FILES/`
- Logs/backups ONLY: `3_PROJECT_BACKUP_AND_DIARY/`
- Code MUST NOT go into folder 1 or 3.

---

## 8-GROUP CONVEYOR BELT

| Group | Phase | Slash Command | Hard Stop? | Required Gates Before Starting |
|-------|-------|---------------|------------|-------------------------------|
| G1 | Product Discovery | `/research` → `/spec` | ✅ After spec, Boss must approve | None |
| G2 | System Blueprint | `/architecture` → `/document` | ✅ After 7 docs, Boss must approve | G1 COMPLETED |
| G3 | Full-Stack Build | `/build-all` | No (auto) | 7 docs + agents.md + tasks.md |
| G4 | QA Audit | `/qa-test` | No (auto) | G3 COMPLETED + `tsc` exit 0 |
| G5 | Polish Loop | `/polish` (max 2×) | No | G4 PASSED |
| G6 | Compliance Audit | `/sec-ops` → `/audit` | No | G5 COMPLETED |
| G7 | Documentation | auto | No | G6: 0 CRITICAL/HIGH |
| G8 | Deployment | `/deploy` **MANUAL ONLY** | ✅ Always | ALL previous + 7 pre-deploy gates |

---

## SUB-AGENT SPAWNING SCHEMA

Agents in this repository are spawned using Antigravity's `invoke_subagent`
tool. Every sub-agent dispatch must follow this schema:

```json
{
  "TypeName": "<subagent-type-name>",
  "Role": "<2-5 word human-readable role title>",
  "Prompt": "<Complete, self-contained task prompt. Must include: which files to read, what to build, what to log to diary_1, what to update in diary_3. NEVER assume the subagent has conversation context — state everything explicitly.>",
  "Model": "inherit"
}
```

### Inter-Agent Messaging (send_message)
When a parent agent needs to send instructions mid-task to an already-running
sub-agent, use the `send_message` tool with the sub-agent's `conversationId`
(returned by `invoke_subagent`).

```
send_message(
  Recipient: "<conversationId from invoke_subagent>",
  Message: "<Updated instructions or additional context>"
)
```

### State Synchronization via Diary Files
All agents synchronize state through 3 universal diary files — not through
direct memory transfer:

| Diary File | Purpose | Who Writes | Who Reads |
|-----------|---------|-----------|----------|
| `3_PROJECT_BACKUP_AND_DIARY/diary_1_audit_log.md` | All agent actions + timestamps | Every agent (mandatory) | Team Leader, Auditor |
| `3_PROJECT_BACKUP_AND_DIARY/diary_2_api_registry.md` | All API endpoints defined | Backend Builder, Document Agent | Frontend Builder, QA |
| `3_PROJECT_BACKUP_AND_DIARY/diary_3_task_matrix.md` | Phase status (NOT_STARTED / IN_PROGRESS / COMPLETED) | Team Leader | All agents |

---

## PROGRESSIVE DISCLOSURE: HOW SKILLS ARE LOADED

Antigravity uses progressive disclosure to manage context window efficiently:

1. **At conversation start**: Only skill `name` and `description` (from YAML
   frontmatter) are injected into context. Full skill body is NOT loaded.
2. **On activation**: When the model (or user) explicitly references a skill
   (via slash command or `view_file`), the full `SKILL.md` body is loaded.
3. **Memory eviction**: If context window pressure is high, skills that have
   not been referenced in recent turns may be evicted. The name/description
   index is always retained.

**Implication for skill authors:** Every `SKILL.md` MUST have a `description`
in YAML frontmatter that fully explains what the skill does. The description
is the ONLY guaranteed-loaded content.

---

## AGENT GROUP REGISTRY

### GROUP 1: Product Discovery

| Agent | File | Skill | Antigravity Tool |
|-------|------|-------|-----------------|
| Researcher | `agents/research_agent.md` | `.agent/skills/research_skill.md` | `invoke_subagent` (Workspace: inherit) |
| Spec Writer | `agents/spec_agent.md` | `.agent/skills/spec_skill.md` | `invoke_subagent` (Workspace: inherit) |

### GROUP 2: System Blueprint (7 Compulsory Documents)

| Agent | File | Skill |
|-------|------|-------|
| System Architect (Agent 03) | `agents/architecture_agent.md` | `.agent/skills/architecture_skill.md` |
| Documentarian (Agent 03b) | `agents/document_agent.md` | `.agent/skills/document_skill.md` |

### GROUP 3: Full-Stack Construction

| Agent | File | Skill | Runs concurrently? |
|-------|------|-------|-------------------|
| Build Orchestrator | `agents/builder_agent.md` | `.agent/skills/build_skill.md` | No — dispatches others |
| Frontend Builder (Agent 05) | `agents/frontend_builder.md` | `.agent/skills/frontend_builder_skill.md` | ✅ Yes (parallel) |
| Backend Builder (Agent 06) | `agents/backend_builder.md` | `.agent/skills/backend_builder_skill.md` | ✅ Yes (parallel) |
| Database Architect (Agent 07) | `agents/database_builder.md` | `.agent/skills/database_builder_skill.md` | ✅ Yes (parallel) |
| Security Guard (Agent 08) | `agents/security_guard.md` | `.agent/skills/security_guard_skill.md` | ✅ Yes (parallel) |
| Version Control (Agent 09) | `agents/github_saver.md` | `.agent/skills/github_saver_skill.md` | ✅ Yes (parallel) |

### GROUP 4: Quality Assurance

| Agent | File | Skill |
|-------|------|-------|
| QA Agent (Agent 10) | `agents/qa_agent.md` | `.agent/skills/qa_skill.md` |
| Surgeon (Agent 14) | `agents/surgical_agent.md` | `.agent/skills/surgical_skill.md` |

### GROUP 5: Polish (Max 2 Iterations — Team Leader enforces)

| Agent | File | Skill |
|-------|------|-------|
| Polisher (Agent 13) | `agents/polish_agent.md` | `.agent/skills/polish_skill.md` |

### GROUP 6: Audit & Compliance

| Agent | File | Skill |
|-------|------|-------|
| Recovery Auditor (Agent 17) | `agents/auditor_agent.md` | `.agent/skills/auditor_skill.md` |
| DevSecOps Scanner (Agent 18) | `agents/sec_ops_agent.md` | `.agent/skills/sec_ops_skill.md` |
| Hardware Safety (Agent 19) | `agents/hardware_compliance.md` | `.agent/skills/hardware_compliance_skill.md` |

### GROUP 7: Documentation & Memory

| Agent | File | Skill |
|-------|------|-------|
| Memory Keeper (Agent 15) | `agents/memory_agent.md` | `.agent/skills/memory_skill.md` |
| Manual Writer (Agent 16) | `agents/user_manual_writer.md` | `.agent/skills/user_manual_writer_skill.md` |

### GROUP 8: Deployment (MANUAL ONLY — Team Leader NEVER auto-invokes)

| Agent | File | Skill |
|-------|------|-------|
| Deployment Orchestrator | `agents/deploy_agent.md` | `.agent/skills/deploy_skill.md` |
| Web Specialist | `agents/web_deploy_agent.md` | `.agent/skills/web_deploy_skill.md` |
| Play Store Specialist | `agents/playstore_deploy_agent.md` | `.agent/skills/playstore_deploy_skill.md` |
| App Store Specialist | `agents/appstore_deploy_agent.md` | `.agent/skills/appstore_deploy_skill.md` |
| GitHub CI/CD Specialist | `agents/github_deploy_agent.md` | `.agent/skills/github_deploy_skill.md` |

---

## MCP TOOL GOVERNANCE

Tool execution policies (configured in Antigravity Settings or `mcp_config.json`):

| Policy | Trigger | Examples |
|--------|---------|---------|
| `always-proceed` | Read-only, non-destructive tools | File reads, web searches, `git log`, `ls` |
| `request-review` | Destructive or irreversible commands | `DROP TABLE`, `rm -rf`, force-push, production deploy |
| `proceed-in-sandbox` | Commands with side effects that can be safely isolated | `npm install`, test database commands |

---

## CONSOLIDATION LOG

| Retired File | Canonical Replacement |
|-------------|----------------------|
| `build_backend.md` | `backend_builder_skill.md` |
| `build_frontend.md` | `frontend_builder_skill.md` |
| `memory_keeper_skill.md` | `memory_skill.md` |
| `documentarian_architect_skill.md` | `document_skill.md` |

---

## SLASH COMMAND QUICK REFERENCE

| Command | Phase | Dispatches |
|---------|-------|-----------|
| `/start` | T1 | Team Leader init |
| `/end` | T1 | Graceful shutdown |
| `/research` | G1 | Researcher sub-agent |
| `/spec` | G1 | Spec Writer sub-agent |
| `/architecture` | G2 | Architecture Agent |
| `/document` | G2 | Documentarian sub-agent |
| `/build-all` | G3 | 5 parallel worker sub-agents |
| `/qa-test` | G4 | QA Agent |
| `/polish` | G5 | Polisher sub-agent (max 2×) |
| `/surgical` | G4/G5/G6 | Surgeon sub-agent |
| `/sec-ops` | G6 | DevSecOps Scanner |
| `/audit` | G6 | Recovery Auditor |
| `/hardware-compliance` | G6 | Hardware Safety Agent |
| `/context-save` | G7 | Memory Keeper (also runs nightly via Cron) |
| `/context-load` | G7 | Memory Restore sub-agent |
| `/deploy` | G8 ⚠️ MANUAL | Deployment Orchestrator |
