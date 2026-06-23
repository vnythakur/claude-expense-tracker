# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev      # Start dev server at http://localhost:5173
npm run build    # Production build
npm run lint     # ESLint check
npm run preview  # Preview production build
```

No test suite is configured.

## Architecture

Single-page React app (Vite + React 19). No routing, no state management library, no backend — all data lives in `useState` and is lost on refresh.

**Component tree:**
```
App                        — holds transactions[] and handleAdd; renders children
├── Summary                — receives transactions[], computes totalIncome/totalExpenses/balance internally
├── TransactionForm        — owns its own form state; calls onAdd(transaction) prop when submitted
└── TransactionList        — receives transactions[]; owns its own filter state (filterType, filterCategory)
```

**Data shape for a transaction:**
```js
{ id: number, description: string, amount: number, type: "income"|"expense", category: string, date: "YYYY-MM-DD" }
```

**Categories** (hardcoded in both `TransactionForm.jsx` and `TransactionList.jsx`): `food`, `housing`, `utilities`, `transport`, `entertainment`, `salary`, `other`.

The CSS class `.delete-btn` is defined in `App.css` but the delete feature is not yet implemented — the table has no delete column.

## Project context

This is a course starter project from [Code with Mosh's Claude Code course](https://codewithmosh.com/p/claude-code). It is intentionally left with bugs, poor UI, and messy code to be fixed during the course.
