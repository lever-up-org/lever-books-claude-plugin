# Spending dashboard brief

Start from `dashboard.html` in this folder and fill its data block.

- **Title:** the insight, for example "You spent $4,211 in October, $230 more than September".
- **KPIs:** Spent (the headline, with its change), Came in, Net, Top category with its share.
- **Sections, in order:**
  1. `lines`: spending through the month as a running total, week by week, this period `actual`
     and the period before `compare` (dashed grey). Titled with how they compare, for example
     "October ran ahead of September from the second week". Each point is one
     `summarise_bank_transactions` call for the weeks so far, grouped by `category` and added up
     without transfers and card repayments.
  2. `bars`: the top eight categories, the comparison as the grey tick, then "Everything else".
     Titled with the biggest mover.
  3. `inout`: money in against money out for the last six months.
  4. `table`: the top ten merchants, with payments and total.
- **Footer:** what was left out (transfers, card repayments).
