# PulseFabric Monitor

> Enterprise infrastructure monitoring dashboard — GitHub Codespaces + Copilot Demo

**Authors:** Mingchieh Wang, Wei Tien Pang
**Date:** March 23, 2026

---

## What Is This?

PulseFabric Monitor is a live demo showing how **GitHub Codespaces** and **GitHub Copilot** can replace complex local dev setups with zero-friction, AI-assisted development.

The dashboard displays real-time KPI cards, traffic/incident charts, and alert feeds — all backed by a fake REST API. During a demo, presenters use Copilot Chat prompts to add new widgets **live**, proving that AI can follow strict coding rules and produce production-quality code.

### Key Highlights

- **Zero local setup** — open a Codespace and everything runs
- **AI governance** — 20 production code rules that Copilot must follow
- **Live coding demos** — 3 ready-to-use prompts for adding widgets on stage
- **Fallback branch** — `demo-completed` has scenario 2 pre-built as backup

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Angular 21, Angular Material (Material 3), ngx-echarts |
| Backend | json-server (fake REST API from `backend/db.json`) |
| Dev Environment | GitHub Codespaces, Dev Containers (Node.js 20) |
| AI Governance | copilot-instructions.md, prompt files, agent profiles |

---

## Project Structure

```
backend/
  db.json                          ← Fake REST data (KPIs, traffic, incidents, alerts)

frontend/src/app/
  core/
    models/api.models.ts           ← All typed DTOs
    services/dashboard-api.service.ts ← API abstraction layer
  dashboard/
    dashboard.ts / .html / .scss   ← Main dashboard page
    kpi-card/                      ← Generic KPI card (input-driven)
    chart-widget/                  ← Generic chart widget (input-driven)
    alert-list/                    ← Alert list component
    chart-config.factory.ts        ← Pure functions generating ECharts options

.github/
  copilot-instructions.md          ← 20 production code rules for Copilot
  prompts/                         ← 3 demo prompt files
  agents/                          ← Custom agent profile
  skills/                          ← Optional widget scaffolding skill

docs/
  demo-script.md                   ← Step-by-step demo script (3 scenarios)
  review-checklist.md              ← Code review checklist
  ai-working-rules.md             ← AI working rules summary
```

---

## Getting Started

### Option A: GitHub Codespaces (Recommended)

1. Click the green **Code** button on the repository page.
2. Select the **Codespaces** tab.
3. Click **Create codespace on main**.
4. Wait for the container to build — Node.js 20, Angular CLI, and all VS Code extensions install automatically.
5. Run the dev servers:

```bash
npm run dev
```

6. The Angular app opens at **port 4200** and the API runs at **port 3000**.

### Option B: Local Development

```bash
# Clone the repository
git clone https://github.com/mwangui/Zero-friction.git
cd Zero-friction

# Install root dependencies (concurrently, json-server)
npm install

# Install frontend dependencies
cd frontend && npm install && cd ..

# Start both servers
npm run dev
```

| URL | Service |
|-----|---------|
| `http://localhost:4200` | Angular dashboard |
| `http://localhost:3000` | json-server API |

The Angular dev server proxies `/api/*` requests to json-server automatically.

### Available Scripts

| Command | Description |
|---------|-------------|
| `npm run dev` | Start both Angular + json-server concurrently |
| `npm run dev:web` | Start Angular dev server only |
| `npm run dev:api` | Start json-server only |
| `npm run build:web` | Production build |
| `npm run validate` | Build + lint |

---

## How to Demo

See [docs/demo-script.md](docs/demo-script.md) for the full script. Quick summary:

### Demo 1: Add a KPI Card

Open Copilot Chat and paste:

> Add a new KPI card called Active Sessions showing the number 14,200 with a trending up indicator. Put it in the same row as the existing KPI cards. All KPI cards must stay in one single row and share the width equally at 100% total width, no matter how many cards there are. Follow all repo rules in copilot-instructions.md.

### Demo 2: Add a Chart to AI Drop Zone

> Add a CPU Load by Device bar chart to the AI Widget Drop Zone at the bottom of the dashboard. Show 5 devices with fake CPU percentage data. Add the data to db.json first, then create the chart. Follow all repo rules in copilot-instructions.md.

### Demo 3: Add a Brand New Widget

> Add a new AI Security Recommendations widget to the dashboard. It should show 3 recommendations, each with a priority level (High, Medium, Low) and a short description. Add the data to db.json first, then build the component. Follow all repo rules in copilot-instructions.md.

**Fallback:** If any demo fails live, switch to the `demo-completed` branch which has scenario 2 pre-built.

---

## AI Governance

This repo enforces 20 production code rules via `.github/copilot-instructions.md`. Key rules:

- **Implementation order:** db.json → typed model → service → UI (never skip steps)
- **No `any` type** — all data must be typed with interfaces
- **No hardcoded data** — everything flows from the API
- **CSS tokens only** — use `--mat-sys-*` and `--pf-*` variables
- **Reuse before creating** — extend existing components via inputs

See [docs/ai-working-rules.md](docs/ai-working-rules.md) for the full list.

---

## Branches

| Branch | Purpose |
|--------|---------|
| `main` | Clean baseline — ready for live demo |
| `demo-completed` | Scenario 2 (CPU Load chart) pre-built as fallback |
