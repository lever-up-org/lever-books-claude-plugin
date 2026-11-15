---
name: where-did-my-money-go
description: Shows a person where their money went, from Lever Books, by category and by merchant, for any period against the one before, with the biggest changes called out, and builds a spending dashboard. Use when the person asks "where did my money go", "what did I spend this month", "how much did I spend on fuel last quarter", "what's my biggest expense", "am I spending more than last month", or wants a spending dashboard. Do NOT use for recurring charges and subscriptions (subscriptions-and-bills), future cash (cash-flow-outlook) or what is tax deductible (tax-time-pack).
---

# Where did my money go

Answer "where did it go" with totals from Lever Books, compared with the period before, in plain
words. Follow [the Lever Books rules](references/lever-books.md).

If the bank tools aren't on this connection, say Lever Books has no bank data here yet and offer
what its receipts and documents show instead.

## Non-negotiables

- Totals come from `lever-books:summarise_bank_transactions`, never from adding up pages of rows.
- Leave out transfers and card repayments, and say you did.
- Explain, don't judge: "Food and drink was up $210", not "you spent too much".

## Steps

1. **Fix the period.** Default to the last full calendar month, compared with the month before.
   Name both.
2. **By category**: `lever-books:summarise_bank_transactions` with `groupBy` `category`, for the period and
   for the comparison. Each group carries its money in and out. Drop the transfer and card
   repayment codes, then add up the rest for the period's money in and out.
3. **By merchant**: `lever-books:summarise_bank_transactions` with `groupBy` `merchant` and `direction` `out`,
   for the period.
4. **Find the movers.** Rank categories by the dollar change, not the percentage, so small
   categories don't top the list.
5. **Drill in when asked**: `lever-books:search_bank_transactions` with the exact `category` code the summary
   returned, or `text` for a merchant, lists the payments behind a total.

## Output

Answer in chat in this shape:

```
‹Profile›, ‹period› (compared with ‹comparison›)

$‹out› went out and $‹in› came in, so you were $‹net› ‹ahead or behind›.

| Category | This period | Before | Change |
| ‹top six by spend, then "Everything else"› |

Biggest changes
- ‹category›: +$‹x›, mostly ‹merchant›
- ‹category›: −$‹x›

Left out: $‹x› of transfers between your accounts and card repayments.
```

Then offer the spending dashboard: an HTML artifact, starting from the HTML in `assets/dashboard.md`, following
[the spending dashboard brief](assets/spending-dashboard.md).

## Gotchas

| Situation | What to do |
| --- | --- |
| Spending with no category | The summary returns it as a group with no key. Show it as "Not categorised"; the person can set a category on each payment in Lever Books |
| Pending transactions | Included. If the period is this week, say some are still pending |
| A refund shows as money in | It lowers a category only if the bank gave it that category. Say so when a category looks low |
| Less history in the comparison period | Say Lever Books holds data from ‹date›, and compare only what both cover |
