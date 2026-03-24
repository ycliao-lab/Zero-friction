---
name: dashboard-widget
description: "(Optional) Skill for scaffolding dashboard widgets following PulseFabric conventions"
optional: true
---

# Dashboard Widget Skill

This skill provides step-by-step scaffolding for adding widgets to the PulseFabric Monitor dashboard.

## When to Use

Use this skill when:
- Adding a new KPI card (data-only change in `db.json`)
- Adding a new chart widget (factory function + template reference)
- Adding a new list or table widget (new component if no existing one fits)

## Widget Types

### KPI Card (no new component needed)
1. Add entry to `kpis` array in `backend/db.json`
2. Verify `KpiDto` interface covers the shape
3. Card renders automatically via `@for` loop

### Chart Widget (no new component needed)
1. Add data collection to `backend/db.json`
2. Add DTO to `api.models.ts`
3. Add service method to `dashboard-api.service.ts`
4. Add factory function to `chart-config.factory.ts`
5. Add `<app-chart-widget>` in `dashboard.html` with `[title]` and `[options]`

### Custom Widget (new component)
1. Follow the full db.json → model → service → UI flow
2. Create standalone component under `frontend/src/app/dashboard/`
3. Must be input-driven, use Material 3 tokens, separate files

## Conventions

- Chart colors: use `PALETTE` from `chart-config.factory.ts`
- Icons: use Material Icons (e.g., `speed`, `warning`, `cloud`)
- Spacing: use `--pf-space-*` tokens
- Border radius: use `--pf-radius-*` tokens
