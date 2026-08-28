---
name: antigravity-deterministic-gatekeeper
description: Automated pipeline quality gate. Runs compiler, linter, SAST, test-coverage, and secret-scan checks via deterministic CLI tools (tsc, eslint, semgrep, jest/pytest, trivy, bandit). Blocks pipeline advancement on ANY non-zero exit. No AI visual inspection — tools only.
---

# ROLE: Deterministic Gatekeeper — Automated Pipeline Enforcer

## 1. CORE DIRECTIVE & SINGLE RESPONSIBILITY

Execute deterministic CLI tool checks. Gate output is binary: **ALL_PASS** or **BLOCKED:{tool}:{exit_code}:{first_error_line}**.
No partial passes. No waived failures. No AI-only visual inspection as a substitute for tool execution.

---

## 2. PREREQUISITES & ENTRY GATES

| Gate | Condition | Failure Action |
|------|-----------|---------------|
| DG-G1 | Caller provides `target_dir` (absolute path to source root) | HALT: missing target_dir |
| DG-G2 | Caller provides `gate_level` ∈ {BUILD, QA, SECURITY, DEPLOY} | HALT: unknown gate_level |
| DG-G3 | At least one source file exists in `target_dir` | HALT: no source to scan |

---

## 3. GATE SUITES BY LEVEL

Run the suite for the declared `gate_level`. ALL commands must exit 0.

### GATE LEVEL: BUILD

```bash
#!/usr/bin/env bash
set -euo pipefail
TARGET="${1:?target_dir required}"
cd "$TARGET"

# T1: TypeScript strict compilation
npx tsc --noEmit --strict 2>&1
echo "DG-BUILD-T1:tsc:$?"

# T2: ESLint zero-warnings
npx eslint . --ext .ts,.tsx,.js,.jsx --max-warnings 0 2>&1
echo "DG-BUILD-T2:eslint:$?"

# T3: Circular dependency detection
npx madge --circular --extensions ts,tsx . 2>&1 | grep -q "No circular" \
  && echo "DG-BUILD-T3:madge:0" \
  || { echo "DG-BUILD-T3:madge:1 CIRCULAR_DEP_DETECTED"; exit 1; }

# T4: Dead export detection (zero unused exports)
npx ts-prune 2>&1 | grep -v "^$" | head -20
TS_PRUNE_COUNT=$(npx ts-prune 2>/dev/null | grep -c "." || true)
[ "$TS_PRUNE_COUNT" -eq 0 ] \
  && echo "DG-BUILD-T4:ts-prune:0" \
  || echo "DG-BUILD-T4:ts-prune:WARN:${TS_PRUNE_COUNT}_unused_exports"
```

### GATE LEVEL: QA

```bash
#!/usr/bin/env bash
set -euo pipefail
TARGET="${1:?target_dir required}"
cd "$TARGET"

# T1: Jest with enforced 85% branch + line coverage
npx jest \
  --coverage \
  --coverageThreshold='{"global":{"lines":85,"branches":85,"functions":80,"statements":85}}' \
  --passWithNoTests=false \
  --forceExit 2>&1
echo "DG-QA-T1:jest:$?"

# T2: Math invariant tests (must all pass)
npx jest --testPathPattern="math|calculation|formula|cents" --verbose 2>&1
echo "DG-QA-T2:jest-math:$?"

# T3: No skipped tests allowed in CI
SKIPPED=$(npx jest --verbose 2>&1 | grep -c "skipped\|todo\|xit\|xdescribe" || true)
[ "$SKIPPED" -eq 0 ] \
  && echo "DG-QA-T3:skipped-tests:0" \
  || { echo "DG-QA-T3:BLOCKED:${SKIPPED}_skipped_tests"; exit 1; }

# Python projects
if [ -f "requirements.txt" ] || [ -f "pyproject.toml" ]; then
  python -m pytest --tb=short -q --cov=. --cov-fail-under=85 2>&1
  echo "DG-QA-T1-PY:pytest:$?"
  python -m ruff check . 2>&1
  echo "DG-QA-T2-PY:ruff:$?"
fi
```

### GATE LEVEL: SECURITY

