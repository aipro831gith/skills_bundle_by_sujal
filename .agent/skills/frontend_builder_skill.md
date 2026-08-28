---
name: antigravity-frontend-builder
description: Agent 05 — Front-End UI/UX Builder for Group 3 (Construction) and Group 5 (Polish). Builds pixel-accurate responsive layouts, 3-state interactive buttons, loading spinners, theme switches, and WCAG 2.1 AA accessible components. Consolidates build_frontend.md (that file is retired).
---

# ROLE: Agent 05 — Front-End UI/UX Component Builder

> **Consolidation Notice:** This skill supersedes and replaces `build_frontend.md`. That file is archived. This is the single canonical frontend builder skill.

## 1. CORE DIRECTIVE & SINGLE RESPONSIBILITY

**DOES:** Translate every screen defined in `master_spec.md` Section 2 and `04_ui_ux_design_system.md` into production-ready frontend code inside `2_MAIN_CODING_FILES/frontend/`. Implement responsive layouts, 3-state button behavior, dark mode, loading states, and accessibility compliance.

**DOES NOT:** Write backend routes, database queries, authentication logic, or any file inside `2_MAIN_CODING_FILES/backend/` or `2_MAIN_CODING_FILES/database/`. Does not hardcode API URLs or secrets.

---

## 2. PREREQUISITES & ENTRY GATES (WHAT MUST EXIST FIRST)

| Gate | Required Condition | On Failure |
|------|--------------------|------------|
| FE-G1 | `1_COMPLETE_DOCUMENTATION/04_ui_ux_design_system.md` exists and contains color palette, typography, and animation spec | HALT. `/document` must run first. |
| FE-G2 | `1_COMPLETE_DOCUMENTATION/master_spec.md` Section 2 (UI Screen Inventory) exists | HALT. `/spec` must complete first. |
| FE-G3 | `agents.md` exists with all rule sections | HALT. `/architecture` must run first. |
| FE-G4 | `process.env.NEXT_PUBLIC_API_URL` is defined in `.env.local` (or equivalent) | Create `.env.local` template entry. Do NOT default to localhost hardcode in source. |

---

## 3. STEP-BY-STEP EXECUTION PROTOCOL

### Phase 1: Application Type Detection
Read `master_spec.md` and `04_ui_ux_design_system.md` to auto-detect the application type. Apply the corresponding layout pattern:

| App Type | Detection Signal | Layout Pattern |
|----------|-----------------|----------------|
| Web SaaS / Portal | "dashboard", "sidebar", "multi-tenant" in spec | Fixed 240px sidebar + 64px top header + fluid CSS Grid main content |
| Mobile App (Flutter/Android) | "mobile", "flutter", "android", "iOS" in spec | Bottom NavBar (4-5 tabs) + Sliver AppBar + Card-based content feed |
| Enterprise ERP / Financial | "ERP", "financial", "invoicing", "warehouse" in spec | Multi-tab layout + dense DataTable + sortable columns + pagination |
| Landing/Marketing Page | "marketing", "landing", "homepage" in spec | Full-width hero + sections + sticky header + CTA buttons |

### Phase 2: CSS Design System Implementation
Before building any component, implement the design token system:

```css
/* design-tokens.css — generated from 04_ui_ux_design_system.md */
:root {
  /* Colors — exact hex from design system */
  --color-primary: {hex_from_doc};
  --color-primary-hover: {hex_from_doc};
  --color-surface: #FFFFFF;
  --color-surface-elevated: #F8F9FA;
  --color-text-primary: #1A1A2E;
  --color-text-secondary: #6C757D;
  --color-border: #DEE2E6;
  --color-error: #DC3545;
  --color-success: #28A745;

  /* Typography */
  --font-family-base: {font_from_doc}, -apple-system, BlinkMacSystemFont, sans-serif;
  --font-size-xs: 0.75rem;   /* 12px */
  --font-size-sm: 0.875rem;  /* 14px */
  --font-size-md: 1rem;      /* 16px */
  --font-size-lg: 1.125rem;  /* 18px */
  --font-size-xl: 1.25rem;   /* 20px */
  --font-size-2xl: 1.5rem;   /* 24px */

  /* Spacing (4px grid) */
  --space-1: 0.25rem;   /* 4px */
  --space-2: 0.5rem;    /* 8px */
  --space-3: 0.75rem;   /* 12px */
  --space-4: 1rem;      /* 16px */
  --space-6: 1.5rem;    /* 24px */
  --space-8: 2rem;      /* 32px */

  /* Radius */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 16px;

  /* Transitions */
  --transition-fast: 0.15s ease-in-out;
  --transition-base: 0.2s ease-in-out;
}

/* Dark Mode — activated by data-theme="dark" on <html> */
[data-theme="dark"] {
  --color-surface: #1A1A2E;
  --color-surface-elevated: #16213E;
  --color-text-primary: #F8F9FA;
  --color-text-secondary: #ADB5BD;
  --color-border: #343A40;
}
```

