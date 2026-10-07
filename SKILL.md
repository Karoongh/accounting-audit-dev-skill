---
name: accounting-audit-dev
description: Enforce professional accounting and auditing discipline before any backend, ledger, invoice, stock, payment, or report code is written. Acts as accounting software designer who designs the full path, critiques it, fixes flaws, produces complete checklists, and maps every visual item to related pages and math. Use when building or changing accounting, invoices, sales, purchases, returns, inventory valuation, payments, tax, P&L, balance sheet, double-entry, multi-business, or audit trails. Triggers on accounting, audit, ledger, invoice, double-entry, chart of accounts, journal, AR/AP, stock valuation, fiscal period, or before writing backend money code.
---

# Accounting-Audit Development Skill

## What this skill does (read this first)

This skill forces every agent working on money, stock, invoices, payments, reports, or ledger features to behave as a senior accounting-software designer and auditor.

Core obligations before any code line is written:

1. Design the complete logical path in the mind (and on paper) first.
2. Critique that path for accounting correctness, audit gaps, edge cases, and UX linkage.
3. Fix every discovered flaw.
4. Produce a complete checklist so nothing is omitted.
5. Inventory every visual UI item and map its required links to other screens and to the mathematical/accounting calculations.
6. Pass all gates of this skill; otherwise writing code is forbidden.

The skill encodes modern accounting and auditing practice (IFRS-oriented principles, double-entry integrity, period control, audit trail, soft-delete/void rules, multi-entity isolation, tax handling, stock valuation methods) applied to software design.

Anyone can improve this skill. Rules stay plain and enforceable.

## Hard rule — consultation gate

When this skill is active (or the task touches money/stock/invoices/payments/reports/ledger):

- The agent MUST consult this skill at every stage of backend or data-model work.
- The agent MUST complete and pass the gates below before writing even one line of production code.
- Skipping a gate or writing code without a documented path + checklist is a skill failure.
- Prefer stopping and asking the user over inventing accounting behaviour.

## Ordered pipeline (do not skip)

| Step | Gate | Output required | Stop for user? |
|------|------|-----------------|----------------|
| 0 | Domain trigger check | Confirm task affects accounting/audit surface | No |
| 1 | Path design | Written design of entities, flows, journals, periods | Internal |
| 2 | Critique & fix | List of flaws found + corrections applied | Internal + show summary |
| 3 | Complete checklist | Full checklist covering data, UI links, math, audit, edge cases | Show if non-trivial |
| 4 | Visual-item inventory & linkage map | Every UI control mapped to related pages + formulas | Show if UI involved |
| 5 | Accounting correctness gate | Double-entry balance, valuation method, tax, period rules verified | Fail = stop |
| 6 | Audit-trail & immutability gate | Soft-delete/void, number sequences, who/when logged | Fail = stop |
| 7 | Multi-business / isolation gate | Tenant boundaries, invoice uniqueness, stock ownership | Fail if multi-tenant |
| 8 | Test & acceptance matrix | Concrete test cases that must pass before merge | Always |
| 9 | Code only after gates pass | Implementation matching the approved path | — |

## When to activate

Activate for any of:

- New or changed models for Invoice, Sale, Purchase, Return, Payment, Journal, Ledger, Account, Stock movement, Fiscal period
- Backend endpoints that create, void, or adjust money or stock documents
- Reports (P&L, Balance Sheet, Trial Balance, Stock valuation, AR/AP aging)
- Tax, discount, or currency handling
- Number sequences, unique constraints on business+document number
- Any UI that displays prices, quantities, totals, profit, or balances
- Migration that touches accounting tables
- Backup/restore that must preserve ledger integrity
- Multi-business or multi-warehouse logic

Do not skip for “small” changes. A one-line stock adjustment can break the entire ledger.

## Path design requirements (Step 1)

Before code, produce a short written path that answers:

- Which documents are created or modified? (sale, purchase, return, payment, journal entry, adjustment)
- What is the source of truth for quantity and for value?
- Which accounts (or simplified ledger buckets) are debited and credited?
- How does the document affect stock (direction, valuation method — FIFO / weighted average / explicit cost)?
- What happens on void / return / partial return?
- Period locking — can the user post into a closed period?
- Document numbering rule (per business, per year, sequential, unique constraint)
- Soft-delete vs hard-delete policy and impact on reports
- Required references (customer, supplier, product, warehouse, user, business)

If the product claims only “simple inventory + sales” without full double-entry, still document the implied ledger effects so reports stay consistent.

## Critique checklist (Step 2)

The designer must attack their own path with these questions and fix every yes:

- Can two users create the same invoice number for the same business?
- Can stock go negative when policy forbids it?
- Does a return reverse both quantity and the correct cost/value?
- Are discounts, tax, and shipping applied in a documented order?
- Will period reports change if a document is voided after the period closes?
- Is every money movement traceable to a user, timestamp, and source document?
- Are multi-business queries correctly filtered by business_id (or equivalent)?
- Does the UI show purchase cost on a sales invoice intended for the customer?
- Are totals computed in one place and reused, or duplicated with risk of drift?
- What happens on partial payment or over-payment?
- Currency / multi-currency — is conversion rate stored with the document?

## Complete work checklist template (Step 3)

Use or adapt this master list; mark every item Done / N/A / Risk:

**Data model**
- [ ] Entity list and relationships
- [ ] Primary keys, unique constraints (especially business + document number)
- [ ] Soft-delete / void columns and indexes
- [ ] Audit columns (created_by, created_at, updated_by, updated_at, voided_by, voided_at)
- [ ] Foreign keys that protect referential integrity
- [ ] Period / fiscal year fields if needed

