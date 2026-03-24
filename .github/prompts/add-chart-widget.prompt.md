---
mode: agent
description: "Add a new chart widget to the PulseFabric Monitor dashboard"
---

# Demo: Add a Chart to AI Drop Zone

Use this prompt during live demo. Copy and paste the sentence below into Copilot Chat:

> Add a CPU Load by Device bar chart to the AI Widget Drop Zone at the bottom of the dashboard. Show 5 devices with fake CPU percentage data. Add the data to db.json first, then create the chart. Follow all repo rules in copilot-instructions.md.

---

## Task: Add a New Chart Widget

Follow the strict implementation order. Do not skip any step.

### Step 1 — db.json
Open `backend/db.json` and add a new top-level collection for the chart data. Example:
```json
"myNewData": [
  { "label": "A", "value": 10 },
  { "label": "B", "value": 20 }
]
```

### Step 2 — Typed Model
Add the DTO interface to `frontend/src/app/core/models/api.models.ts`:
```ts
export interface MyNewDto {
  label: string;
  value: number;
}
```

### Step 3 — Service Method
Add a method to `frontend/src/app/core/services/dashboard-api.service.ts`:
```ts
getMyNewData(): Observable<MyNewDto[]> {
  return this.http.get<MyNewDto[]>(`${this.base}/myNewData`);
}
```

### Step 4 — Chart Factory Function
Add a pure function in `frontend/src/app/dashboard/chart-config.factory.ts`:
```ts
export function buildMyNewChartOption(data: MyNewDto[]): EChartsOption {
  // Use PALETTE constants for colors.
  // Return a complete EChartsOption object.
}
```
Colors MUST come from the `PALETTE` constant — no raw hex in the function.

### Step 5 — Dashboard Integration
In `frontend/src/app/dashboard/dashboard.ts`:
- Import the new factory function and DTO.
- Add a signal for the chart options.
- Call the new service method inside the existing `forkJoin` block.

In `frontend/src/app/dashboard/dashboard.html`:
- Add a `<app-chart-widget>` reference with `[title]` and `[options]`.

**Do NOT create a new component.** Use the existing `ChartWidget` with different inputs.

### Validation
- Run `ng build` — must pass.
- Confirm the chart renders in the browser.
- Confirm no `console.log`, no `any`, no magic numbers, SCSS ≤ 3 levels.