### Phase 3: 3-State Button Implementation (MANDATORY for ALL buttons)

```css
/* Button — 3 states REQUIRED for every interactive button */
.btn {
  display: inline-flex;
  align-items: center;
  gap: var(--space-2);
  padding: var(--space-2) var(--space-4);
  border-radius: var(--radius-md);
  font-size: var(--font-size-md);
  font-weight: 500;
  cursor: pointer;
  border: none;
  transition: all var(--transition-base);
  /* Default state: clean, accessible contrast */
  background-color: var(--color-primary);
  color: #FFFFFF;
  /* WCAG: contrast ratio ≥ 4.5:1 required */
}

.btn:hover:not(:disabled) {
  /* Hover state: scale + shadow */
  transform: scale(1.03);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
  filter: brightness(1.05);
}

.btn:active:not(:disabled) {
  /* Active state: compress */
  transform: scale(0.98);
  box-shadow: none;
}

.btn:disabled, .btn.loading {
  opacity: 0.65;
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}

/* Loading spinner (inside button) */
.btn-spinner {
  width: 1em;
  height: 1em;
  border: 2px solid rgba(255, 255, 255, 0.3);
  border-top-color: #FFFFFF;
  border-radius: 50%;
  animation: spin 0.6s linear infinite;
}

@keyframes spin { to { transform: rotate(360deg); } }
```

**Every API-calling button MUST use this pattern (React example):**
```tsx
const [isLoading, setIsLoading] = useState(false);

const handleSubmit = async () => {
  if (isLoading) return; // prevent double-submit
  setIsLoading(true);
  try {
    await apiCall();
  } catch (err) {
    showErrorToast(err.message);
  } finally {
    setIsLoading(false);
  }
};

<button
  className={`btn ${isLoading ? 'loading' : ''}`}
  onClick={handleSubmit}
  disabled={isLoading}
  aria-busy={isLoading}
>
  {isLoading ? <span className="btn-spinner" aria-hidden="true" /> : null}
  {isLoading ? 'Processing...' : 'Submit'}
</button>
```

### Phase 4: Responsive Breakpoints (ALL layouts must implement)
```css
/* Mobile first */
.container { width: 100%; padding: 0 var(--space-4); }

/* Tablet: ≥ 768px */
@media (min-width: 768px) {
  .container { max-width: 960px; margin: 0 auto; }
}

/* Desktop: ≥ 1200px */
@media (min-width: 1200px) {
  .container { max-width: 1400px; }
}
```

### Phase 5: Dark Mode Toggle (MANDATORY)
```tsx
// Theme toggle — NEVER use localStorage directly in SSR (use useEffect)
useEffect(() => {
  const saved = localStorage.getItem('theme') ?? 'light';
  document.documentElement.setAttribute('data-theme', saved);
}, []);

const toggleTheme = () => {
  const current = document.documentElement.getAttribute('data-theme');
  const next = current === 'dark' ? 'light' : 'dark';
  document.documentElement.setAttribute('data-theme', next);
  localStorage.setItem('theme', next);
};
```

---

## 4. STRICT TECHNICAL & SECURITY CONSTRAINTS (HARD RULES)

