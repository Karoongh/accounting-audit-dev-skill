# Visual Item Inventory & Linkage Map

Use this template for every screen that shows money, stock, prices, totals, or balances.

## Procedure

1. Open the screen (or its prototype / HTML).
2. List every visible numeric or money-related control, column, badge, button, total.
3. For each item fill the table below.
4. Any missing logical link or wrong visibility (e.g. cost visible to customer) is a defect to fix before code.

## Template table

| UI item | Related page(s) / action | Math or stored source | Customer-facing? | Role restriction | Notes |
|---------|--------------------------|-----------------------|------------------|------------------|-------|
| Invoice total | Invoice detail, Payments | sum(lines) + tax − disc | Yes | — | Must equal PDF |
| Line unit price | Product card (optional) | stored selling price | Yes | — | |
| Line cost | — | purchase / valuation cost | No | Manager only | Never on customer invoice |
| Stock on-hand | Stock movements, Purchases, Sales | ledger quantity | Internal | Staff+ | Block negative if policy |
| Profit / margin | — | revenue − COGS | No | Manager | Label method used |
| Void button | Confirmation + reverse movements | — | No | Authorised role | Logs who/when |
| Document number | Search / list filter | unique per business | Yes | — | Never reuse after void |

## Common failure patterns to catch

- Purchase cost column visible on a sales invoice print for the customer.
- Total calculated in the front-end differently from the back-end.
- Clicking a total does not open the detail or related documents.
- Stock quantity has no link to the movement history that produced it.
- Profit shown without clarifying gross vs net or the valuation method.
- “Edit” allowed on a posted document that should only be voided.

## When to re-run

- Any new column or total added to a money screen.
- Any change to print / PDF layout.
- After Capability Inventory Keep decisions that touch accounting UI.
