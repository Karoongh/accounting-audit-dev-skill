# Test Matrix Patterns for Accounting Features

Implement (or extend) tests that cover these classes before merging money/stock code.

## Core flow

- Purchase N units → stock increases by N, value increases by cost
- Partial or full return of purchase → stock and value reverse correctly
- Sale of goods → stock decreases, revenue recorded, COGS according to valuation method
- Sale of service only → stock unchanged
- Sale quantity > available stock → 422 or policy-defined behaviour
- After the above sequence, on-hand quantity and value match expectations

## Uniqueness and concurrency

- Two simultaneous creates with the same intended number → exactly one succeeds, the other is rejected by unique constraint
- Document number never reused after void

## Void and return

- Void of sale restores stock and removes revenue/COGS impact (or posts reversing entries)
- Void of purchase restores stock downward
- Return of sale (customer return) increases stock and reverses revenue/COGS appropriately
- Partial return leaves residual quantities correct

## Isolation

- User/token of business A cannot read or write documents of business B
- Queries without business filter are forbidden in production paths

## Totals integrity

- Invoice total == sum of line nets + tax − discounts (within rounding)
- Front-end displayed total matches API total
- Printed PDF total matches stored total

## Period (if enforced)

- Posting into a closed period is rejected (or requires elevated role + audit)
- Re-opening a period is itself audited

## UI / acceptance smoke (manual or automated)

- Create sale from UI appears in list with correct totals
- Print hides purchase cost
- Double-submit does not create two invoices
- Staff role cannot see manager-only cost/profit columns

Adapt names to the project (e.g. test_accounting_flow, test_sqlite_backup). Never drop existing money-flow coverage when adding new features.
