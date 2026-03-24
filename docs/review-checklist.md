# Code Review Checklist — PulseFabric Monitor

Use this checklist after any AI-generated or human code change. Every item must pass before merging.

---

## Implementation Order

- [ ] Data added to `backend/db.json` **before** any frontend code
- [ ] DTO interface added/updated in `core/models/api.models.ts`
- [ ] Service method added in `core/services/dashboard-api.service.ts`
- [ ] UI component consumes data through the service (never directly)

## TypeScript

- [ ] No `any` type — parameters, return types, variables, generics, assertions
- [ ] All data structures have typed interfaces
- [ ] No `console.log`, `console.warn`, or `console.error`
- [ ] No commented-out code blocks
- [ ] Every import is used — no orphan imports
- [ ] One component/service/model group per file
- [ ] Signals used for reactive state (`signal()`, `input()`, `computed()`)
- [ ] `inject()` used instead of constructor injection

## HTML Templates

- [ ] Flat structure — no unnecessary `<div>` wrappers
- [ ] No hardcoded data in templates
- [ ] No logic beyond simple bindings and control flow (`@if`, `@for`)
- [ ] Material components used where appropriate (`mat-card`, `mat-icon`, etc.)

## SCSS Styles

- [ ] All colors use tokens: `--mat-sys-*`, `--pf-*`, or SCSS variables
- [ ] No raw hex values (e.g., `#ff0000`)
- [ ] No raw px/rem values — use spacing tokens (`--pf-spacing-*`)
- [ ] Nesting depth ≤ 3 levels
- [ ] `:host { display: block; }` present on components

## File Organization

- [ ] TS, HTML, SCSS in separate files (no inline templates/styles)
- [ ] Files use `kebab-case` naming
- [ ] Classes use `PascalCase`, variables use `camelCase`
- [ ] Standalone component (no NgModule)

## Component Design

- [ ] Input-driven — data passed via `input()` signals
- [ ] No data fetching inside the component
- [ ] Reuses existing components where possible (KpiCard, ChartWidget, AlertList)
- [ ] Adding a new instance requires only different inputs, not a new file

## Material 3 Compliance

- [ ] Uses Material 3 components from `@angular/material`
- [ ] Follows M3 spacing and typography guidelines
- [ ] Uses system variables for theming (`--mat-sys-*`)
- [ ] Card components use `appearance="outlined"`

## Validation

- [ ] `ng build` passes with zero errors
- [ ] No new budget warnings (echarts warning is pre-existing and acceptable)
- [ ] App loads correctly in browser at `http://localhost:4200`
- [ ] New feature is visible and functional

## Constants & Configuration

- [ ] No magic numbers — all values in named constants or enums
- [ ] Chart colors use `PALETTE` from `chart-config.factory.ts`
- [ ] API base URL comes from `environment.ts` (not hardcoded)
