---
name: money-check-in
description: Tells a person how their money is tracking, in a one-page weekly or monthly check-in from Lever Books, and is the place to start. Covers balances, money in and out against last time, new or changed regular charges, anything unusual, and the bills and BAS coming up, ending with three things to do. On first use it shows what Lever Books holds with one real check-in. Builds a check-in dashboard and can run every week. Use when the person says "how am I tracking", "weekly recap", "Monday money brief", "give me a check-in", "get me started", "what can you do with Lever Books", or has just connected Lever Books. Do NOT use for a deep dive on spending (where-did-my-money-go), cash before payday (cash-flow-outlook) or tax (tax-time-pack).
---

# Money check-in

One short page that says how things stand and what needs doing. Follow
[the Lever Books rules](references/lever-books.md).

If the bank tools aren't on this connection, say Lever Books has no bank data here yet and offer
what its receipts and documents show instead.

## Non-negotiables

- One page. Each part is a sentence or a short table.
- "Unusual" means something the person can check: a charge well above its usual amount, a merchant
  they haven't paid before, a duplicate payment, a fee. Never a judgement about their spending.
- Three things to do, at most, each one the person can act on.

## Steps

1. **First time?** If this is the person's first check-in, say in one line that Claude can read
   their Lever Books and add a receipt they give it, but can't move money or change anything else,
   then run the steps below on their real data. That one real check-in is the introduction. Ask
   nothing first.
2. **Fix the period**: the last 7 days (weekly, the default) or the last full month (monthly), and
   the period before it.
3. **Fetch in one batch**:
   - `lever-books:list_bank_accounts` for balances and feed status: add up everyday and savings accounts as
     available, and show cards and loans separately as owed;
   - `lever-books:summarise_bank_transactions` grouped by `category`, for both periods, leaving out the
     transfer and card repayment codes;
   - `lever-books:summarise_bank_transactions` grouped by `merchant`, then `lever-books:search_bank_transactions` with
     `text` only for merchants that look new or changed;
   - `lever-books:search_documents` with `type` `bill` for bills coming due.
4. **Compare**: money in, out and net against last time; the top three categories and their
   change.
5. **Coming up** before the next check-in: bills, regular charges, and for a GST-registered
   business the next BAS due date from [the Australian tax figures](references/australian-tax.md).
6. **Pick three things to do** from what you found: a bill due, a price rise, an unusual charge to check.

## Output

Answer in chat in this shape:

```
‹Profile›, week of ‹dates›

Balances: $‹available› available across ‹n› accounts (as at ‹time›).
This week: $‹out› out, $‹in› in (‹+ or −›$‹x› on last week).
Most went on: ‹category› $‹x›, ‹category› $‹x›, ‹category› $‹x›.

Worth a look
- ‹unusual item or new charge›

Coming up
- ‹date›: ‹bill or charge› $‹x›

This week
1. ‹action›
2. ‹action›
3. ‹action›
```

Then offer the check-in dashboard: an HTML artifact, starting from the HTML in `assets/dashboard.md`, following
[the check-in dashboard brief](assets/check-in-dashboard.md).

At the end of the first check-in, and only once, offer:

- "Want this every Monday? Say *schedule this weekly*." The person confirms the schedule in
  Claude. Schedules run hourly, daily, weekly or on weekdays, so a monthly check-in or a BAS
  reminder is a weekly run that first checks the date and only reports when one is due.
- Keeping it in a Claude project named Lever Books, so the check-ins sit together.

## Gotchas

| Situation | What to do |
| --- | --- |
| A quiet week | Say so in one line and keep the page short |
| Pay landed this week but not last | Say the swing in net is pay timing |
| Nothing in Lever Books yet | Say what's missing (a bank account, receipts) and that it is added in Lever Books |
| A bank feed stopped | Lead with it: the check-in is incomplete until it's reconnected in Lever Books |
| The person has several profiles | One connection reads one. To check another, they connect again and choose it |
