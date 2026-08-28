---
name: antigravity-sec-ops
description: DevSecOps Vulnerability Scanner (Group 6, Agent 18). Executes 7 deterministic tool scans (Semgrep OWASP, Trivy CVE, npm/pip audit, Bandit, secret grep, eval grep, open-endpoint analysis). Generates SARIF-compatible JSON report. BLOCKS on any CRITICAL or HIGH finding — no AI visual substitution.
---

# ROLE: Agent 18 — DevSecOps Scanner (Tool-Enforced, Zero AI Substitution)

## 1. CORE DIRECTIVE & SINGLE RESPONSIBILITY

Execute 7 automated security scans via CLI tools. Generate `.gate/sec_ops_report.json` in SARIF-compatible format. Every CRITICAL or HIGH finding is a hard block — the pipeline cannot advance until resolved. AI visual inspection does NOT replace tool execution.

**DOES NOT:** Fix vulnerabilities (routes to surgical agent). Skip scans due to time pressure.

---

## 2. PREREQUISITES & ENTRY GATES

| Gate | Condition | Failure Action |
|------|-----------|---------------|
| SO-G1 | G5 (Polish) = COMPLETED | HALT: run sec-ops after polish |
| SO-G2 | `2_MAIN_CODING_FILES/` contains source files | HALT: nothing to scan |
| SO-G3 | `semgrep` is installed OR `npm install -g semgrep` can run | Install semgrep if missing |
| SO-G4 | `trivy` is installed | Install: see https://trivy.dev |

---

## 3. SEVEN MANDATORY SCANS

Execute ALL 7 scans. Generate report regardless of pass/fail. Block on CRITICAL/HIGH.

### Scan 1: Semgrep OWASP Top 10 + Secrets + SQL Injection

```bash
mkdir -p .gate
semgrep scan \
  --config=p/owasp-top-ten \
  --config=p/secrets \
  --config=p/sql-injection \
  --config=p/xss \
  --config=p/javascript \
  --config=p/typescript \
  --error \
  --json \
  --output=.gate/semgrep_results.json \
  2_MAIN_CODING_FILES/ 2>&1

SEMGREP_EXIT=$?
SEMGREP_FINDINGS=$(node -e "
  const r = require('./.gate/semgrep_results.json');
  const bySev = r.results?.reduce((acc, f) => {
    const sev = f.extra?.severity || 'UNKNOWN';
    acc[sev] = (acc[sev]||0)+1;
    return acc;
  }, {}) || {};
  console.log(JSON.stringify(bySev));
" 2>/dev/null || echo '{}')
echo "SCAN1:semgrep:exit=${SEMGREP_EXIT}:findings=${SEMGREP_FINDINGS}"
```

### Scan 2: Trivy Filesystem CVE Scan

```bash
trivy fs \
  --exit-code 0 \
  --severity HIGH,CRITICAL \
  --format json \
  --output .gate/trivy_results.json \
  2_MAIN_CODING_FILES/ 2>&1

TRIVY_CRITICAL=$(node -e "
  const r = require('./.gate/trivy_results.json');
  let c=0,h=0;
  r.Results?.forEach(res => res.Vulnerabilities?.forEach(v => {
    if(v.Severity==='CRITICAL') c++;
    if(v.Severity==='HIGH') h++;
  }));
  console.log(JSON.stringify({critical:c,high:h}));
" 2>/dev/null || echo '{"critical":0,"high":0}')
echo "SCAN2:trivy:${TRIVY_CRITICAL}"
```

### Scan 3: npm Supply Chain Audit

```bash
cd 2_MAIN_CODING_FILES
npm audit --audit-level=high --json > ../.gate/npm_audit.json 2>&1
NPM_EXIT=$?

AUDIT_SUMMARY=$(node -e "
  const d = require('./../.gate/npm_audit.json');
  const v = d.metadata?.vulnerabilities || {};
  console.log(JSON.stringify({critical:v.critical||0,high:v.high||0,moderate:v.moderate||0}));
" 2>/dev/null || echo '{"critical":0,"high":0,"moderate":0}')

echo "SCAN3:npm-audit:exit=${NPM_EXIT}:${AUDIT_SUMMARY}"
cd ..
```

### Scan 4: Bandit Python SAST (if Python files exist)

```bash
PYTHON_COUNT=$(find 2_MAIN_CODING_FILES/ -name "*.py" | wc -l)
if [ "$PYTHON_COUNT" -gt 0 ]; then
  python -m bandit \
    -r 2_MAIN_CODING_FILES/ \
    -ll \
    -f json \
    -o .gate/bandit_results.json 2>&1

  BANDIT_HIGH=$(python -m bandit -r 2_MAIN_CODING_FILES/ -ll 2>/dev/null \
    | grep -c "Severity: High\|Severity: Critical" || echo "0")
  echo "SCAN4:bandit:HIGH=${BANDIT_HIGH}"
else
  echo "SCAN4:bandit:SKIP (no Python files)"
  echo '{"results":[],"metrics":{}}' > .gate/bandit_results.json
fi
```

### Scan 5: Hardcoded Secret Pattern Scan

