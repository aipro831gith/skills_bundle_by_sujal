# Master Agents Index (AGENTS.md) — Enterprise Edition v2.0
# Refactored to zero-ambiguity, 7-section production standards
# Deduplication applied: 4 redundant files retired (see Consolidation Log below)

---

## ARCHITECTURE: 3-TIER SYSTEM

```
┌─────────────────────────────────────────────────┐
│  TIER 1: Salesman AI (User-Facing)              │
│  @agents/start_agent.md                         │
│  Slash: /start, /end                            │
└──────────────────────┬──────────────────────────┘
                       │ dispatches
┌──────────────────────▼──────────────────────────┐
│  TIER 2: Team Leader (Engineering Manager)      │
│  @agents/team_leader.md                         │
│  Manages: 8-Group Conveyor Belt                 │
└──────────────────────┬──────────────────────────┘
                       │ orchestrates
┌──────────────────────▼──────────────────────────┐
│  TIER 3: Specialized Worker Sub-Agents (×15)    │
│  Groups 1-8 (see below)                         │
└─────────────────────────────────────────────────┘
```

---

## MANDATORY GLOBAL RULES (Apply to ALL agents)

### Financial-Grade Standards
- **Money precision:** ALL monetary values stored as integer cents. NEVER floating-point.
- **Password hashing:** Argon2id PRIMARY (m=65536, t=3, p=4). bcrypt ≥12 rounds fallback ONLY.
- **JWT:** RS256 asymmetric algorithm via `jose` library. HS256 FORBIDDEN.
- **Sessions:** HttpOnly + SameSite=Strict + Secure cookies. NEVER localStorage for tokens.
- **CORS:** Whitelist-only. `cors({ origin: '*' })` FORBIDDEN in production.

### Zero-Trust Standards
- NEVER hardcode secrets. ALL from `process.env`.
- NEVER string-interpolate SQL. ALL queries parameterized.
- NEVER log PII (email, password, SSN, CC) to console or disk.
- NEVER deploy without staging verification and rollback insurance.

### Verification Standards
- `tsc --noEmit` MUST exit 0 before any Group advances.
- `npm audit --audit-level=high` MUST exit 0 before deployment.
- Test coverage MUST be ≥ 85% lines for service/utility functions.
- DevSecOps audit MUST show 0 CRITICAL, 0 HIGH findings before Group 7.

---

## 8-GROUP CONVEYOR BELT

| Group | Phase | Slash Command | Hard Gates | Key Skills |
|-------|-------|---------------|------------|------------|
| **G1** | Product Discovery | `/research` → `/spec` | HARD STOP after spec for Boss approval | research_skill, spec_skill |
| **G2** | System Blueprint | `/architecture` → `/document` | HARD STOP after 7 docs for Boss approval | architecture_skill, document_skill |
| **G3** | Full-Stack Build | `/build-all` | All 7 docs + agents.md + tasks.md must exist | build_skill (dispatches 5 workers) |
| **G4** | QA Audit | `/qa-test` | G3 COMPLETED + tsc exit 0 | qa_skill |
| **G5** | Polish Loop | `/polish` | G4 PASS | polish_skill → surgical_skill (MAX 2 loops) |
| **G6** | Audit & Compliance | `/sec-ops` + `/audit` + `/hardware-compliance` | G5 PASS | sec_ops_skill, auditor_skill, hardware_compliance_skill |
| **G7** | Documentation & Memory | auto | G6 PASS (0 CRITICAL/HIGH) | memory_skill, user_manual_writer_skill |
| **G8** | Production Release | `/deploy` MANUAL ONLY | ALL previous groups + 7 pre-deploy gates | deploy_skill → specialists |

---

## TIER 1: User-Facing Management

- [@agents/start_agent.md](agents/start_agent.md) — Salesman AI / Account Manager
  - Skill: `.agent/skills/start_skill.md`
  - Slash: `/start`, `/end`

---

## TIER 2: Engineering Management & Orchestration

- [@agents/team_leader.md](agents/team_leader.md) — Agent 00: Team Leader & 8-Group Orchestrator
  - Skill: `.agent/skills/team_leader_skill.md`

---

## TIER 3: Specialized Workforce (8 Groups)

### Group 1: Product Discovery & Specification
- [@agents/research_agent.md](agents/research_agent.md) — Agent 01: Market & API Researcher
  - Skill: `.agent/skills/research_skill.md`
- [@agents/spec_agent.md](agents/spec_agent.md) — Agent 02: Master Spec Writer
  - Skill: `.agent/skills/spec_skill.md`

### Group 2: System Architecture & 7 Compulsory Documents
- [@agents/architecture_agent.md](agents/architecture_agent.md) — Agent 03: System Architect
  - Skill: `.agent/skills/architecture_skill.md`
- [@agents/document_agent.md](agents/document_agent.md) — Agent 03b: Documentarian Architect
  - Skill: `.agent/skills/document_skill.md`
  - **Note:** `documentarian_architect_skill.md` is RETIRED — this skill is canonical.

### Group 3: Full-Stack Construction (5 Parallel Workers)
- [@agents/builder_agent.md](agents/builder_agent.md) — Build Orchestrator (dispatches 5 workers)
  - Skill: `.agent/skills/build_skill.md`
- [@agents/frontend_builder.md](agents/frontend_builder.md) — Agent 05: Front-End UI/UX Builder
  - Skill: `.agent/skills/frontend_builder_skill.md`
  - **Note:** `build_frontend.md` is RETIRED — this skill is canonical.
