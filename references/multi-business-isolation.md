# Multi-Business / Tenant Isolation Rules

When the product supports more than one business (or any tenant concept):

## Mandatory

1. **business_id (or equivalent) on every financial and stock document**
2. **Unique constraints include the business scope**  
   Example: UNIQUE (business_id, invoice_number)
3. **Every read query filters by the authorised business(es)**  
   No “select all invoices” without a business predicate in production code paths.
4. **Stock belongs to a business** (directly or via warehouse that belongs to a business)
5. **Sequences / counters are per business**
6. **A token or session for business A never returns data of business B**

## Tests required

- Create document as A, attempt read/update/void as B → 403 or empty result
- List endpoints never leak cross-business rows
- Unique constraint is scoped; two businesses may both have invoice “1001”

## Common leaks to watch for

- Global sequence table without business key
- Report queries that omit business filter
- Search that matches across all tenants
- Backup/restore that mixes businesses without clear separation
- Admin “super” views that forget to re-apply filters when switching context

If the product is single-business only, document that fact and still keep the data model ready for a future business_id column (additive migration later).