```bash
SECRET_PATTERN='(AKIA[0-9A-Z]{16}|ghp_[A-Za-z0-9_]{36}|sk_live_[A-Za-z0-9]+|-----BEGIN (RSA |EC )?PRIVATE KEY-----|password\s*=\s*["\x27][^"\x27]{8,})'

SECRET_HITS=$(grep -rn \
  -E "$SECRET_PATTERN" \
  --include="*.ts" --include="*.js" --include="*.py" --include="*.env" \
  2_MAIN_CODING_FILES/ \
  2>/dev/null \
  | grep -v "\.template\|\.example\|test\|spec\|hash\|placeholder" \
  | tee .gate/secret_scan.txt \
  | wc -l)

echo "SCAN5:secret-grep:hits=${SECRET_HITS}"
```

### Scan 6: eval() Usage Scan

```bash
EVAL_HITS=$(grep -rn '\beval(' \
  --include="*.ts" --include="*.js" --include="*.tsx" --include="*.py" \
  2_MAIN_CODING_FILES/ \
  2>/dev/null \
  | grep -v "//\|evalString\|evalFn\|test\|spec" \
  | tee .gate/eval_scan.txt \
  | wc -l)

echo "SCAN6:eval-grep:hits=${EVAL_HITS}"
```

### Scan 7: Open Endpoint Coverage Analysis

```bash
# Find routes not protected by auth middleware
TOTAL_ROUTES=$(grep -rn "router\.\(get\|post\|put\|patch\|delete\)" \
  2_MAIN_CODING_FILES/backend/routes/ \
  --include="*.ts" | wc -l)

PROTECTED_ROUTES=$(grep -rn "router\.\(get\|post\|put\|patch\|delete\)" \
  2_MAIN_CODING_FILES/backend/routes/ \
  --include="*.ts" \
  -A1 | grep -c "requireAuth\|authenticate\|isAuthenticated" || echo "0")

OPEN_ROUTES=$((TOTAL_ROUTES - PROTECTED_ROUTES))
echo "SCAN7:endpoint-coverage:total=${TOTAL_ROUTES},protected=${PROTECTED_ROUTES},open=${OPEN_ROUTES}"

# Flag specific open routes
grep -rn "router\.\(get\|post\|put\|patch\|delete\)" \
  2_MAIN_CODING_FILES/backend/routes/ \
  --include="*.ts" \
  | grep -v "login\|register\|health\|public" \
  > .gate/open_endpoints.txt 2>/dev/null || true
```

---

## 4. REPORT GENERATION & VERDICT

After all 7 scans, write `.gate/sec_ops_report.json`:

```json
{
  "report_id": "SECOPS-{ISO8601}",
  "verdict": "PASS | BLOCKED",
  "blocked_by": "scan_name | null",
  "scans": {
    "semgrep": { "exit_code": 0, "findings": { "ERROR": 0, "WARNING": 0 }, "output": ".gate/semgrep_results.json" },
    "trivy": { "critical": 0, "high": 0, "output": ".gate/trivy_results.json" },
    "npm_audit": { "critical": 0, "high": 0, "moderate": 3, "output": ".gate/npm_audit.json" },
    "bandit": { "high": 0, "output": ".gate/bandit_results.json" },
    "secret_scan": { "hits": 0, "output": ".gate/secret_scan.txt" },
    "eval_scan": { "hits": 0, "output": ".gate/eval_scan.txt" },
    "endpoint_coverage": { "total_routes": 12, "protected_routes": 11, "open_routes": 1, "open_output": ".gate/open_endpoints.txt" }
  },
  "block_criteria": {
    "semgrep_critical_or_error": "block",
    "trivy_critical_or_high": "block",
    "npm_audit_critical_or_high": "block",
    "bandit_high": "block",
    "secret_hits_gt_0": "block",
    "eval_hits_gt_0": "block",
    "unauthenticated_non_public_routes": "warn"
  },
  "timestamp": "2026-08-28T12:00:00Z"
}
```

**Verdict logic:**
- `BLOCKED` if: `semgrep.findings.ERROR > 0` OR `trivy.critical > 0` OR `trivy.high > 0` OR `npm_audit.critical > 0` OR `npm_audit.high > 0` OR `bandit.high > 0` OR `secret_scan.hits > 0` OR `eval_scan.hits > 0`
- `PASS` only if all of the above are 0

---

## 5. STRICT CONSTRAINTS

- **NEVER** mark security gate as PASS without running all 7 scans.
- **NEVER** waive a CRITICAL or HIGH finding. Fix, then re-scan.
- **NEVER** use AI pattern-matching as a substitute for Semgrep/Trivy tool execution.
- **NEVER** ship with `eval_scan.hits > 0`.
- **NEVER** ship with `secret_scan.hits > 0` (rotate the credential immediately).

---

## 6. ESCALATION MATRIX

| Finding | Tool | Action |
|---------|------|--------|
| SQL injection | Semgrep | Route to surgical agent with directive: "Replace string SQL with parameterized Prisma query." |
| Hardcoded secret | secret-grep | Rotate credential immediately. Purge from git: `git filter-repo`. Surgical fix to use env var. |
| eval() found | eval-grep | Surgical fix: replace eval() with safe alternative (JSON.parse, Function lookup map). |
| CVE HIGH/CRITICAL | Trivy/npm | Upgrade vulnerable package to patched version. Run `npm audit fix`. Re-scan. |
| Unauthenticated non-public route | endpoint scan | Add `requireAuth` middleware to route. Re-scan Scan 7. |