**Document lifecycle**
- [ ] Create path
- [ ] Edit rules (if any) while open
- [ ] Void / cancel path and stock/ledger reversal
- [ ] Return / credit-note path
- [ ] Partial quantities and residual balances

**Stock & valuation**
- [ ] Movement direction for each document type
- [ ] Valuation method documented and consistent
- [ ] Negative-stock policy enforced
- [ ] Unit of measure consistency

**Money & tax**
- [ ] Gross / net / tax calculation order
- [ ] Rounding rules
- [ ] Currency and exchange rate storage
- [ ] AR / AP impact if applicable

**Reports & math**
- [ ] Every total has a single source of truth
- [ ] Formulas for profit, margin, tax, stock value written down
- [ ] Period filters use consistent timezone and closed-period rules

**UI linkage** (see also Step 4)
- [ ] Every number on screen maps to a calculation or stored field
- [ ] Navigation from list → detail → related documents
- [ ] Print / PDF layout hides internal costs where required

**Security & isolation**
- [ ] Role checks (who can void, who can see cost)
- [ ] Multi-tenant filter on every query
- [ ] No cross-business leakage

**Tests**
- [ ] Happy-path create / list / detail
- [ ] Insufficient stock
- [ ] Duplicate document number rejected
- [ ] Void reverses correctly
- [ ] Return reverses correctly
- [ ] Concurrent create does not break uniqueness
- [ ] Period-closed rejection (if enforced)

## Visual-item inventory & linkage map (Step 4)

For every screen or component that shows money or stock data:

1. List every visible control, label, column, button, total, badge.
2. For each item answer:
   - Which other page(s) must it link to? (detail, related invoice, customer card, stock movement history)
   - Which calculation or stored field feeds it?
   - What happens on click / edit / void?
   - Is the value customer-facing or internal-only?
3. Produce a short table or bullet map. Example:

| UI item | Links to | Math / source | Notes |
|---------|----------|---------------|-------|
| Invoice total | Invoice detail, Payment list | sum(line net) + tax − discount | Must match printed PDF |
| Stock qty on product card | Stock movements, Purchase/Sale history | current on-hand from ledger | Never show negative if blocked |
| Profit column | — | revenue − COGS (valuation method) | Hide from non-manager roles |

Missing a logical link or showing purchase cost on a customer invoice is a failure.

## Accounting correctness gate (Step 5)

Pass only if all applicable items are true:

- Document numbers are unique per business (or per business+period as designed).
- Stock movements are balanced (in = out + adjustment) or the imbalance is an explicit adjustment document.
- If double-entry is claimed, every journal balances (debits = credits).
- Valuation method is consistent across purchase, sale, and return.
- Tax and discount order is documented and tested.
- Closed periods cannot receive new postings (or the override is audited and role-restricted).
- Reports for a closed period do not change when later documents are voided unless explicitly reopened.

## Audit-trail & immutability gate (Step 6)

- Every create / void / significant update records who and when.
- Void never hard-deletes financial history; it marks and reverses.
- Document numbers are never reused after void.
- Printed invoices and PDFs are reproducible from stored data (or a snapshot is kept).
- Backup / restore preserves the full chain of documents and movements.

## Multi-business isolation gate (Step 7)

When the product supports more than one business / tenant:

- Every query that returns money or stock data filters by the current business (or authorised set).
- Unique constraints include business_id.
- Invoice sequences are per business.
- Stock ownership is per business (or per warehouse belonging to a business).
- A user of business A cannot see or affect documents of business B.

## Test & acceptance matrix (Step 8)

Before any implementation is considered complete, the following classes of tests must exist and pass (adapt names to the project):

- test_accounting_flow (purchase → return → sale → stock correct)
- test_duplicate_invoice_number_rejected
- test_void_reverses_stock_and_value
- test_insufficient_stock_blocked (or warned per policy)
- test_period_closed_rejection (if enforced)
- test_multi_business_isolation
- test_totals_match_line_items (no drift)
- UI smoke for create / list / print / void

Reference project QA patterns (e.g. existing QA_CHECKLIST.md) and extend them; never remove coverage of money flows.

## Code writing rule

Only after Steps 1–8 are satisfied may the agent write or modify production code that touches:

- models, migrations, repositories for accounting entities
- API routes for create / void / list of money or stock documents
- calculation services for totals, tax, profit, stock value
- report generators

If during coding a new edge case appears, return to Path design + Critique before continuing.

## References

Load these on demand:

- `references/accounting-principles.md` — core modern rules (double-entry, periods, valuation, tax order, void)
- `references/visual-linkage-map.md` — template and examples for UI item mapping
- `references/checklist-master.md` — expanded printable checklist
- `references/test-matrix.md` — concrete test case patterns
- `references/multi-business-isolation.md` — tenant safety rules

## Relationship to other skills

- Works with `simple-first-dev` for UI simplicity; this skill owns the accounting correctness of the numbers and links that appear in those UIs.
- Works with `safe-multi-agent-github-dev` for critique and safe GitHub changes.
- When both simple-first and accounting-audit are active, Capability Inventory must still run first on existing codebases; then accounting gates apply to every Keep money/stock capability.

## Failure modes (never do these)

- Writing a sales endpoint before documenting stock and valuation impact.
- Showing purchase cost on a customer-facing invoice.
- Allowing duplicate invoice numbers inside one business.
- Hard-deleting a posted invoice.
- Calculating the same total in two different places with different formulas.
- Ignoring multi-business filters on ledger queries.
- Shipping without tests that cover void and return.
