# Lever Books for Claude

- "Where did my money go last month?"
- "Will I make it to payday?"
- "Get me ready for tax time."
- "What does my accountant still need from me?"

Ask Claude questions like these and get answers from your own Lever Books: your bank accounts and
transactions, the receipts and documents you've added, and what your accountant is waiting on. It
works for your own finances and for a business, and turns the answer into a dashboard, a Claude
Sheets worksheet or a Claude Docs pack when you want one.

**Claude can read your Lever Books. It can't move money or change anything.** There is no tool
that pays, edits a transaction or replies to your accountant: Claude drafts, you send.

## Install

**From the Claude Directory.** Find Lever Books, install the plugin, then open its Connectors tab
and press Connect. Sign in to Lever Books, choose the profile Claude may read, and allow it.

**As a custom connector.** In Claude, go to Customize › Connectors, add a custom connector named
Lever Books with the URL `https://books.leverup.com.au/api/mcp`, then connect and sign in as above.
You get the data without the skills.

Skills need a paid Claude plan with code execution and file creation turned on. Each connection
reads one profile. You can see every call Claude made, and stop sharing at any time, in Lever Books
under Account › your profile › Connected AI apps. Business owners turn Claude on for the business
first, in the same place.

## Skills

| Skill | What it does |
| --- | --- |
| money-check-in | A one-page weekly or monthly check-in with three things to do. Start here: the first one shows what Lever Books holds |
| where-did-my-money-go | Spending by category and merchant against the period before, with a spending dashboard |
| subscriptions-and-bills | Your recurring charges, their yearly cost and price rises, and the bills coming due |
| cash-flow-outlook | Your balance, what's due before payday and the lowest point on the way |
| tax-time-pack | The income year's may-be-deductible spending, payments with no receipt, GST credits on purchases and your accountant's checklist |
| answer-my-accountant | Works through each open request: the evidence that answers it and a reply to send |

Australian tax figures and dates sit in
[skills/tax-time-pack/references/australian-tax.md](skills/tax-time-pack/references/australian-tax.md),
copied into each skill that uses them, each with its Australian Taxation Office source. They are
checked again every 1 July.

## Data and privacy

Lever Books sends Claude only what a tool returns for the question you asked. That data then sits
in your Claude account, under Anthropic's terms, outside Australia. Dashboards and documents hold
your bank data, so they stay private unless you share them. Read Lever Up's
[privacy policy](https://leverup.com.au/privacy) and [terms](https://leverup.com.au/terms).

Claude can make mistakes. Check figures against your bank or Lever Books before you rely on them.
This plugin gives general information, not financial, tax or legal advice. Anything about tax is
prepared for your accountant.

## Support

Email [hello@leverup.com.au](mailto:hello@leverup.com.au).

## Licence

MIT. See [LICENSE](LICENSE).
