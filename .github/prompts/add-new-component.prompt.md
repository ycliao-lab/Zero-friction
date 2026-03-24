---
mode: agent
description: "Add a new standalone component to the PulseFabric Monitor dashboard"
---

# Demo: Add a Brand New Widget

Use this prompt during live demo. Copy and paste the sentence below into Copilot Chat:

> Add a new AI Security Recommendations widget to the dashboard. It should show 3 recommendations, each with a priority level (High, Medium, Low) and a short description. Add the data to db.json first, then build the component. Follow all repo rules in copilot-instructions.md.

---

## Task: Add a New Component

Before creating anything, answer these questions:

1. **Can an existing component handle this?** Check `kpi-card`, `chart-widget`, and `alert-list` first. If the new feature fits one of these patterns, extend via inputs — do not create a new component.
2. **Will this modify more than 3 existing files?** If yes, stop and explain the scope before proceeding.

If a new component is genuinely needed, follow the steps below.

### Step 1 — db.json
Add the backing data to `backend/db.json` as a new collection.

### Step 2 — Typed Model
Add the DTO interface to `frontend/src/app/core/models/api.models.ts`.

### Step 3 — Service Method
Add a method to `frontend/src/app/core/services/dashboard-api.service.ts` returning `Observable<YourDto[]>`.

### Step 4 — Create Component
Create under `frontend/src/app/dashboard/<component-name>/`:
```
<component-name>.ts
<component-name>.html
<component-name>.scss
```

Requirements:
- **Standalone component** — no NgModule.
- **TS / HTML / SCSS in separate files** — no inline templates or styles.
- **Input-driven** — accept data via `input()` signals, not internal fetching.
- **Use Material 3 tokens** — `--mat-sys-*` and `--pf-*` for all visual values.
- **Flat HTML** — no unnecessary `<div>` wrappers.
- **SCSS nesting ≤ 3 levels**.
- **No `any` type anywhere**.

### Step 5 — Wire into Dashboard
In `dashboard.ts`:
- Import the new component, add to `imports` array.
- Fetch data via the service; store in a signal.

In `dashboard.html`:
- Add the component tag with `[input]` bindings.

### Step 6 — Validation
- `ng build` must pass.
- No `console.log`, no dead code, no unused imports.
- Naming: file = `kebab-case`, class = `PascalCase`, variables = `camelCase`.
