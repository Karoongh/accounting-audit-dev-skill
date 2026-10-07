# Master Accounting Checklist

Copy and mark each item Done / N/A / Risk for the current task.

## 1. Scope & path

- [ ] Written path design exists (entities, flows, valuation, void behaviour)
- [ ] Critique performed and flaws fixed
- [ ] No invented accounting rules without user confirmation

## 2. Data model

- [ ] Entities listed with relationships
- [ ] Unique constraint includes business (or designed scope) + document number
- [ ] Soft-delete / void flags and indexes
- [ ] Audit columns (created_by/at, voided_by/at, …)
- [ ] Foreign keys protect integrity
- [ ] Period / fiscal fields present if used

## 3. Document lifecycle

- [ ] Create path complete
- [ ] Edit rules while open (if any) documented
- [ ] Void path reverses stock and value
- [ ] Return / credit-note path defined
- [ ] Partial quantities and residual balances handled
- [ ] Concurrent create cannot produce duplicate numbers

## 4. Stock & valuation

- [ ] Movement direction for sale / purchase / return / adjustment
- [ ] Valuation method named and consistent
- [ ] Negative-stock policy enforced in code and tests
- [ ] Unit of measure consistent

## 5. Money, tax, rounding

- [ ] Calculation order written (qty × price → discount → tax → shipping)
- [ ] Rounding rule stated
- [ ] Currency and rate stored with document if multi-currency
- [ ] AR/AP impact clear if applicable

## 6. Reports & single source of truth

- [ ] Every displayed total has one calculation path
- [ ] Profit / margin formula written
- [ ] Period filters use consistent timezone and closed-period rules
- [ ] Closed-period reports do not silently change

## 7. Visual linkage

- [ ] Every UI money/stock item mapped (see visual-linkage-map.md)
- [ ] Customer-facing views hide internal cost
- [ ] List → detail → related documents navigation exists
- [ ] Print / PDF matches on-screen customer view

## 8. Security & isolation

- [ ] Role checks for void, cost visibility, period reopen
- [ ] Multi-business filter on every money/stock query
- [ ] No cross-tenant leakage possible

## 9. Tests that must exist and pass

- [ ] Accounting flow (purchase → return → sale → stock)
- [ ] Duplicate document number rejected
- [ ] Void reverses correctly
- [ ] Insufficient stock blocked or warned per policy
- [ ] Multi-business isolation
- [ ] Totals match line items
- [ ] UI smoke create / list / print / void

## 10. Final gate

- [ ] All applicable accounting-correctness, audit-trail, isolation gates passed
- [ ] Ready for code only after the above