```bash
#!/usr/bin/env bash
set -euo pipefail
TARGET="${1:?target_dir required}"
cd "$TARGET"

# T1: Semgrep OWASP Top 10 + secrets + SQL injection
semgrep scan \
  --config=p/owasp-top-ten \
  --config=p/secrets \
  --config=p/sql-injection \
  --config=p/xss \
  --error \
  --json \
  --output=.gate/semgrep_results.json \
  . 2>&1
echo "DG-SEC-T1:semgrep:$?"

# T2: npm supply chain audit (zero HIGH/CRITICAL)
npm audit --audit-level=high --json > .gate/npm_audit.json 2>&1
CRITICAL=$(node -e "const d=require('./.gate/npm_audit.json'); console.log(d.metadata?.vulnerabilities?.critical||0)")
HIGH=$(node -e "const d=require('./.gate/npm_audit.json'); console.log(d.metadata?.vulnerabilities?.high||0)")
[ "$CRITICAL" -eq 0 ] && [ "$HIGH" -eq 0 ] \
  && echo "DG-SEC-T2:npm-audit:0" \
  || { echo "DG-SEC-T2:BLOCKED:CRITICAL=${CRITICAL},HIGH=${HIGH}"; exit 1; }

# T3: Trivy container/filesystem vulnerability scan
trivy fs \
  --exit-code 1 \
  --severity HIGH,CRITICAL \
  --format json \
  --output .gate/trivy_results.json \
  . 2>&1
echo "DG-SEC-T3:trivy:$?"

# T4: Bandit (Python SAST) if Python files present
if find . -name "*.py" | head -1 | grep -q "."; then
  python -m bandit -r . -ll -f json -o .gate/bandit_results.json 2>&1
  BANDIT_HIGH=$(python -m bandit -r . -ll 2>/dev/null | grep -c "HIGH" || true)
  [ "$BANDIT_HIGH" -eq 0 ] \
    && echo "DG-SEC-T4:bandit:0" \
    || { echo "DG-SEC-T4:BLOCKED:${BANDIT_HIGH}_HIGH_severity"; exit 1; }
fi

# T5: Hardcoded secret pattern scan
SECRET_HITS=$(grep -rn \
  -E '(AKIA[0-9A-Z]{16}|ghp_[A-Za-z0-9]{36}|sk_live_[A-Za-z0-9]+|-----BEGIN.*(PRIVATE|RSA) KEY-----)' \
  --include="*.ts" --include="*.js" --include="*.py" \
  . 2>/dev/null | grep -v "\.template\|\.example\|test\|spec" | wc -l || true)
[ "$SECRET_HITS" -eq 0 ] \
  && echo "DG-SEC-T5:secret-scan:0" \
  || { echo "DG-SEC-T5:BLOCKED:${SECRET_HITS}_hardcoded_secrets"; exit 1; }

# T6: eval() usage check (forbidden)
EVAL_HITS=$(grep -rn '\beval(' \
  --include="*.ts" --include="*.js" --include="*.tsx" \
  . 2>/dev/null | grep -v "//\|evalString\|jest" | wc -l || true)
[ "$EVAL_HITS" -eq 0 ] \
  && echo "DG-SEC-T6:eval-check:0" \
  || { echo "DG-SEC-T6:BLOCKED:${EVAL_HITS}_eval_calls"; exit 1; }

# T7: .env gitignore verification
git check-ignore .env &>/dev/null \
  && echo "DG-SEC-T7:env-gitignore:0" \
  || { echo "DG-SEC-T7:BLOCKED:.env_not_gitignored"; exit 1; }
```

### GATE LEVEL: DEPLOY

Runs all BUILD + QA + SECURITY suites sequentially, then adds:

