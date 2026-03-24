---
mode: agent
description: "Add a new KPI widget to the PulseFabric Monitor dashboard"
---

# Demo: Add a KPI Card

Use this prompt during live demo. Copy and paste the sentence below into Copilot Chat:

> Add a new KPI card called Active Sessions showing the number 14,200 with a trending up indicator. Put it next to the existing KPI cards. Follow all repo rules in copilot-instructions.md.

---

## Task: Add a New KPI Widget

Follow the strict implementation order below. Do not skip any step.

### Step 1 — db.json
Open `backend/db.json` and add a new entry to the `kpis` array:
```json
{
  "id": "<kebab-case-id>",
  "title": "<Display Title>",
  "value": "<string value>",
  "unit": "<optional unit>",
  "trend": "up | down | stable",
  "trendLabel": "<human-readable trend>",
  "icon": "<Material Icon name>"
}
```

### Step 2 — Verify Typed Model
Ensure `KpiDto` in `frontend/src/app/core/models/api.models.ts` already covers the shape. If new fields are needed, add them to the interface.

### Step 3 — Verify Service
Confirm `DashboardApi.getKpis()` in `frontend/src/app/core/services/dashboard-api.service.ts` already returns `Observable<KpiDto[]>`. No change should be needed.

### Step 4 — UI (Automatic)
Because the dashboard template uses `@for (kpi of kpis(); track kpi.title)` with `<app-kpi-card>`, the new KPI renders automatically. **No component code changes required.**

### Validation
- Run `ng build` — must pass with no errors.
- Confirm the new KPI card appears in the browser.
- Confirm no `console.log`, no `any` type, no hardcoded values.