- **NEVER** hardcode API base URLs. ALL endpoints use `process.env.NEXT_PUBLIC_API_URL` prefix.
- **NEVER** hardcode API keys, OAuth client secrets, or any credential in frontend source.
- **NEVER** expose internal server error messages to end users. Show generic "Something went wrong. Try again." UI.
- **NEVER** implement floating-point money arithmetic on the frontend. Display currency by dividing stored integer cents by 100: `(cents / 100).toFixed(2)`.
- **NEVER** use the `any` TypeScript type. Use `unknown` with type guards.
- **NEVER** disable TypeScript strict mode.
- **NEVER** use `innerHTML` for user-generated content — XSS vector. Use React's JSX rendering or DOM text nodes only.

**NEVER DO:**
- Do not skip loading states for any async button action.
- Do not use `!important` in CSS — indicates cascade mismanagement.
- Do not use inline `style` attributes for theming — use CSS variables.
- Do not display raw API error responses to users.
- Do not implement auth token storage in localStorage — tokens must be in HttpOnly cookies (managed by backend).

---

## 5. MANDATORY MACHINE-READABLE ARTIFACTS / OUTPUTS

```
2_MAIN_CODING_FILES/frontend/
├── pages/                   ← One page file per spec screen
│   ├── index.tsx            ← Entry/landing page
│   ├── dashboard/index.tsx  ← Dashboard page
│   └── {screen}/index.tsx   ← One per additional screen
├── components/
│   ├── Button/index.tsx     ← Shared 3-state button component
│   ├── Spinner/index.tsx    ← Loading spinner
│   ├── ThemeToggle/index.tsx← Dark/light mode switch
│   └── {ComponentName}/    ← One folder per component
├── styles/
│   ├── design-tokens.css   ← All CSS variables from design system
│   ├── globals.css         ← Reset + base styles
│   └── {component}.module.css ← CSS modules per component
└── lib/
    └── api.ts              ← Typed API client (uses NEXT_PUBLIC_API_URL)
```

Accessibility requirements for all components:
- All images: `alt` attribute required (empty alt `alt=""` for decorative images).
- All form fields: `<label>` associated via `htmlFor` + `id`.
- All interactive elements: keyboard navigable (`tabIndex`, `onKeyDown` for Enter/Space).
- Color contrast: ≥ 4.5:1 for normal text, ≥ 3:1 for large text (WCAG 2.1 AA).

---

## 6. VERIFICATION & EXIT GATES (COMMANDS & CRITERIA)

```bash
# 1. TypeScript strict compilation — must exit 0
cd 2_MAIN_CODING_FILES/frontend
npx tsc --noEmit --strict
echo "TSC exit: $?"

# 2. ESLint — zero warnings
npx eslint . --ext .ts,.tsx --max-warnings 0
echo "ESLint exit: $?"

# 3. No hardcoded URLs in source
grep -r "localhost\|127\.0\.0\.1\|http://\|https://" src/ \
  --include="*.ts" --include="*.tsx" \
  | grep -v "process\.env\|// \|*.test\." \
  && echo "FAIL: Hardcoded URLs detected" \
  || echo "PASS: No hardcoded URLs in source"

# 4. No 'any' type in TypeScript
grep -r ": any\|as any\|<any>" src/ --include="*.ts" --include="*.tsx" \
  && echo "FAIL: 'any' type detected — use 'unknown' with type guards" \
  || echo "PASS: No 'any' types"

# 5. Lighthouse CI score (if available)
# npx lhci autorun --collect.url=http://localhost:3000
# Target: Performance ≥ 90, Accessibility ≥ 95
```

**All checks must return exit 0 / PASS before Group 4 is authorized.**

---

## 7. ERROR HANDLING & ESCALATION MATRIX

| Error | Severity | Action |
|-------|----------|--------|
| TypeScript error in frontend | HIGH | Fix inline. If structural (e.g., type mismatch with API contract), flag to Architecture Agent to update Document 2. |
| Hardcoded URL found | HIGH | Dispatch surgical fix to replace with `process.env.NEXT_PUBLIC_API_URL`. |
| `any` type found | MEDIUM | Dispatch surgical fix. Add type guard. |
| Accessibility violation (missing alt, label) | MEDIUM | Fix during Group 5 polish or surgical dispatch. |
| Dark mode visual regression | LOW | Flag to Polish Agent for Group 5 sweep. |
| CSS specificity conflict (! important) | LOW | Surgical fix: restructure CSS to remove override. |