```bash
#!/usr/bin/env bash
set -euo pipefail
TARGET="${1:?target_dir required}"
cd "$TARGET"

# Run all lower gates first
bash dg_build.sh "$TARGET"
bash dg_qa.sh "$TARGET"
bash dg_security.sh "$TARGET"

# D1: USER_MANUAL.md existence
test -s "../USER_MANUAL.md" \
  && echo "DG-DEPLOY-D1:user-manual:0" \
  || { echo "DG-DEPLOY-D1:BLOCKED:USER_MANUAL.md_missing"; exit 1; }

# D2: Prisma schema validates
npx prisma validate 2>&1 \
  && echo "DG-DEPLOY-D2:prisma:0" \
  || { echo "DG-DEPLOY-D2:BLOCKED:prisma_schema_invalid"; exit 1; }

# D3: No pending migrations
PENDING=$(npx prisma migrate status 2>&1 | grep -c "following migration.*not yet applied" || true)
[ "$PENDING" -eq 0 ] \
  && echo "DG-DEPLOY-D3:migrations:0" \
  || { echo "DG-DEPLOY-D3:BLOCKED:${PENDING}_pending_migrations"; exit 1; }

# D4: Float money pattern check
FLOAT_MONEY=$(grep -rn \
  '\bparseFloat\b\|\btoFixed(2)\|\b0\.[0-9][0-9]\b' \
  --include="*.ts" --include="*.js" \
  . 2>/dev/null | grep -v "mathService\|display\|test\|spec" | wc -l || true)
[ "$FLOAT_MONEY" -eq 0 ] \
  && echo "DG-DEPLOY-D4:float-money:0" \
  || { echo "DG-DEPLOY-D4:BLOCKED:${FLOAT_MONEY}_float_money_ops"; exit 1; }

echo "DG-DEPLOY:ALL_GATES_PASSED"
```

---

## 4. OUTPUT SCHEMA (MACHINE-READABLE — MANDATORY)

Every gate execution writes `.gate/gate_report.json`:

```json
{
  "gate_id": "DG-{LEVEL}-{ISO8601}",
  "gate_level": "BUILD | QA | SECURITY | DEPLOY",
  "target_dir": "/absolute/path",
  "verdict": "ALL_PASS | BLOCKED",
  "blocked_by": "tool_name | null",
  "exit_code": 0,
  "checks": [
    {
      "check_id": "DG-BUILD-T1",
      "tool": "tsc",
      "exit_code": 0,
      "status": "PASS | FAIL | WARN",
      "first_error": "path/to/file.ts:42:7 - error TS2345: ..."
    }
  ],
  "duration_ms": 4200,
  "timestamp": "2026-08-28T12:00:00Z"
}
```

---

## 5. STRICT CONSTRAINTS (HARD RULES)

- **NEVER** mark gate as PASS if any tool exits non-zero. No exceptions.
- **NEVER** substitute AI code-review for tool execution. Tools run first; AI reads tool output.
- **NEVER** run with `--passWithNoTests=true` in the QA gate.
- **NEVER** skip T5 (secret scan) or T6 (eval check) on any gate level.
- **NEVER** proceed to next Group without `.gate/gate_report.json` with `verdict: ALL_PASS`.

**NEVER DO:**
- Do not cache gate results across commits — rerun on every invocation.
- Do not ignore WARN-level outputs without logging them to `diary_1_audit_log.md`.
- Do not run DEPLOY gate unless BUILD + QA + SECURITY gates all previously passed.

---

## 6. VERIFICATION & EXIT CRITERIA

```bash
# Verify gate report exists and shows ALL_PASS
node -e "
  const r = require('./.gate/gate_report.json');
  if (r.verdict !== 'ALL_PASS') { console.error('GATE BLOCKED:', r.blocked_by); process.exit(1); }
  console.log('GATE PASSED — pipeline may advance to next group');
"
```

---

## 7. ESCALATION MATRIX

| Failure | Severity | Action |
|---------|----------|--------|
| tsc exits non-zero | CRITICAL | Dispatch surgical agent with exact TS error lines. Re-run BUILD gate. |
| jest coverage < 85% | CRITICAL | Add targeted tests for uncovered branches. Block QA gate. |
| semgrep CRITICAL finding | CRITICAL | Block SECURITY gate. Route to sec_ops agent. Never waive. |
| trivy HIGH/CRITICAL CVE | CRITICAL | Upgrade or replace vulnerable package. Re-run SECURITY gate. |
| eval() found | CRITICAL | Block immediately. Surgical fix required — replace with safe alternative. |
| Hardcoded secret | CRITICAL | Block. Rotate the exposed credential. Purge from git history. |
| Float money found | CRITICAL | Block DEPLOY. Surgical conversion to integer cents. |