- [@agents/backend_builder.md](agents/backend_builder.md) — Agent 06: Backend API Builder
  - Skill: `.agent/skills/backend_builder_skill.md`
  - **Note:** `build_backend.md` is RETIRED — this skill is canonical.
- [@agents/database_builder.md](agents/database_builder.md) — Agent 07: Database Architect
  - Skill: `.agent/skills/database_builder_skill.md`
- [@agents/security_guard.md](agents/security_guard.md) — Agent 08: Zero-Trust Security Guard
  - Skill: `.agent/skills/security_guard_skill.md`
- [@agents/github_saver.md](agents/github_saver.md) — Agent 09: Version Control Defender
  - Skill: `.agent/skills/github_saver_skill.md`

### Group 4: Quality Assurance & Defect Remediation
- [@agents/qa_agent.md](agents/qa_agent.md) — Agent 10: 3-Pillar QA & Testing Agent
  - Skill: `.agent/skills/qa_skill.md`
- [@agents/surgical_agent.md](agents/surgical_agent.md) — Agent 14: Precision Surgeon
  - Skill: `.agent/skills/surgical_skill.md`

### Group 5: Visual UX Refinement & Polish (Max 2 Loops)
- [@agents/polish_agent.md](agents/polish_agent.md) — Agent 13: UI/UX Enhancement Polisher
  - Skill: `.agent/skills/polish_skill.md`

### Group 6: Enterprise Audit, Security & Hardware Safety
- [@agents/auditor_agent.md](agents/auditor_agent.md) — Agent 17: Pre-Built Recovery Auditor
  - Skill: `.agent/skills/auditor_skill.md`
- [@agents/sec_ops_agent.md](agents/sec_ops_agent.md) — Agent 18: DevSecOps Vulnerability Scanner
  - Skill: `.agent/skills/sec_ops_skill.md`
- [@agents/hardware_compliance.md](agents/hardware_compliance.md) — Agent 19: Hardware Safety Specialist
  - Skill: `.agent/skills/hardware_compliance_skill.md`

### Group 7: Documentation, User Manual & Memory
- [@agents/memory_agent.md](agents/memory_agent.md) — Agent 15: Memory Keeper & Context Rot Prevention
  - Skill: `.agent/skills/memory_skill.md`
  - **Note:** `memory_keeper_skill.md` is RETIRED — this skill is canonical.
- [@agents/user_manual_writer.md](agents/user_manual_writer.md) — Agent 16: End-User Manual Writer
  - Skill: `.agent/skills/user_manual_writer_skill.md`

### Group 8: Production Release (STRICTLY MANUAL ONLY — NEVER AUTO)
- [@agents/deploy_agent.md](agents/deploy_agent.md) — Master Deployment Orchestrator
  - Skill: `.agent/skills/deploy_skill.md`
- [@agents/web_deploy_agent.md](agents/web_deploy_agent.md) — Web & Cloud Deployment Specialist
  - Skill: `.agent/skills/web_deploy_skill.md`
- [@agents/playstore_deploy_agent.md](agents/playstore_deploy_agent.md) — Google Play Store Specialist
  - Skill: `.agent/skills/playstore_deploy_skill.md`
- [@agents/appstore_deploy_agent.md](agents/appstore_deploy_agent.md) — Apple App Store Specialist
  - Skill: `.agent/skills/appstore_deploy_skill.md`
- [@agents/github_deploy_agent.md](agents/github_deploy_agent.md) — GitHub CI/CD Specialist
  - Skill: `.agent/skills/github_deploy_skill.md`

---

## CONSOLIDATION LOG (Retired Files)

The following files have been consolidated into their canonical counterparts:

| Retired File | Canonical File | Reason |
|-------------|----------------|--------|
| `build_backend.md` | `backend_builder_skill.md` | Duplicate coverage of same agent |
| `build_frontend.md` | `frontend_builder_skill.md` | Duplicate coverage of same agent |
| `memory_keeper_skill.md` | `memory_skill.md` | Subset functionality, now merged |
| `documentarian_architect_skill.md` | `document_skill.md` | Subset functionality, now merged |

Retired files contain a deprecation header directing users to the canonical skill.

---

## QUICK-REFERENCE: SLASH COMMAND MAP

| Command | Tier | Group | Dispatches |
|---------|------|-------|------------|
| `/start` | 1 | — | Team Leader initialization |
| `/end` | 1 | — | Graceful shutdown |
| `/research` | 1 | G1 | Researcher sub-agent |
| `/spec` | 1 | G1 | Spec Writer sub-agent |
| `/architecture` | 2 | G2 | Architecture Agent |
| `/document` | 1 | G2 | Documentarian Architect sub-agent |
| `/build-all` | 1 | G3 | 5 parallel worker sub-agents |
| `/qa-test` | 2 | G4 | QA Agent |
| `/polish` | 1 | G5 | Polisher sub-agent |
| `/surgical` | 1 | G4/G5/G6 | Surgeon sub-agent |
| `/sec-ops` | 2 | G6 | DevSecOps scanner |
| `/audit` | 1 | G6 | Auditor Recovery sub-agents |
| `/hardware-compliance` | 2 | G6 | Hardware Safety Agent |
| `/context-save` | 1 | G7 | Memory Keeper sub-agent |
| `/context-load` | 1 | G7 | Memory Restore sub-agent |
| `/deploy` | 1 | G8 ⚠️ MANUAL | Deployment Orchestrator |
| `/deploy-web` | — | G8 | Web Deployment Specialist |
| `/deploy-playstore` | — | G8 | Play Store Specialist |
| `/deploy-appstore` | — | G8 | App Store Specialist |
| `/deploy-github` | — | G8 | GitHub CI/CD Specialist |
