---
name: subscriptions-and-bills
description: Finds the subscriptions and recurring charges in a Lever Books profile's bank transactions, with how often each charges, the next expected date, the yearly cost and any price rise, plus the bills in Lever Books that are coming due. Builds a subscriptions board or a tracker sheet. Use when the person asks "which subscriptions am I paying for", "what are my recurring charges", "did Netflix go up", "what bills are coming up", "what's due this month", or wants to cut regular costs. Do NOT use for a full spending breakdown (where-did-my-money-go) or whether there is enough cash to cover them (cash-flow-outlook).
---

# Subscriptions and bills

List what charges the person again and again, and what is about to fall due. Follow
[the Lever Books rules](references/lever-books.md).

## Non-negotiables

- Lever Books can't cancel anything. Point out what's worth a look; the person cancels with the
  provider.
- Call a charge recurring only when it repeats at a steady gap. One that has happened twice is
  "possibly recurring".

## Steps

1. **Find the candidates**: `summarise_bank_transactions` with `groupBy` `merchant` and
   `direction` `out` over the last year (`from` a year ago, `to` today). It returns the 100
   biggest merchants, so run it again with `maxAmount` 100 to catch small subscriptions. Keep
   merchants charged several times.
2. **Drill into each**: `search_bank_transactions` with `text` set to the merchant, `direction`
   `out` and the same year's `from` and `to`, for its dates and amounts.
3. **Read the pattern**: the gap between charges (weekly, fortnightly, monthly, quarterly,
   yearly, allowing a few days' slack), the usual amount and the last date. Work out the next
   expected date, the yearly cost, and whether the latest charge is higher than the ones before.
4. **Bills due**: `search_documents` with `type` `bill`. Keep those with a `dueOn` from today, and
   overdue ones with no matching payment in the bank.

## Output

Answer in chat in this shape:

```
‹Profile›: ‹n› recurring charges, about $‹yearly› a year ($‹monthly› a month).

| Merchant | How often | Usual amount | Next expected | A year |
| ‹sorted by yearly cost› |

Price rises: ‹merchant› went from $‹old› to $‹new› on ‹date›.
Worth a look: ‹ones not used lately, duplicates, small ones that add up›.

Bills due
| Supplier | Due | Amount |
```

Then offer one of:

- **The subscriptions board**: an HTML artifact from `assets/dashboard.html`, following
  [the subscriptions board brief](assets/subscriptions-board.md).
- **A tracker the person keeps**: a Claude Sheets spreadsheet with one row per charge (merchant,
  how often, amount, next date, yearly cost as a formula, and a Keep or Cancel dropdown), and a
  Source tab with the payments. Without Sheets, an .xlsx file.

## Gotchas

| Situation | What to do |
| --- | --- |
| A yearly subscription | It needs more than a year of history to repeat. Mark it "possibly yearly" |
| The merchant name varies (APPLE.COM/BILL, Apple Services) | Group by the clear brand, and say you grouped them |
| A bill that varies (power, phone) | Recurring with a varying amount. Show the average and the range |
| Rent, loan and card repayments | Recurring, but not subscriptions. List them in their own short group |
| A bill document still reading | Its amount or due date may be missing. Show "Couldn't read yet" |
