This is the HTML reference build for this skill's dashboard artifact. Start from the HTML below and fill its data block.

```html
<!doctype html>
<!--
  1. Replace the JSON in #lever-data with the figures the Lever Books tools returned.
  2. Keep the sections the skill's brief names, in its order. Each title is an insight sentence.
  Names from outside Lever (merchants, suppliers) are set as text, never as HTML, and in the JSON
  every < is written \u003c so a name can't end the data block early.
  No external scripts: charts are inline SVG and CSS.
-->
<html lang="en-AU">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Lever Books dashboard</title>
<style>
  :root {
    --bg: #FAFAFA; --card: #FFFFFF; --text: #0A0A0A; --muted: #67676F; --border: #E4E4E7;
    --accent: #5B5DF1; --accent-text: #3F42EE; --compare: #91919D;
    --in: #0072B2; --out: #D55E00; --warn: #B45309;
    --display: system-ui, -apple-system, 'Segoe UI', Roboto, sans-serif;
    --body: system-ui, -apple-system, 'Segoe UI', Roboto, sans-serif;
    --mono: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace;
  }
  @media (prefers-color-scheme: dark) {
    :root:not([data-theme="light"]) {
      --bg: #0A0A0F; --card: #101013; --text: #FAFAFA; --muted: #9797A5; --border: #27272A;
      --accent-text: #8E90F5; --in: #56B4E9; --out: #E69F00; --warn: #F59E0B;
    }
  }
  :root[data-theme="dark"] {
    --bg: #0A0A0F; --card: #101013; --text: #FAFAFA; --muted: #9797A5; --border: #27272A;
    --accent-text: #8E90F5; --in: #56B4E9; --out: #E69F00; --warn: #F59E0B;
  }
  * { box-sizing: border-box; }
  body { margin: 0; background: var(--bg); color: var(--text); font: 15px/1.5 var(--body); }
  main { max-width: 960px; margin: 0 auto; padding: 24px 16px 48px; }
  h1, h2 { font-family: var(--display); font-weight: 600; margin: 0; }
  h1 { font-size: 26px; letter-spacing: -0.01em; }
  h2 { font-size: 17px; }
  .note, .sub, footer { color: var(--muted); font-size: 13px; }
  .num { font-family: var(--mono); font-variant-numeric: tabular-nums; }
  .banner { color: var(--accent-text); font-weight: 600; font-size: 13px; margin-bottom: 6px; }
  header { margin-bottom: 20px; }
  .kpis { display: grid; gap: 12px; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); }
  .card { background: var(--card); border: 1px solid var(--border); border-radius: 12px; padding: 16px; }
  .kpi .label { color: var(--muted); font-size: 13px; }
  .kpi .value { font-size: 24px; font-weight: 500; margin: 2px 0; }
  .kpi .value.text { font-size: 17px; font-weight: 600; }
  .kpi.headline { grid-column: 1 / -1; }
  .kpi.headline .value { font-size: 40px; color: var(--accent-text); }
  .kpi .delta { font-size: 13px; color: var(--muted); }
  section.card { margin-top: 16px; }
  section .note { margin: 2px 0 14px; }
  .bars { display: grid; gap: 10px; }
  .bar-row { display: grid; grid-template-columns: minmax(96px, 30%) 1fr auto; gap: 10px; align-items: center; }
  .bar-label { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  .track { position: relative; height: 12px; }
  .fill { height: 100%; border-radius: 3px; background: var(--accent); }
  .tick { position: absolute; top: -3px; width: 2px; height: 18px; background: var(--compare); }
  svg.chart { width: 100%; height: auto; display: block; overflow: visible; }
  svg text { font: 12px var(--body); fill: var(--muted); }
  svg text.strong { fill: var(--text); font-weight: 600; }
  .cal { display: grid; grid-template-columns: repeat(7, 1fr); gap: 6px; }
  .day { border: 1px solid var(--border); border-radius: 8px; padding: 6px; min-height: 72px; font-size: 12px; }
  .day .d { color: var(--muted); }
  .chip { display: block; margin-top: 4px; padding: 2px 6px; border-radius: 6px; background: var(--bg); overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  .chip.out { border-left: 3px solid var(--out); }
  .chip.in { border-left: 3px solid var(--in); }
  .table-wrap { overflow-x: auto; }
  table { width: 100%; border-collapse: collapse; font-size: 14px; }
  th, td { text-align: left; padding: 8px 6px; border-bottom: 1px solid var(--border); }
  th { color: var(--muted); font-weight: 500; font-size: 13px; }
  td.num, th.num { text-align: right; }
  .pill { display: inline-block; padding: 1px 8px; border-radius: 999px; font-size: 12px; border: 1px solid var(--border); }
  .pill.warn { color: var(--warn); border-color: var(--warn); }
  .empty { color: var(--muted); padding: 24px 0; text-align: center; }
  footer { margin-top: 24px; }
  @media (max-width: 600px) {
    .bar-row { grid-template-columns: 1fr auto; }
    .bar-row .track { grid-column: 1 / -1; }
    .cal { grid-template-columns: repeat(4, 1fr); }
  }
</style>
</head>
<body>
<main>
  <header>
    <div class="banner" id="banner" hidden></div>
    <h1 id="title"></h1>
    <div class="sub" id="subtitle"></div>
  </header>
  <div class="kpis" id="kpis"></div>
  <div id="sections"></div>
  <footer id="footer"></footer>
</main>

<!--
  Amounts are numbers in dollars; bars and inout draw sizes, so their sign doesn't matter. Dates are
  YYYY-MM-DD. Section types: bars, lines, inout, calendar, table. Leave out what you don't use.
  lines: each series' style is "actual", "compare" (dashed grey) or "forecast" (dashed); optional
  band {low, high}, markLowest, and marks [{date, text}].
-->
<script type="application/json" id="lever-data">
{
  "title": "You spent $4,211 in October, $230 more than September",
  "banner": null,
  "profile": "My finances",
  "period": "1 to 31 Oct 2026",
  "comparison": "1 to 30 Sep 2026",
  "accounts": "Everyday, Credit card",
  "asOf": "2026-11-15T09:02:00+11:00",
  "kpis": [
    { "label": "Spent in October", "value": 4210.55, "previous": 3980.10, "headline": true },
    { "label": "Came in", "value": 5050.00, "previous": 5050.00 },
    { "label": "Net", "value": 839.45, "previous": 1069.90 },
    { "label": "Top category", "text": "Food and drink, 19%" }
  ],
  "sections": [
    {
      "type": "lines",
      "title": "On pace for $4,200, close to September",
      "note": "Spending so far this month against the same days last month.",
      "series": [
        { "label": "October", "style": "actual", "points": [{ "date": "2026-10-01", "value": 120 }, { "date": "2026-10-15", "value": 2010 }, { "date": "2026-10-31", "value": 4210.55 }] },
        { "label": "September", "style": "compare", "points": [{ "date": "2026-10-01", "value": 90 }, { "date": "2026-10-15", "value": 1880 }, { "date": "2026-10-31", "value": 3980.10 }] }
      ]
    },
    {
      "type": "bars",
      "title": "Food and drink up $210 on September",
      "note": "Top categories this month; the grey tick is September.",
      "items": [
        { "label": "Food and drink", "value": 820.40, "previous": 610.00 },
        { "label": "Transportation", "value": 512.75, "previous": 498.20 }
      ]
    },
    {
      "type": "table",
      "title": "Two supermarkets made up most of it",
      "columns": [{ "key": "name", "label": "Merchant" }, { "key": "count", "label": "Payments", "num": true }, { "key": "total", "label": "Total", "money": true }],
      "rows": [{ "name": "Example Supermarket", "count": 6, "total": 412.30 }]
    }
  ],
  "footer": "Transfers between your accounts and card repayments are left out."
}
</script>
<script>
  const data = JSON.parse(document.getElementById('lever-data').textContent);
  const money = new Intl.NumberFormat('en-AU', { style: 'currency', currency: 'AUD' });
  const whole = new Intl.NumberFormat('en-AU', { style: 'currency', currency: 'AUD', maximumFractionDigits: 0 });
  const signed = new Intl.NumberFormat('en-AU', { style: 'currency', currency: 'AUD', maximumFractionDigits: 0, signDisplay: 'always' });
  const dayMonth = new Intl.DateTimeFormat('en-AU', { day: 'numeric', month: 'short' });
  const fullDate = new Intl.DateTimeFormat('en-AU', { day: 'numeric', month: 'short', year: 'numeric' });
  const stamp = new Intl.DateTimeFormat('en-AU', { day: 'numeric', month: 'short', hour: 'numeric', minute: '2-digit' });
  const NS = 'http://www.w3.org/2000/svg';
  const asDate = (s) => new Date(`${s}T00:00:00`);
  const el = (tag, cls, text) => { const n = document.createElement(tag); if (cls) n.className = cls; if (text != null) n.textContent = text; return n; };
  const svgEl = (tag, attrs, text) => { const n = document.createElementNS(NS, tag); for (const [k, v] of Object.entries(attrs)) n.setAttribute(k, v); if (text != null) n.textContent = text; return n; };
  const cents = (v) => v == null ? 'Not in Lever Books' : money.format(v);

  document.getElementById('title').textContent = data.title;
  document.getElementById('subtitle').textContent = [
    data.profile, data.period, data.comparison && `compared with ${data.comparison}`,
    data.accounts && `Accounts: ${data.accounts}`, data.asOf && `Updated ${stamp.format(new Date(data.asOf))}`,
  ].filter(Boolean).join(' · ');
  if (data.banner) { const b = document.getElementById('banner'); b.textContent = data.banner; b.hidden = false; }

  for (const k of data.kpis ?? []) {
    const card = el('div', `card kpi${k.headline ? ' headline' : ''}`);
    card.append(el('div', 'label', k.label), k.text ? el('div', 'value text', k.text) : el('div', 'value num', k.value == null ? 'Not in Lever Books' : whole.format(k.value)));
    if (k.previous != null && k.value != null) {
      const d = Math.round(k.value - k.previous);
      card.append(el('div', 'delta', d === 0 ? 'Same as last period' : `${signed.format(d).replace('-', '−')} on last period`));
    }
    document.getElementById('kpis').append(card);
  }

  const render = {
    bars(s, body) {
      const abs = Math.abs;
      const max = Math.max(...s.items.flatMap((b) => [abs(b.value), abs(b.previous ?? 0)])) || 1;
      const wrap = el('div', 'bars');
      for (const b of s.items) {
        const row = el('div', 'bar-row'); const track = el('div', 'track'); const fill = el('div', 'fill');
        fill.style.width = `${(abs(b.value) / max) * 100}%`; track.append(fill);
        if (b.previous != null) { const t = el('div', 'tick'); t.style.left = `${(abs(b.previous) / max) * 100}%`; track.append(t); }
        row.append(el('div', 'bar-label', b.label), track, el('div', 'num', whole.format(abs(b.value))));
        wrap.append(row);
      }
      body.append(wrap);
    },
    lines(s, body) {
      const W = 640, H = 240, L = 8, R = 120, T = 16, B = 28;
      const series = s.series.filter((ser) => ser.points.length);
      const pts = [...series.flatMap((x) => x.points), ...(s.band?.low ?? []), ...(s.band?.high ?? [])];
      const ts = pts.map((p) => asDate(p.date).getTime()); const vs = pts.map((p) => p.value);
      const t0 = Math.min(...ts), t1 = Math.max(...ts), v0 = Math.min(0, ...vs), v1 = Math.max(0, ...vs);
      const x = (d) => L + ((asDate(d).getTime() - t0) / (t1 - t0 || 1)) * (W - L - R);
      const y = (v) => T + (1 - (v - v0) / (v1 - v0 || 1)) * (H - T - B);
      const path = (p) => p.map((q, i) => `${i ? 'L' : 'M'}${x(q.date).toFixed(1)},${y(q.value).toFixed(1)}`).join('');
      const svg = svgEl('svg', { class: 'chart', viewBox: `0 0 ${W} ${H}`, role: 'img', 'aria-label': s.title });
      svg.append(svgEl('line', { x1: L, x2: W - R, y1: y(0), y2: y(0), stroke: 'var(--border)' }));
      if (s.band) svg.append(svgEl('path', { d: `${path(s.band.high)}${path([...s.band.low].reverse()).replace('M', 'L')}Z`, fill: 'var(--accent)', 'fill-opacity': '0.12' }));
      const style = { actual: ['var(--accent)', ''], compare: ['var(--compare)', '5 4'], forecast: ['var(--accent)', '6 4'] };
      const taken = [];
      for (const ser of series) {
        const [stroke, dash] = style[ser.style] ?? style.actual;
        svg.append(svgEl('path', { d: path(ser.points), fill: 'none', stroke, 'stroke-width': 2.5, 'stroke-dasharray': dash, 'stroke-linejoin': 'round' }));
        // Label each line at its end, nudged down so two ends never overlap.
        const last = ser.points.at(-1);
        let ly = y(last.value) + 4;
        while (taken.some((t) => Math.abs(t - ly) < 14)) ly += 14;
        taken.push(ly);
        svg.append(svgEl('text', { x: x(last.date) + 6, y: ly, class: ser.style === 'compare' ? '' : 'strong' }, `${ser.label} ${whole.format(last.value)}`));
      }
      for (const m of s.marks ?? []) {
        svg.append(svgEl('line', { x1: x(m.date), x2: x(m.date), y1: T, y2: H - B, stroke: 'var(--border)', 'stroke-dasharray': '2 3' }));
        svg.append(svgEl('text', { x: x(m.date) + 3, y: T + 10 }, m.text));
      }
      const own = series.filter((ser) => ser.style !== 'compare').flatMap((ser) => ser.points);
      if (s.markLowest && own.length) {
        const low = own.reduce((a, b) => (b.value < a.value ? b : a));
        svg.append(svgEl('circle', { cx: x(low.date), cy: y(low.value), r: 5, fill: 'var(--card)', stroke: 'var(--out)', 'stroke-width': 2.5 }));
        svg.append(svgEl('text', { x: x(low.date), y: y(low.value) + 20, 'text-anchor': 'middle', class: 'strong' }, `Lowest ${whole.format(low.value)}, ${dayMonth.format(asDate(low.date))}`));
      }
      svg.append(svgEl('text', { x: L, y: H - 6 }, dayMonth.format(new Date(t0))));
      svg.append(svgEl('text', { x: W - R, y: H - 6, 'text-anchor': 'end' }, dayMonth.format(new Date(t1))));
      body.append(svg);
    },
    inout(s, body) {
      const W = 640, H = 200, B = 24, T = 16; const n = s.items.length; const gw = W / n;
      const max = Math.max(...s.items.flatMap((i) => [Math.abs(i.moneyIn), Math.abs(i.moneyOut)])) || 1;
      const h = (v) => (Math.abs(v) / max) * (H - B - T);
      const svg = svgEl('svg', { class: 'chart', viewBox: `0 0 ${W} ${H}`, role: 'img', 'aria-label': s.title });
      s.items.forEach((it, i) => {
        const bw = Math.min(28, gw / 3), cx = i * gw + gw / 2;
        svg.append(svgEl('rect', { x: cx - bw - 1, y: H - B - h(it.moneyIn), width: bw, height: h(it.moneyIn), fill: 'var(--in)', rx: 2 }, null));
        svg.append(svgEl('rect', { x: cx + 1, y: H - B - h(it.moneyOut), width: bw, height: h(it.moneyOut), fill: 'var(--out)', rx: 2 }, null));
        svg.append(svgEl('text', { x: cx, y: H - 6, 'text-anchor': 'middle' }, it.label));
      });
      const lastItem = s.items.at(-1);
      svg.append(svgEl('text', { x: W, y: T, 'text-anchor': 'end', class: 'strong' }, `+${whole.format(Math.abs(lastItem.moneyIn))} in · −${whole.format(Math.abs(lastItem.moneyOut))} out`));
      body.append(svg);
    },
    calendar(s, body) {
      const grid = el('div', 'cal'); const start = asDate(s.start);
      for (let i = 0; i < (s.days ?? 14); i++) {
        const d = new Date(start); d.setDate(start.getDate() + i);
        const key = `${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, '0')}-${String(d.getDate()).padStart(2, '0')}`;
        const cell = el('div', 'day'); cell.append(el('div', 'd', dayMonth.format(d)));
        for (const it of s.items.filter((x) => x.date === key)) {
          const chip = el('span', `chip ${it.kind === 'in' ? 'in' : 'out'} num`, `${it.kind === 'in' ? '+' : '−'}${whole.format(Math.abs(it.amount))} ${it.label}`);
          chip.title = `${it.label} ${money.format(Math.abs(it.amount))}`; cell.append(chip);
        }
        grid.append(cell);
      }
      body.append(grid);
    },
    table(s, body) {
      const wrap = el('div', 'table-wrap'); const table = el('table'); const head = el('tr');
      for (const c of s.columns) head.append(el('th', c.num || c.money ? 'num' : '', c.label));
      table.append(head);
      for (const r of s.rows) {
        const tr = el('tr');
        for (const c of s.columns) {
          const v = r[c.key];
          if (c.pill) { const td = el('td'); if (v) td.append(el('span', `pill${r.warn ? ' warn' : ''}`, v)); tr.append(td); }
          else if (c.date) tr.append(el('td', '', v ? fullDate.format(asDate(v)) : ''));
          else if (c.money) tr.append(el('td', 'num', cents(v)));
          else tr.append(el('td', c.num ? 'num' : '', v ?? ''));
        }
        table.append(tr);
      }
      wrap.append(table); body.append(wrap);
    },
  };

  for (const s of data.sections ?? []) {
    const card = el('section', 'card');
    card.append(el('h2', '', s.title));
    if (s.note) card.append(el('div', 'note', s.note));
    const empty = !(s.items?.length || s.rows?.length || s.series?.some((x) => x.points.length));
    if (empty) card.append(el('div', 'empty', s.empty ?? 'Nothing in Lever Books for this period.'));
    else render[s.type](s, card);
    document.getElementById('sections').append(card);
  }

  document.getElementById('footer').textContent = [
    data.footer, 'Data from Lever Books. All amounts in AUD.', 'General information, not financial advice.',
  ].filter(Boolean).join(' ');
</script>
</body>
</html>
```
