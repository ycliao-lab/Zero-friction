# Coding Standards

## CSS / SCSS Rules

- ALL colors must use `var(--pf-*)` or `var(--mat-sys-*)`. Never write raw hex, rgb(), or rgba() in components.
- ALL spacing must use `var(--pf-spacing-*)`. Never write raw px values for padding, margin, or gap.
- ALL icon sizes must use `var(--pf-icon-size-*)`. Never write raw px for font-size, width, height on icons.
- ALL border-radius must use `var(--pf-radius-*)`.
- ALL shadows must use `var(--pf-shadow-*)`.
- If a token doesn't exist yet, ADD it to `frontend/src/app/shared/styles/_tokens.scss` first, then use the token. Never hardcode.

## Component Rules

- Reusable UI must be extracted into shared components.
- API data must have a typed interface in `core/models/api.models.ts`.
- API URLs must use `environment`, never hardcode URLs in services.

## When Unsure

- If you are not sure whether a value should be a token, ASK the user before writing raw values.
- If a design requirement conflicts with these rules, ASK the user before proceeding.
