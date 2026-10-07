# Accounting Principles for Software Design

Apply these rules when designing or reviewing any money, stock, or ledger feature.

## Double-entry mindset (even in simplified systems)

- Every economic event has two sides. Even if the product stores only “stock movements + invoices”, document the implied debit and credit so reports stay consistent.
- Debits must equal credits for any journal that claims full accounting.
- Simplified products still need a clear source of truth for quantity and for value.

## Document numbering and uniqueness

- Invoice / document numbers are unique inside a business (or inside business + fiscal period if that is the design).
- Preferred DB constraint: unique (business_id, document_type, number) or equivalent.
- Numbers are never reused after void.
- Sequences should be gap-tolerant under concurrency (use DB unique constraint + retry or atomic counter).

## Soft-delete / void vs hard-delete

- Posted financial documents are never hard-deleted.
- Void marks the document and posts reversing stock and value movements.
- Reports of closed periods remain stable unless the period is explicitly reopened under audit.

## Stock valuation methods

Document and stick to one primary method per business (or per warehouse if multi-location is real):

- FIFO
- Weighted average cost
- Explicit cost on each movement (common in small systems)

Returns and voids must reverse the same value that was originally posted, or follow the documented method consistently.

## Tax, discount, shipping order

Write the exact order once and reuse it everywhere:

1. Line quantity × unit price
2. Line discount (if any)
3. Tax on the taxable base
4. Document-level discount / shipping / other charges

Rounding rules (per line vs per document) must be stated.

## Period control

- Define what a period is (calendar month, fiscal year, custom).
- Closed periods reject new postings (or require elevated role + audit log).
- Timezone for period boundaries must be explicit.

## Audit trail minimum

Every create, void, and significant update records:

- who (user id)
- when (timestamp with timezone)
- what changed (or full before/after for critical fields)

Printed invoices must be reproducible from stored data or from an immutable snapshot.

## Multi-entity isolation

When the product has multiple businesses / tenants:

- business_id (or equivalent) is part of every unique constraint on documents.
- Every read and write query filters by authorised business(es).
- Stock is owned by a business (or by a warehouse that belongs to one business).

## Reports integrity

- A total shown on screen must have a single calculation path.
- Profit / margin = revenue − COGS according to the chosen valuation method.
- Gross vs net and with-tax vs without-tax labels must be honest.
- Empty or partial data must not invent numbers.
