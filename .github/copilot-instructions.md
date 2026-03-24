# PulseFabric Monitor — Copilot Instructions

## Project Overview

PulseFabric Monitor is an enterprise infrastructure monitoring dashboard built with Angular 21, Angular Material (Material 3), and a json-server fake backend. The codebase lives under `/frontend` (Angular app) and `/backend` (db.json).

## Feature Implementation Order

When adding any new feature, **always** follow this sequence:

1. **db.json** — Add or update the data shape in `backend/db.json`
2. **Typed model** — Add or update the DTO interface in `frontend/src/app/core/models/api.models.ts`
3. **Service** — Add a method to `frontend/src/app/core/services/dashboard-api.service.ts`
4. **UI** — Build or extend the component, consuming data via the service

Never skip steps. Never put data fetching logic directly in a component.

## All Decisions Must Be Documented

Before making any change, write a brief rationale in the commit message or PR description. If a decision involves trade-offs, document it explicitly.

---

## Twenty Production Code Rules

Every line of code generated or modified **must** comply with all twenty rules below. Violations are not acceptable.

### 1. Separation of Concerns
TS, HTML, and SCSS must be in separate files. Frontend and backend are separate directories.

### 2. No Hardcoded Data
All data must come from mock files or API services. Never embed data literals in templates or component classes.

### 3. Typed Interfaces for All Data
Every data structure must have a TypeScript interface. The `any` type is **strictly forbidden** everywhere—function parameters, return types, variables, generics.

### 4. Component-Driven Architecture
All UI must be composed of reusable, standalone Angular components. No logic in `app.html` beyond a single root component tag.

### 5. No Magic Numbers or Strings
All values must be stored in named constants, enums, or configuration objects. No unexplained literals in code.

### 6. CSS Token System
All colors, spacing, border-radius, and typography must use Material 3 system variables (`--mat-sys-*`), project tokens (`--pf-*`), or SCSS variables. Never write raw hex, px, or rem values directly in component SCSS.

### 7. Material 3 Compliance
Follow Angular Material / Material 3 guidelines for spacing, typography, color, and component usage. Use `mat-card`, `mat-icon`, etc. from `@angular/material`.

### 8. Concise, Idiomatic Angular
Use signals, `input()`, `inject()`, pipes, and directives. Avoid verbose boilerplate. Prefer declarative patterns over imperative ones.

### 9. No Over-Engineering
Do not add state management libraries, unit test frameworks, authentication, or logging infrastructure unless explicitly requested. Keep it minimal.

### 10. Reuse Before Creating
Before creating any new component, service, or utility, audit existing code for reusable pieces. If an existing component can be extended via inputs, prefer that over creating a new one.

### 11. Generic Component Design
Components like KPI cards, chart widgets, and alert items must be input-driven. Adding a new instance of the same type should require only a single template reference with different inputs—no new component file.

### 12. Minimal Change Principle
If adding a feature requires modifying more than 3 existing files, **stop and explain** the scope before proceeding. Get confirmation before making broad changes.

### 13. Flat HTML Structure
No unnecessary DOM wrappers. Keep HTML structure as flat and semantic as possible. Every element must serve a layout or semantic purpose.

### 14. No console.log
Remove all `console.log`, `console.warn`, `console.error` statements before committing. Use proper error handling patterns instead.

### 15. No Dead Code
No commented-out code blocks. If code is not needed, delete it. Version control preserves history.

### 16. Clean Imports
Every import must be used. No orphan imports. Organize imports logically (Angular core → Angular Material → third-party → project).

### 17. Consistent Naming Conventions
- Files: `kebab-case` (e.g., `kpi-card.ts`)
- Variables/functions: `camelCase`
- Classes/interfaces: `PascalCase`
- CSS classes: `kebab-case`

### 18. Single Responsibility per File
Each file does exactly one thing. One component per file, one service per file, one model group per file.

### 19. No `any` Type (Reinforced)
This rule is critical enough to state twice. `any` is forbidden in all positions: parameters, return types, generics, type assertions. Use `unknown` with type guards if needed.

### 20. SCSS Nesting ≤ 3 Levels
SCSS selectors must not nest deeper than 3 levels. Refactor into separate classes if nesting would exceed this limit.

---

## Project Structure Reference

```
backend/
  db.json                         ← json-server database

frontend/src/app/
  core/
    models/api.models.ts          ← All typed DTOs
    services/dashboard-api.service.ts ← API abstraction layer
    mocks/                        ← Legacy mock files (being phased out)
  dashboard/
    dashboard.ts / .html / .scss  ← Main dashboard page
    kpi-card/                     ← Generic KPI card (input-driven)
    chart-widget/                 ← Generic chart widget (input-driven)
    alert-list/                   ← Alert list component
    chart-config.factory.ts       ← Pure functions generating ECharts options
  shared/styles/
    _tokens.scss                  ← Design tokens (spacing, radius, etc.)
  environments/
    environment.ts                ← Dev config (apiBaseUrl)
    environment.prod.ts           ← Prod config
```

## Key Patterns

- **Chart addition**: Add factory function in `chart-config.factory.ts` → reference `<app-chart-widget>` in template with `[options]`
- **KPI addition**: Add entry to `db.json` kpis array → `<app-kpi-card>` renders automatically via `@for`
- **Alert addition**: Add entry to `db.json` alerts array → renders automatically
- **API base URL**: Managed via `environment.ts` + Angular dev proxy (`proxy.conf.json`)
