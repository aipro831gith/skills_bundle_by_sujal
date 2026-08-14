---
name: antigravity-memory
description: Deploys Memory Keeper Sub-Agent for context save/load, and Documentarian Sub-Agent for auto-generating USER_MANUAL.md upon project completion.
---
# Antigravity Enterprise Ecosystem: Memory & Auto-Documentation Skill

This skill governs **Agent 15 (Memory Keeper)** and **Agent 16 (Documentarian User-Manual Writer)**.

## 1. Memory Keeper Sub-Agent (`/context-save` and `/context-load`)

### Step 1: Sub-Agent Dispatch for `/context-save`
When the Boss types `/context-save`, execute `invoke_subagent`:

```json
{
  "Subagents": [
    {
      "TypeName": "memory-keeper-save",
      "Role": "Context Snapshot Memory Keeper Sub-Agent",
      "Prompt": "Take a complete textual picture of the office space. Compile all uploaded user documents, chat text, AI responses, code files, plans, tasks.md, and diaries into context_snapshot.md inside 3_PROJECT_BACKUP_AND_DIARY/. Log action in diary_1_audit_log.md."
    }
  ]
}
```

### Step 2: Sub-Agent Dispatch for `/context-load`
When the Boss types `/context-load`, execute `invoke_subagent`:

```json
{
  "Subagents": [
    {
      "TypeName": "memory-keeper-load",
      "Role": "Context Restore Memory Keeper Sub-Agent",
      "Prompt": "Read context_snapshot.md in folder 3. Silently restore all project context, diaries, active file registries, and task matrices into memory."
    }
  ]
}
```

---

## 2. Documentarian User-Manual Writer Sub-Agent (Auto-Completion)

When the software achieves 100% completion (after `/surgical`), execute `invoke_subagent`:

```json
{
  "Subagents": [
    {
      "TypeName": "user-manual-writer",
      "Role": "End-User Documentation Sub-Agent",
      "Prompt": "Read master_spec.md and 2_MAIN_CODING_FILES/. Generate a plain-English USER_MANUAL.md in the root directory for end-users (customers). Include login steps, core feature walkthroughs, button explanations, and user troubleshooting guides."
    }
  ]
}
```

### Salesman Hand-off
Tell the Boss:
*"Boss, our Documentarian Sub-Agent has automatically generated the `USER_MANUAL.md` for your end-users and customers! Your enterprise application is completely finished, documented, and ready for deployment."*
