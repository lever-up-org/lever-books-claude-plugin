---
name: cash-flow-outlook
description: Shows whether a Lever Books profile's money lasts until the next payday or a big bill. Starts from today's balance, adds the regular outgoings and the bills and payment requests coming due, and finds the lowest point before money next comes in. Builds a cash flow dashboard. Use when the person asks "will I make it to payday", "can I cover rent next week", "can I pay my BAS", "am I going to be short this month", or "how much can I spend until payday". Do NOT use for past spending (where-did-my-money-go) or a list of subscriptions (subscriptions-and-bills).
---

# Cash flow outlook

Answer "will I have enough" from real balances and what is already known to be coming. Follow
[the Lever Books rules](references/lever-books.md).

If the bank tools aren't on this connection, say Lever Books has no bank data here yet and offer
what its documents and requests show instead.

## Non-negotiables

- Start from the real balance. If no account has one, say "no opening balance" and show only what
  is coming in and out.
- Show only what is known or regular: the balance, regular payments, bills and requests with due
  dates. Don't model a range or guess at other spending; say it isn't included.
- State what you assumed under the answer.

## Steps

1. **Balance today**: `lever-books:list_bank_accounts`. Add up the available balances of everyday and savings
   accounts; show cards and loans separately as owed. Note each balance's date.
2. **Regular money in and out**: `lever-books:summarise_bank_transactions` with `groupBy` `merchant` over the
   last three months, then `lever-books:search_bank_transactions` with `text` for each merchant that repeats
   (pay, rent, loan repayments, subscriptions) to find its day and amount.
3. **Bills due**: `lever-books:search_documents` with `type` `bill`, keeping those with a `dueOn` before the
   end date.
4. **Payment requests**: `lever-books:list_requests`, then `lever-books:get_request` to see which ask for a payment, with
   their `dueOn`. The request doesn't give the amount: list it as "amount not in Lever Books" and
   ask the person, rather than putting a figure into the walk.
5. **Tax dates**: for a GST-registered business, add the next BAS due date from
   [the Australian tax figures](references/australian-tax.md). Its amount comes from the person or
   their accountant; never invent one.
6. **Walk the days** from today to the next payday (or the date the person asked about): the
   balance, minus each known outgoing on its day, plus each known payment in. Find the lowest
   point and its date.

## Output

Answer in chat in this shape:

```
‹Profile›: $‹balance› available today (as at ‹date›).

Before ‹payday or date›: $‹out› is due to go out and $‹in› to come in.
Lowest point: $‹x› on ‹date›, ‹before or after› ‹the big item›.

| Date | Item | In or out | Amount | Balance after |
| ‹each known item in date order› |

Not included: everyday spending such as groceries and fuel. Last month that was about $‹x›.
```

Then offer the cash flow dashboard: an HTML artifact, starting from the HTML in `assets/dashboard.md`, following
[the cash flow dashboard brief](assets/cash-flow-dashboard.md).

## Gotchas

| Situation | What to do |
| --- | --- |
| Pay is irregular (sole trader, casual work) | Ask when the next payment is expected rather than guessing a payday |
| An account's bank feed is stopped | Its balance is old. Show its date and leave it out of "available today" unless the person says otherwise |
| Money moves between the person's own accounts | Not money in or out. Leave it out |
| A bill document still reading | Its amount may be missing. List it as "Couldn't read yet" |
