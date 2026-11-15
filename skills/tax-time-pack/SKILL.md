---
name: tax-time-pack
description: Gets a Lever Books profile ready for tax time, for one Australian income year (1 July to 30 June) or, for a GST-registered business, one BAS quarter. Totals the spending that may be deductible, shows which payments have a possible receipt, totals the GST credits on purchase documents and lists purchases with no tax invoice, and lists what the accountant has asked for. Builds a Claude Docs pack and a Claude Sheets worksheet, marked prepared for your accountant, not tax advice. Use when the person asks "get me ready for tax time", "EOFY", "what can I claim", "which expenses have no receipt", "how much GST can I claim this quarter", "get my BAS ready", or "what does my accountant need for my return". Do NOT use for one request (answer-my-accountant) or for advice on what is allowed.
---

# Tax time pack

Gather the evidence so the accountant can do the return or the BAS. Follow
[the Lever Books rules](references/lever-books.md). Every Australian figure and date comes from
[the Australian tax figures](references/australian-tax.md).

## Non-negotiables

- Open every output with: "‹income year or quarter›. Prepared for your accountant, not tax
  advice."
- "May be deductible" means "worth asking your accountant about", never "you can claim this".
- A receipt match is only ever **Possible**: Claude compares amounts, dates and names, and never
  says a receipt is linked to a payment.
- GST comes from what a document says, never from a bank amount. Purchases only: Lever Books
  documents can't show which were sales, so the pack gives GST credits, not GST owed.
- Never estimate a working-from-home claim without the person's own record of hours. Ask for it.

## Steps

1. **Fix the period.** For a return, the income year: before October, the year just ended;
   otherwise ask. For a BAS, the quarter just ended. Name its due date from the tax figures.
2. **Totals by category**: `lever-books:summarise_bank_transactions` with `groupBy` `category` and
   `direction` `out` for the period.
3. **Choose the may-be-deductible categories** with the person:
   - their own finances, as an employee: work equipment (MERCHANDISE_ELECTRONICS,
     MERCHANDISE_OFFICE_SUPPLIES), phone and internet (RENT_AND_UTILITIES_TELEPHONE,
     RENT_AND_UTILITIES_INTERNET_AND_CABLE, the work share only), self-education
     (SERVICES_EDUCATION), donations (GOVERNMENT_AND_NON_PROFIT_DONATIONS), work travel;
   - a business or sole trader: everything spent running the business.
   For 2026–27 and later, flag the standard deduction for employees from the tax figures.
4. **Payments behind each** chosen category only: `lever-books:search_bank_transactions` with the exact
   `category` code, paging until done. A year of payments is many calls, and calls per day are
   limited, so work through the categories the person picked and stop to ask before the rest.
5. **Possible receipts**: `lever-books:search_documents` over the period (any `type`). A payment has a
   possible receipt when a document has the same amount, a date close before it and the same
   business name. List the payments with none, largest first.
6. **GST credits** (GST-registered business, one quarter at a time): `lever-books:get_document` for each
   purchase document in the quarter to read its `gst`. Total them, and list business payments with
   no document as "No tax invoice". For a large quarter, say how many documents there are and ask
   before opening them all.
7. **The accountant's asks**: `lever-books:list_requests`, open and with `state` `done`, keeping those about
   this return or BAS.

## Output

Answer in chat in this shape:

```
2025–26 income year (1 Jul 2025 to 30 Jun 2026). Prepared for your accountant, not tax advice.

| May be deductible | Total | Payments | Possible receipt |
| ‹category› | $‹x› | ‹n› | ‹n› |

No receipt found: ‹n› payments, $‹x›.
GST credits on purchase documents: $‹x› (‹n› documents). No tax invoice: ‹n› payments.
Your accountant asked for ‹n› things, ‹n› still open.
Still need from you: ‹hours worked from home, car logbook, private-use share›.
```

Then offer the pack:

- **A Claude Docs document** titled "Tax time pack, ‹period›, ‹profile›": the heading line above,
  a summary, a section per may-be-deductible category, the payments with no receipt, the
  accountant's checklist, what's still needed from the person, and where the figures came from.
  Export it before sharing. Without Docs, a .docx file.
- **A Claude Sheets worksheet** with a tab per category, a GST credits tab (supplier, date,
  amount, GST, "No tax invoice" flagged) with totals as formulas, and a Source tab with the rows
  the tools returned. Without Sheets, an .xlsx file.

## Gotchas

| Situation | What to do |
| --- | --- |
| The person asks "can I claim this?" | Explain what the ATO page in the tax figures says in general and say their accountant decides |
| Mixed work and private use (phone, car, home internet) | Show the full amount and ask for the work share. Never pick a percentage |
| A big purchase, such as a laptop | Flag it: it may be claimed at once or over several years. The accountant decides |
| A document shows a total but no GST | Show "GST not shown". Don't work it out from the total |
| The income year isn't over | Say it covers 1 July to today |
| No bank data for part of the period | Say which months Lever Books holds and total only those |
