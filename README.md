# Lever Books for Claude

Your receipts and your money, in Claude. Ask a question and get the answer from your own Lever
Books: where your money went, what you're paying for every month, whether you'll make it to
payday, and what you need for tax time. When you want more than an answer, Claude turns it into a
dashboard, a spreadsheet or a document. It works for your own finances, as a sole trader or for a
small business, in Australia.

Photograph a receipt in the Lever Books app, or give one to Claude, and it's there for Claude to find.

**Claude can read your Lever Books and add a receipt you give it. Nothing else.** It can't pay a
bill, cancel a subscription, edit a transaction or change a document. Claude drafts, you decide.

## What you can do

Spending, subscriptions and payday answers come from your bank. **Bank connection is coming soon:
for now, Claude works from your receipts and documents** and tells you when an answer needs your
bank.

| Ask Claude | What you get |
| --- | --- |
| "Where did my money go last month?" | Spending by category and shop against the month before, the biggest changes called out, and a spending dashboard you can open as a Claude artifact |
| "Find the receipt for the laptop I bought in March." | The receipt from Lever Books, with the supplier, date, amount and GST |
| "Add this receipt to Lever Books." (with a photo or PDF) | The receipt filed in your Lever Books documents, where Lever Books reads its supplier, amount and GST |
| "Which subscriptions am I paying for, and did Netflix go up?" | Every regular charge, what it costs a year and any price rise, as a subscriptions board or a tracker in Claude Sheets with Keep or Cancel beside each |
| "Will I make it to payday? Can I cover rent next week?" | Today's balance, what's due before payday and the lowest point on the way, with a cash flow dashboard |
| "Give me a check-in, and schedule it every Monday." | A one-page money check-in with three things to do, run every week by Claude and kept together in a Claude project |
| "Get me ready for EOFY. Which expenses have no receipt?" | The spending that may be deductible, the payments with no receipt, and a tax time pack as a Claude Docs document and a Claude Sheets worksheet |
| "How much GST can I claim this quarter?" | The GST on your purchase receipts and invoices, and the purchases with no tax invoice, ready for your BAS |

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

## Use it with Claude Code

**Claude Code can't sign in to Lever Books directly yet.** The sign-in accepts only claude.ai's
callback, so use Lever Books from claude.ai, Claude desktop or Claude mobile.

## Skills

Claude picks the right skill from what you ask. You don't need to name one.

| Skill | What you get |
| --- | --- |
| **money-check-in** | Start here. A one-page weekly or monthly check-in: balances, money in and out, anything unusual, what's coming up and three things to do |
| **where-did-my-money-go** | Where your money went, by category and shop, against the period before, with a spending dashboard |
| **subscriptions-and-bills** | Your subscriptions and regular charges, what they cost a year, any price rises and the bills coming due |
| **cash-flow-outlook** | Whether your money lasts until payday or a big bill, and the lowest point on the way |
| **tax-time-pack** | Everything for tax time or your BAS in one place: spending that may be deductible, payments with no receipt and GST credits on your purchases |

Australian tax figures and dates sit in
[skills/tax-time-pack/references/australian-tax.md](skills/tax-time-pack/references/australian-tax.md),
copied into each skill that uses them, each with its Australian Taxation Office source. They are
checked again every 1 July.

## Data and privacy

Lever Books sends Claude only what a tool returns for the question you asked. That data then sits
in your Claude account, under Anthropic's terms, outside Australia. Dashboards and documents hold
your financial details, so they stay private unless you share them. Read Lever Up's
[privacy policy](https://leverup.com.au/privacy) and [terms](https://leverup.com.au/terms).

Claude can make mistakes. Check figures against your bank or Lever Books before you rely on them.
This plugin gives general information, not financial, tax or legal advice.

## Support

Email [hello@leverup.com.au](mailto:hello@leverup.com.au).

## Licence

MIT. See [LICENSE](LICENSE).
