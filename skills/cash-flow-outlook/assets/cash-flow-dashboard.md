# Cash flow dashboard brief

Start from `dashboard.html` in this folder and fill its data block.

- **Title:** the lowest point as an insight, for example "Lowest $612 on 26 Nov, two days before
  payday".
- **KPIs:** Lowest point (the headline), Available today, Due out before payday, Coming in.
- **Sections, in order:**
  1. `lines` with `markLowest`: the daily balance, the last 30 days as `actual` and the days ahead
     as `forecast` (dashed), the lowest point marked, and `marks` for payday, rent and big bills.
  2. `calendar`: the next 14 days, each known item as a chip, money in with + and out with −.
  3. `table`: each known item in date order with the running balance; unread bills marked `warn`.
- **Footer:** "Known and regular items only; everyday spending isn't included."
