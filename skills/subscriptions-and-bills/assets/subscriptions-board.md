# Subscriptions board brief

Start from the HTML in `dashboard.md` in this folder and fill its data block.

- **Title:** the total as an insight, for example "$184 a month on 11 subscriptions".
- **KPIs:** Per month (the headline), Per year, Due in the next fortnight, Price rises.
- **Sections, in order:**
  1. `calendar`: the next 14 days from today, each charge and bill as a chip on its date, money
     out with a − sign.
  2. `table`: merchant, how often, next date, amount, yearly cost and a status pill ("Went up",
     "Possibly duplicate"), sorted by yearly cost. Mark rises with `warn`.
  3. `bars`: yearly cost per subscription, largest first.
- **Footer:** how a charge was judged recurring (it repeats at a steady gap).
