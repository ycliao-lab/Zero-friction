# Demo Script — PulseFabric Monitor

This script contains 3 live demo scenarios. Each scenario is designed to be executed in Copilot Chat during a presentation.

**Prerequisites:**
- Codespace is running (or local dev with `npm run dev`)
- Both servers are up: Angular on port 4200, json-server on port 3000
- Dashboard is visible in the browser at `http://localhost:4200`

---

## Scenario 1: Add a KPI Card — "Active Sessions"

### Story

> "Our operations team wants to see how many users are currently connected. Let's add an Active Sessions KPI card to the dashboard — live, using only a Copilot Chat prompt."

### Copilot Chat Prompt

Copy and paste this into Copilot Chat:

> Add a new KPI card called Active Sessions showing the number 14,200 with a trending up indicator. Put it next to the existing KPI cards. Follow all repo rules in copilot-instructions.md.

### Expected AI Actions

1. **db.json** — Add a new entry to the `kpis` array:
   ```json
   {
     "id": "active-sessions",
     "title": "Active Sessions",
     "value": "14,200",
     "trend": "up",
     "trendLabel": "+8.3% from last hour",
     "icon": "group"
   }
   ```
2. **api.models.ts** — No change needed (KpiDto already covers the shape)
3. **dashboard-api.service.ts** — No change needed (getKpis already returns the array)
4. **UI** — No change needed (the `@for` loop renders it automatically)

### Expected Result

- A fifth KPI card appears in the grid: "Active Sessions — 14,200 — trending up"
- The card uses the same `<app-kpi-card>` component as existing cards
- `ng build` still passes

### What to Point Out

- Only `db.json` was changed — the AI understood the data-first pattern
- No new component was created — the existing KPI card is input-driven
- The AI followed the implementation order: db.json → model → service → UI

---

## Scenario 2: Add a CPU Load Chart to AI Drop Zone

### Story

> "We have a drop zone at the bottom of the dashboard reserved for AI-generated widgets. Let's ask Copilot to fill it with a CPU Load by Device bar chart."

### Copilot Chat Prompt

Copy and paste this into Copilot Chat:

> Add a CPU Load by Device bar chart to the AI Widget Drop Zone at the bottom of the dashboard. Show 5 devices with fake CPU percentage data. Add the data to db.json first, then create the chart. Follow all repo rules in copilot-instructions.md.

### Expected AI Actions

1. **db.json** — Add a new `cpuLoad` collection (replacing the empty array):
   ```json
   "cpuLoad": [
     { "device": "Server-A1", "load": 72 },
     { "device": "Server-B2", "load": 85 },
     { "device": "Router-C3", "load": 45 },
     { "device": "Switch-D4", "load": 91 },
     { "device": "Gateway-E5", "load": 63 }
   ]
   ```
2. **api.models.ts** — Add `CpuLoadDto` interface:
   ```typescript
   export interface CpuLoadDto {
     device: string;
     load: number;
   }
   ```
3. **dashboard-api.service.ts** — Add `getCpuLoad()` method
4. **chart-config.factory.ts** — Add `buildCpuLoadOption()` factory function using PALETTE colors
5. **dashboard.ts** — Add signal + service call in forkJoin
6. **dashboard.html** — Add `<app-chart-widget>` inside or near `#ai-dropzone`

### Expected Result

- A bar chart titled "CPU Load by Device" appears in the AI Widget Drop Zone area
- 5 bars showing percentage values for each device
- Colors come from the PALETTE constant (no raw hex in factory function)
- `ng build` still passes

### What to Point Out

- The AI followed all 6 steps of the implementation order
- No new component was created — the existing `ChartWidget` was reused
- Chart colors use the PALETTE constant, not hardcoded hex values
- The factory function is pure (no side effects)

### Fallback

If this scenario fails live, switch to the `demo-completed` branch:
```bash
git stash && git checkout demo-completed
```
Then restart the dev servers with `npm run dev`.

---

## Scenario 3: Add an AI Security Recommendations Widget

### Story

> "The security team wants an AI-powered recommendations panel. This requires a brand new component since it doesn't fit the KPI card or chart widget pattern. Let's see if Copilot can build it from scratch while following all 20 rules."

### Copilot Chat Prompt

Copy and paste this into Copilot Chat:

> Add a new AI Security Recommendations widget to the dashboard. It should show 3 recommendations, each with a priority level (High, Medium, Low) and a short description. Add the data to db.json first, then build the component. Follow all repo rules in copilot-instructions.md.

### Expected AI Actions

1. **db.json** — Add a `securityRecommendations` collection:
   ```json
   "securityRecommendations": [
     { "id": 1, "priority": "high", "description": "Rotate API keys for production cluster — last rotated 90+ days ago" },
     { "id": 2, "priority": "medium", "description": "Enable MFA for 3 service accounts in US-East region" },
     { "id": 3, "priority": "low", "description": "Update TLS certificates to use SHA-256 on staging endpoints" }
   ]
   ```
2. **api.models.ts** — Add `SecurityRecommendationDto` interface
3. **dashboard-api.service.ts** — Add `getSecurityRecommendations()` method
4. **New component** — Create `security-recommendations/` under `dashboard/`:
   - `security-recommendations.ts` — standalone, input-driven
   - `security-recommendations.html` — flat HTML with Material components
   - `security-recommendations.scss` — uses `--mat-sys-*` and `--pf-*` tokens
5. **dashboard.ts** — Import component, fetch data, store in signal
6. **dashboard.html** — Add `<app-security-recommendations>` tag

### Expected Result

- A new card-style widget appears showing 3 security recommendations
- Each item shows a priority badge (High/Medium/Low) with appropriate color
- The component uses Material 3 tokens for all styling
- `ng build` still passes

### What to Point Out

- The AI created a **new component** because the data doesn't fit existing patterns
- It still followed the strict order: db.json → model → service → UI
- TS, HTML, SCSS are in separate files (Rule 1)
- The component is standalone and input-driven (Rules 4, 11)
- No `any` type anywhere (Rule 3/19)

---

## Tips for Presenters

1. **Keep the terminal visible** — the audience should see file changes in real time
2. **Show the Source Control diff** — highlight which files Copilot changed
3. **Run `ng build`** after each scenario to prove the code compiles
4. **Refresh the browser** after scenario 1 (db.json change may need a server restart)
5. **Have the `demo-completed` branch ready** as a fallback for scenario 2
6. **Point out the rules** — open `copilot-instructions.md` to show what the AI is following
