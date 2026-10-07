# accounting-audit-dev

Skill that forces professional accounting and auditing discipline before any backend, ledger, invoice, stock, payment, or report code is written.

## What it does

- Acts as a senior accounting-software designer: design the full path first, critique it, fix flaws, then produce a complete checklist
- Maps every visual UI item to related pages and to the underlying math/accounting calculations
- Hard consultation gate — agent must pass the ordered gates before writing even one line of money/stock code
- Encodes modern practice (double-entry mindset, period control, soft-delete/void, multi-business isolation, tax/discount order, stock valuation, audit trail)
- Works with existing project patterns (e.g. MultiBiz sales/purchase/return flows, unique invoice constraints, backup)

## When to use

Say the skill name or use phrases such as:

- accounting
- audit
- ledger
- invoice / double-entry
- chart of accounts / journal
- AR/AP
- stock valuation
- fiscal period
- before writing backend money code
- void / return / payment logic

## Files

- `SKILL.md` — main rules, ordered pipeline, gates
- `references/accounting-principles.md` — core modern rules
- `references/visual-linkage-map.md` — UI item mapping template
- `references/checklist-master.md` — expanded checklist
- `references/test-matrix.md` — required test classes
- `references/multi-business-isolation.md` — tenant safety

## Companion skills

- [simple-first-dev](https://github.com/Karoongh/simple-first-dev-skill) — UI simplicity and capability inventory
- [safe-multi-agent-github-dev](https://github.com/Karoongh/safe-multi-agent-github-dev-skill) — multi-agent critique and safe GitHub work

## How to edit

Open `SKILL.md` for core rules and gates.  
Put detailed catalogs and templates in `references/`.
