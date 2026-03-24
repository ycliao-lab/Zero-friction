# AI Working Rules — PulseFabric Monitor

This document defines the rules and conventions that AI assistants (GitHub Copilot, agents, etc.) must follow when working on this project.

## Feature Implementation Order

All features must be implemented in this strict sequence:

1. **db.json** — Add or update data in `backend/db.json`
2. **Typed model** — Add or update the DTO interface in `frontend/src/app/core/models/api.models.ts`
3. **Service** — Add a method to `frontend/src/app/core/services/dashboard-api.service.ts`
4. **UI** — Build or extend the component, consuming data via the service

Never skip steps. Never put data fetching logic directly in a component.

## 20 Production Code Rules

| # | Rule | Summary |
|---|------|---------|
| 1 | Separation of Concerns | TS, HTML, SCSS in separate files |
| 2 | No Hardcoded Data | All data from API or mock files |
| 3 | Typed Interfaces | Every data structure has a TS interface; `any` is forbidden |
| 4 | Component-Driven | Reusable standalone Angular components |
| 5 | No Magic Numbers | Named constants, enums, or config objects only |
| 6 | CSS Token System | Use `--mat-sys-*`, `--pf-*`, or SCSS variables |
| 7 | Material 3 Compliance | Follow M3 guidelines for all UI |
| 8 | Concise Angular | Signals, `input()`, `inject()`, pipes, directives |
| 9 | No Over-Engineering | No unnecessary libraries or infrastructure |
| 10 | Reuse Before Creating | Audit existing code first |
| 11 | Generic Component Design | Input-driven; new instance = new inputs, not new file |
| 12 | Minimal Change Principle | >3 files changed → stop and explain |
| 13 | Flat HTML Structure | No unnecessary DOM wrappers |
| 14 | No console.log | Remove all logging before committing |
| 15 | No Dead Code | No commented-out blocks |
| 16 | Clean Imports | Every import must be used |
| 17 | Consistent Naming | kebab-case files, camelCase vars, PascalCase classes |
| 18 | Single Responsibility | One component/service/model group per file |
| 19 | No `any` (Reinforced) | Forbidden in all positions |
| 20 | SCSS Nesting ≤ 3 Levels | Refactor into separate classes if exceeded |

## Governance Files

| File | Purpose |
|------|---------|
| `.github/copilot-instructions.md` | Full rule set loaded into every Copilot session |
| `.github/prompts/add-kpi-widget.prompt.md` | Step-by-step prompt for adding a KPI card |
| `.github/prompts/add-chart-widget.prompt.md` | Step-by-step prompt for adding a chart widget |
| `.github/prompts/add-new-component.prompt.md` | Step-by-step prompt for creating a new component |
| `.github/agents/network-dashboard-implementer.md` | Agent profile with planning, compliance, and stop behavior |
| `.github/skills/dashboard-widget.md` | Optional skill for widget scaffolding |

## Stop Conditions

The AI must stop and ask the user when:

- A task requires modifying more than 3 existing files
- A task conflicts with any of the 20 rules
- The data shape is ambiguous or incomplete
- It is unclear whether to create or reuse a component

## Validation Checklist

Before completing any task, verify:

- [ ] `ng build` passes from `frontend/`
- [ ] No `any` types introduced
- [ ] No `console.log` statements
- [ ] No dead or commented-out code
- [ ] All imports are used
- [ ] SCSS nesting ≤ 3 levels
- [ ] CSS tokens used (no raw hex/px/rem)
- [ ] Implementation follows db.json → model → service → UI order
