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

This is a single-page React app (Vite + React 19) with all logic in `src/App.jsx`. There is no routing, no state management library, and no backend — all data lives in `useState` and is lost on refresh.

**Known bug in the initial code:** `amount` is stored as a string in state, so `totalIncome` and `totalExpenses` use string concatenation instead of numeric addition — the summary cards show wrong totals. Fix by parsing `amount` to a number when adding a transaction or when reducing.

**Data shape for a transaction:**
```js
{ id: number, description: string, amount: string|number, type: "income"|"expense", category: string, date: "YYYY-MM-DD" }
```

**Categories** (hardcoded array in `App.jsx`): `food`, `housing`, `utilities`, `transport`, `entertainment`, `salary`, `other`.

The CSS class `.delete-btn` is defined in `App.css` but the delete feature is not yet implemented in `App.jsx` — the table has no delete column.

## Project context

This is a course starter project from [Code with Mosh's Claude Code course](https://codewithmosh.com/p/claude-code). It is intentionally left with bugs, poor UI, and messy code to be fixed during the course.
