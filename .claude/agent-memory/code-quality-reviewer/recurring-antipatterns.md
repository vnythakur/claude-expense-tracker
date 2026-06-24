---
name: recurring-antipatterns
description: Known quality debt, bugs, and anti-patterns observed across the codebase in the first full review
metadata:
  type: project
---

## Duplicated categories array
`TransactionForm.jsx` line 3 and `TransactionList.jsx` line 3 both declare an identical `const categories = [...]` array. No shared constants file exists. This is the single most impactful maintainability fix in the codebase.

**Why:** Adding or renaming a category requires changing two files and risks them drifting out of sync.
**How to apply:** Always flag when either file is edited; suggest extracting to `src/constants.js`.

## Seed data bug — "Freelance Work" typed as expense
`App.jsx` line 13: `{ id: 4, description: "Freelance Work", amount: 800, type: "expense", category: "salary" }` — a salary-category item is typed as `"expense"`. This makes the Summary totals incorrect in the default state and could confuse learners.

## No currency formatting (toLocaleString / Intl.NumberFormat)
`Summary.jsx` displays amounts as bare `$` + raw number (e.g. `$5000`). Large or decimal amounts will render without commas or fixed decimal places (e.g. `$5000` instead of `$5,000.00`). Same pattern in `TransactionList.jsx` line 51.

## Missing `<label>` elements in TransactionForm
`TransactionForm.jsx` has no `<label>` tags for any of its inputs or selects — only placeholder text. Placeholders are not accessible substitutes for labels.

## Inline event handler in TransactionList delete button
`TransactionList.jsx` lines 54-56: the delete button uses an inline handler that calls `window.confirm`. This mixes UI logic into JSX, is untestable, and `window.confirm` blocks the main thread.

## `handleAdd` uses spread instead of functional updater
`App.jsx` line 21: `setTransactions([...transactions, transaction])` closes over stale `transactions`. Should use functional form: `setTransactions(prev => [...prev, transaction])`. Same risk exists for `handleDelete` line 25.

## Missing empty-state for TransactionList table
When all transactions are filtered out, the table renders with no rows and no message. There is no empty-state UI.

## `Date.now()` used as transaction ID
`TransactionForm.jsx` line 16: IDs are generated with `Date.now()`, which is not guaranteed unique if two transactions are added within the same millisecond (unlikely but semantically wrong). A counter or `crypto.randomUUID()` would be more correct.

## `outline: none` on form inputs without replacement focus style
`App.css` line 122: `outline: none` removes the browser's default focus ring. The replacement `box-shadow` on `:focus` partially compensates, but this is still an accessibility concern for high-contrast mode users.

## `Cell key` uses array index in SpendingChart
`SpendingChart.jsx` line 32: `<Cell key={index} ...>` uses array index as key. Since data is derived from a reduce over a fixed category set, keys will never reorder — low-risk but still a pattern to flag.

## `.delete-btn` CSS is defined but was previously unused
`App.css` lines 220-235 define `.delete-btn` styles. As of the "Improve the design" commit the delete feature IS implemented in `TransactionList.jsx`, so the rule is now active. CLAUDE.md note is stale.
