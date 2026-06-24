---
name: project-architecture
description: Architectural decisions, naming conventions, and style patterns observed in this codebase
metadata:
  type: project
---

## State ownership
All persistent state (`transactions[]`) lives in `App.jsx`. Children receive it as props. This is intentional — no state management library is used and data is lost on refresh. Do not suggest lifting state further or adding Redux/Zustand unless the user asks.

## Component responsibilities (as designed)
- `Summary` — pure display, computes totals inline from `transactions[]` prop
- `TransactionForm` — owns its own controlled-form state; communicates up via `onAdd` prop
- `TransactionList` — owns its own filter state (`filterType`, `filterCategory`); communicates deletes up via `onDelete` prop
- `SpendingChart` — pure display, derives pie data from `transactions[]` prop; renders null when no expenses

## Styling approach
Single `App.css` file with global selectors. No CSS modules, no Tailwind. BEM-lite modifier classes used for summary cards (`.summary-card--income`). Tag-level selectors used for `form`, `table`, `th`, `td` — these are scoped by context but technically global.

## No `<label>` convention (gap to fill)
No component in the codebase uses `<label>` elements. This is a systemic accessibility gap, not a one-off.

## Export style
`Summary`, `TransactionForm`, `TransactionList` use `export default` at the bottom of the file. `SpendingChart` uses `export default function` inline declaration. Both styles coexist — no enforced convention.

## No PropTypes or TypeScript
The project is plain JavaScript. No runtime type checking. Transaction shape is documented in CLAUDE.md only.

## Recharts used for SpendingChart
`PieChart`, `Pie`, `Cell`, `Tooltip`, `Legend`, `ResponsiveContainer` from `recharts`. COLORS array is hardcoded to 7 values matching the 7 categories.
