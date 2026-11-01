<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Flip 7 Strategy Lab</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Bungee&amp;family=Barlow+Semi+Condensed:wght@400;500;600;700&amp;display=swap" rel="stylesheet">
<style>
:root {
  /* Layout: a game table with a side rail (controls, advisor, card count, log). Cards are paper tiles on an ink-blue table. */
  --bg: #10141c;
  --surface: #171d28;
  --surface-2: #1f2735;
  --line: #2d3748;
  --ink: #ede7d7;
  --muted: #9ba5b8;
  --accent: #f3b33e;
  --paper: #f5efe0;
  --card-ink: #1a202d;
  --good: #4cbc92;
  --bad: #ec5a4f;
  --frost: #8acbf2;
  --flip: #f39446;
  --second: #e8678a;
  --brick: #c4553e;
  --accent-ink: #f3b33e;
  --frost-ink: #8acbf2;
  --glow: #1d2738;
  --card-shadow: 0 2px 0 rgba(0, 0, 0, 0.45);
  --display: 'Bungee', 'Arial Black', Impact, system-ui, sans-serif;
  --body: 'Barlow Semi Condensed', 'Barlow', 'Arial Narrow', 'Helvetica Neue', Arial, sans-serif;
  color-scheme: dark;
}
/* Light mode: toggled by a class on <html>, so it applies live without touching the game */
html.light {
  --bg: #e6e9ef;
  --surface: #ffffff;
  --surface-2: #eef1f5;
  --line: #d2d8e2;
  --ink: #1b2130;
  --muted: #5a6476;
  --accent: #f1aa2f;
  --accent-ink: #8f5703;
  --paper: #fffaf0;
  --good: #1c8a5e;
  --bad: #cc3a2f;
  --frost: #86c6ee;
  --frost-ink: #23699c;
  --glow: #ffffff;
  --card-shadow: 0 0 0 1px #d9cfb6, 0 2px 0 rgba(30, 35, 50, 0.16);
  color-scheme: light;
}

* { box-sizing: border-box; }
html, body { margin: 0; }
body {
  background: radial-gradient(1200px 500px at 50% -120px, var(--glow) 0%, transparent 70%), var(--bg);
  color: var(--ink);
  font-family: var(--body);
  font-size: 16px;
  line-height: 1.4;
  min-height: 100%;
}
h2, h3 { text-wrap: balance; margin: 0 0 8px; }
h2 { font-family: var(--display); font-weight: 400; font-size: 20px; letter-spacing: 0.02em; }
h3 { font-size: 13px; text-transform: uppercase; letter-spacing: 0.12em; color: var(--muted); font-weight: 600; }
p { margin: 0 0 10px; }
b { font-weight: 700; }
button, input, select, textarea { font: inherit; color: inherit; }
select { max-width: 100%; }
:focus-visible { outline: 2px solid var(--accent); outline-offset: 2px; }
.muted { color: var(--muted); }
.lede { color: var(--muted); max-width: 62ch; }
.fine { color: var(--muted); font-size: 13px; margin-top: 18px; }

/* ---------- header ---------- */
.top {
  display: flex; align-items: center; justify-content: space-between; gap: 16px; flex-wrap: wrap;
  padding: 14px 20px; border-bottom: 1px solid var(--line); position: relative;
}
.brand { display: flex; align-items: baseline; gap: 14px; flex-wrap: wrap; }
.mark { font-family: var(--display); font-size: 30px; line-height: 1; letter-spacing: 0.01em; }
.mark .seven { color: var(--accent-ink); margin-left: 2px; }
.sub { text-transform: uppercase; letter-spacing: 0.22em; font-size: 13px; color: var(--muted); font-weight: 600; }
.tabs { display: flex; gap: 4px; flex-wrap: wrap; }
.topright { display: flex; align-items: center; gap: 10px 14px; flex-wrap: wrap; }
.tab-btn {
  background: none; border: 0; border-bottom: 2px solid transparent; padding: 8px 12px; cursor: pointer;
  color: var(--muted); font-weight: 600; font-size: 15px; letter-spacing: 0.02em;
}
.tab-btn:hover { color: var(--ink); }
.tab-btn[aria-selected="true"] { color: var(--ink); border-bottom-color: var(--accent-ink); }

main { max-width: 1360px; margin: 0 auto; padding: 16px 20px 40px; }
.panel { background: var(--surface); border: 1px solid var(--line); border-radius: 12px; padding: 14px 16px; }

/* ---------- forms ---------- */
.fld { display: grid; gap: 4px; font-size: 13px; color: var(--muted); font-weight: 600; letter-spacing: 0.02em; }
.fld.inline { display: flex; align-items: center; gap: 10px; flex-wrap: wrap; }
.fld input, .fld select, .seatrow input, .seatrow select, .profedit input[type="text"], .profedit input[type="number"], .profedit textarea, .profedit select, .simrow input, .simrow select {
  background: var(--bg); border: 1px solid var(--line); border-radius: 7px; padding: 7px 9px; color: var(--ink); font-size: 15px; font-weight: 500; min-width: 0;
}
.fld input[type="number"] { width: 100%; max-width: 140px; font-variant-numeric: tabular-nums; }
.chk { display: flex; align-items: center; gap: 8px; font-size: 15px; cursor: pointer; }
.chk input { accent-color: var(--accent-ink); width: 16px; height: 16px; }
.unit { color: var(--muted); font-weight: 500; }
.hint { color: var(--muted); font-size: 13px; }
.warn { color: var(--accent-ink); font-size: 14px; }

.btn {
  background: var(--surface-2); border: 1px solid var(--line); border-radius: 8px; padding: 8px 14px; cursor: pointer;
  font-weight: 600; letter-spacing: 0.02em; color: var(--ink);
}
.btn:hover:not(:disabled) { border-color: var(--muted); }
.btn:disabled { opacity: 0.45; cursor: default; }
.btn.primary { background: var(--accent); color: #1c1506; border-color: transparent; }
.btn.primary:hover:not(:disabled) { filter: brightness(1.08); }
.btn.small { padding: 5px 10px; font-size: 14px; }
.btn.big { font-family: var(--display); font-weight: 400; font-size: 22px; padding: 12px 0; letter-spacing: 0.04em; }

/* ---------- setup ---------- */
.setup { margin-bottom: 14px; padding: 0; }
.setup summary {
  list-style: none; cursor: pointer; padding: 12px 16px; display: flex; gap: 12px; align-items: baseline; flex-wrap: wrap;
  font-weight: 700; letter-spacing: 0.04em;
}
.setup summary::-webkit-details-marker { display: none; }
.setup summary::before { content: '▸'; color: var(--accent-ink); transition: transform 0.15s; display: inline-block; }
.setup[open] summary::before { transform: rotate(90deg); }
.setup-sum { color: var(--muted); font-weight: 500; letter-spacing: 0; font-size: 14px; }
.setup-body { padding: 0 16px 16px; display: grid; grid-template-columns: minmax(0, 1fr); gap: 14px; }
.setup-grid { display: flex; flex-wrap: wrap; gap: 14px 18px; align-items: end; }
.setup-grid.tight { margin: 12px 0; }
.seatcfg { display: grid; grid-template-columns: repeat(auto-fill, minmax(290px, 1fr)); gap: 8px; }
.seatrow { display: grid; grid-template-columns: 26px 1fr 1.3fr; gap: 6px; align-items: center; }
.seatrow > * { min-width: 0; width: 100%; }
.simrow > * { min-width: 0; }
.house { min-width: 0; grid-template-columns: minmax(0, 1fr); }
.house select { min-width: 0; }
.seatrow .num { font-family: var(--display); color: var(--muted); font-size: 14px; text-align: center; }
.house { border: 1px solid var(--line); border-radius: 10px; padding: 10px 14px; display: grid; gap: 10px; }
.house legend { padding: 0 6px; font-size: 13px; color: var(--muted); font-weight: 700; letter-spacing: 0.1em; text-transform: uppercase; }
.setup-actions { display: flex; gap: 14px; align-items: center; flex-wrap: wrap; }

/* ---------- play layout ---------- */
.playgrid { display: grid; grid-template-columns: minmax(0, 1fr) 370px; gap: 18px; align-items: start; }
.tablecol { display: grid; gap: 12px; min-width: 0; }
.side { display: grid; gap: 12px; position: sticky; top: 12px; }
@media (max-width: 1020px) {
  .playgrid { grid-template-columns: 1fr; }
  .side { position: static; }
}
.tablehead { display: flex; flex-wrap: wrap; gap: 6px 18px; color: var(--muted); font-size: 14px; align-items: baseline; }
.tablehead b { color: var(--ink); font-variant-numeric: tabular-nums; }
.tablehead .rnd { font-family: var(--display); color: var(--ink); font-size: 18px; }

.seats { display: grid; grid-template-columns: repeat(auto-fill, minmax(260px, 1fr)); gap: 10px; }
.seat {
  background: var(--surface); border: 1px solid var(--line); border-radius: 12px; padding: 10px 12px;
  display: grid; gap: 7px; align-content: start; transition: border-color 0.2s, box-shadow 0.2s;
}
.seat.human { box-shadow: inset 3px 0 0 var(--accent); }
.seat.turn { border-color: var(--accent-ink); box-shadow: 0 0 0 1px var(--accent), 0 8px 24px -12px rgba(243, 179, 62, 0.5); }
.seat.human .prof { color: var(--accent-ink); }
.seat-h { display: grid; grid-template-columns: minmax(0, 1fr) auto auto; gap: 10px; align-items: center; }
.who { display: grid; min-width: 0; }
.nm, .prof { white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.nm { font-weight: 700; font-size: 17px; line-height: 1.15; }
.prof { color: var(--muted); font-size: 13px; line-height: 1.25; }
.badges { display: grid; justify-items: end; gap: 3px; }
.dealer-tag { font-size: 10px; font-weight: 700; letter-spacing: 0.14em; text-transform: uppercase; color: var(--muted); line-height: 1; }
.pill { font-size: 12px; font-weight: 700; letter-spacing: 0.06em; text-transform: uppercase; border-radius: 999px; padding: 2px 9px; border: 1px solid var(--line); color: var(--muted); white-space: nowrap; min-width: 78px; text-align: center; }
.pill.active { color: var(--ink); }
.pill.deciding { background: var(--accent); border-color: var(--accent-ink); color: #1c1506; }
.pill.stayed { color: var(--good); border-color: color-mix(in oklab, var(--good) 55%, transparent); }
.pill.frozen { color: var(--frost-ink); border-color: color-mix(in oklab, var(--frost) 55%, transparent); }
.pill.busted { color: #fff; background: var(--bad); border-color: var(--bad); }
.pill.flip7 { background: var(--accent); border-color: var(--accent-ink); color: #1c1506; }

.score { display: flex; align-items: baseline; gap: 2px; white-space: nowrap; }
.tot { font-family: var(--display); font-size: 27px; line-height: 1; font-variant-numeric: tabular-nums; }
.of { color: var(--muted); font-size: 13px; }
.meter { height: 5px; background: var(--bg); border-radius: 3px; overflow: hidden; display: flex; }
.meter i { display: block; height: 100%; background: var(--ink); opacity: 0.75; }
.meter i.add { background: var(--accent); opacity: 1; }

.hand { display: grid; grid-template-columns: minmax(0, 1fr) auto; gap: 10px; align-items: center; }
.cards { display: flex; flex-wrap: wrap; gap: 5px; min-height: 54px; align-items: center; }
.sep { width: 2px; align-self: stretch; margin: 8px 2px; background: var(--line); border-radius: 1px; }
.rpcol { display: grid; justify-items: end; gap: 2px; font-variant-numeric: tabular-nums; }
.rp { font-weight: 700; font-size: 17px; }
.rp.pos { color: var(--accent-ink); }
.rp.lost { color: var(--bad); text-decoration: line-through; text-decoration-thickness: 2px; }
.odds { font-size: 12px; color: var(--muted); white-space: nowrap; }
.odds.hot { color: var(--bad); font-weight: 700; }
.empty { color: var(--muted); font-size: 13px; font-style: italic; }

/* cards */
.card {
  display: inline-grid; place-items: center; position: relative;
  width: 38px; height: 54px; border-radius: 6px;
  background: var(--paper); color: var(--card-ink);
  font-family: var(--display); font-size: 21px; line-height: 1;
  box-shadow: var(--card-shadow);
}
.card.n::after {
  content: ''; position: absolute; left: 5px; right: 5px; bottom: 5px; height: 3px; border-radius: 2px;
  background: linear-gradient(90deg, var(--brick) calc(var(--h) * 100%), rgba(26, 32, 45, 0.12) 0);
}
.card.mod { background: var(--card-ink); color: var(--accent); box-shadow: inset 0 0 0 1.5px var(--accent), 0 2px 0 rgba(0,0,0,.45); font-size: 15px; }
.card.act { width: auto; min-width: 54px; padding: 0 7px; height: 30px; font-size: 11px; letter-spacing: 0.04em; color: var(--card-ink); }
.card.a19 { background: var(--frost); }
.card.a20 { background: var(--flip); }
.card.a21 { background: var(--second); }
.card.queued { opacity: 0.6; outline: 1.5px dashed var(--muted); outline-offset: 2px; }
.card.bustcard { background: #fff3f0; box-shadow: inset 0 0 0 2px var(--bad), 0 2px 0 rgba(0,0,0,.45); transform: rotate(8deg); }
.card.bustcard::before { content: 'BUST'; position: absolute; top: -9px; font-family: var(--body); font-weight: 700; font-size: 10px; letter-spacing: 0.1em; color: #fff; background: var(--bad); padding: 0 4px; border-radius: 3px; }
.seat.busted .cards .card:not(.bustcard) { opacity: 0.42; }
.card.fresh { animation: flipin 0.38s cubic-bezier(.2, .8, .2, 1); }
@keyframes flipin {
  from { transform: perspective(500px) rotateY(88deg) translateY(-6px); }
  to { transform: perspective(500px) rotateY(0) translateY(0); }
}

/* ---------- control ---------- */
.control { display: grid; gap: 12px; }
.ctl-q { font-size: 17px; margin: 0; }
.ctl-q small { display: block; color: var(--muted); font-size: 14px; margin-top: 2px; }
.hitstay { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
.btn.hit { background: var(--accent); color: #1c1506; border-color: transparent; }
.btn.stay { background: transparent; border: 2px solid var(--good); color: var(--good); }
.choices { display: grid; gap: 8px; }
.choice { text-align: left; display: flex; justify-content: space-between; gap: 10px; align-items: baseline; }
.choice small { color: var(--muted); font-weight: 500; font-variant-numeric: tabular-nums; }
.choice.rec { border-color: var(--accent-ink); }
.choice.rec small::after { content: ' · advisor pick'; color: var(--accent-ink); }
.ctl-row { display: flex; gap: 8px; flex-wrap: wrap; align-items: center; }
.keys { color: var(--muted); font-size: 13px; }
.kbd { font-family: var(--display); font-size: 11px; border: 1px solid var(--line); border-radius: 4px; padding: 0 5px; color: var(--ink); }
.results { width: 100%; border-collapse: collapse; font-variant-numeric: tabular-nums; font-size: 15px; }
.results td, .results th { padding: 5px 4px; border-bottom: 1px solid var(--line); text-align: left; }
.results th { color: var(--muted); font-size: 12px; text-transform: uppercase; letter-spacing: 0.08em; font-weight: 600; }
.results td.n, .results th.n { text-align: right; }
.results .st-busted { color: var(--bad); } .results .st-flip7 { color: var(--accent-ink); } .results .st-frozen { color: var(--frost-ink); } .results .st-stayed { color: var(--good); }
.winner { font-family: var(--display); font-size: 26px; line-height: 1.1; color: var(--accent-ink); }

/* ---------- advisor ---------- */
.advisor:empty, .tracker:empty { display: none; }
.adv-stats { display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 8px; margin-bottom: 10px; }
.stat { background: var(--bg); border-radius: 8px; padding: 8px 10px; display: grid; gap: 1px; }
.stat span { font-size: 12px; color: var(--muted); text-transform: uppercase; letter-spacing: 0.08em; font-weight: 600; }
.stat b { font-family: var(--display); font-weight: 400; font-size: 22px; font-variant-numeric: tabular-nums; }
.stat b.hot { color: var(--bad); } .stat b.ok { color: var(--good); }
.stat small { color: var(--muted); font-size: 12px; }
.verdicts { width: 100%; border-collapse: collapse; font-size: 14px; }
.verdicts td { padding: 5px 4px; border-top: 1px solid var(--line); vertical-align: top; }
.verdicts td:first-child { font-weight: 700; white-space: nowrap; }
.verdicts td.why { color: var(--muted); font-size: 13px; }
.call { font-size: 11px; font-weight: 700; letter-spacing: 0.08em; padding: 1px 6px; border-radius: 4px; }
.call.hit { background: var(--accent); color: #1c1506; }
.call.stay { border: 1px solid var(--good); color: var(--good); }
.ranked { margin: 0; padding-left: 20px; display: grid; gap: 6px; font-size: 14px; }
.ranked li::marker { color: var(--accent-ink); font-weight: 700; }
.ranked .v { font-variant-numeric: tabular-nums; color: var(--muted); }

/* ---------- tracker ---------- */
.trk-nums { display: grid; grid-template-columns: repeat(13, 1fr); gap: 3px; align-items: end; }
.trk-col { display: grid; gap: 2px; justify-items: center; font-variant-numeric: tabular-nums; }
.trk-col .bar { position: relative; width: 100%; max-width: 20px; height: 54px; }
.trk-col .bar i { position: absolute; left: 0; right: 0; bottom: 0; border-radius: 3px 3px 0 0; }
.trk-col .bar i.full { background: var(--line); }
.trk-col .bar i.rem { background: var(--ink); opacity: 0.8; }
.trk-col.risk .bar i.rem { background: var(--bad); opacity: 1; }
.trk-col b { font-family: var(--display); font-weight: 400; font-size: 12px; }
.trk-col small { font-size: 10px; color: var(--muted); }
.trk-col.risk b { color: var(--bad); }
.trk-chips { display: flex; flex-wrap: wrap; gap: 5px; margin-top: 10px; }
.chip { font-size: 12px; border: 1px solid var(--line); border-radius: 6px; padding: 2px 7px; font-variant-numeric: tabular-nums; color: var(--ink); }
.chip.gone { opacity: 0.35; text-decoration: line-through; }
.trk-legend { font-size: 12px; color: var(--muted); margin-top: 8px; }

/* ---------- log & sheet ---------- */
.logbox { max-height: 360px; display: grid; grid-template-rows: auto 1fr; }
.log { list-style: none; margin: 0; padding: 0; overflow-y: auto; font-size: 14px; display: grid; gap: 3px; align-content: start; }
.log li { padding: 2px 0; }
.log li.reason { color: var(--muted); font-size: 13px; padding-left: 12px; border-left: 2px solid var(--line); }
.log li.round { font-weight: 700; color: var(--accent-ink); margin-top: 6px; }
.log li.bad { color: var(--bad); }
.log li.good { color: var(--good); }
.sheet summary { cursor: pointer; font-weight: 700; letter-spacing: 0.04em; }
.sheet-wrap { overflow-x: auto; margin-top: 10px; }
.sheet table { border-collapse: collapse; font-variant-numeric: tabular-nums; font-size: 14px; min-width: 100%; }
.sheet th, .sheet td { padding: 4px 8px; border-bottom: 1px solid var(--line); text-align: right; white-space: nowrap; }
.sheet th:first-child, .sheet td:first-child { text-align: left; color: var(--muted); }
.sheet td small { color: var(--muted); }
.sheet td.bust { color: var(--bad); }
.sheet td.f7 { color: var(--accent-ink); font-weight: 700; }
.sheet tr.tot td { font-weight: 700; border-top: 2px solid var(--line); }

/* ---------- simulator ---------- */
.simgrid { display: grid; grid-template-columns: minmax(300px, 400px) minmax(0, 1fr); gap: 16px; align-items: start; margin-bottom: 16px; }
@media (max-width: 900px) { .simgrid { grid-template-columns: 1fr; } }
.simcfg { display: grid; gap: 10px; }
.simseats { display: grid; gap: 6px; }
.simrow { display: grid; grid-template-columns: 1fr 1.2fr 30px; gap: 6px; }
.simrow button { background: none; border: 1px solid var(--line); border-radius: 7px; cursor: pointer; color: var(--muted); }
.simrow button:disabled { opacity: 0.3; cursor: default; }
.runrow { display: flex; gap: 10px; align-items: center; flex-wrap: wrap; }
.progress { flex: 1; min-width: 140px; height: 26px; background: var(--bg); border-radius: 7px; position: relative; overflow: hidden; }
.progress i { position: absolute; inset: 0 auto 0 0; width: 0; background: color-mix(in oklab, var(--accent) 35%, transparent); transition: width 0.15s; }
.progress span { position: relative; display: block; text-align: center; line-height: 26px; font-size: 13px; color: var(--muted); font-variant-numeric: tabular-nums; }
.simres { min-width: 0; }
.tblwrap { overflow-x: auto; }
.stbl { border-collapse: collapse; width: 100%; font-variant-numeric: tabular-nums; font-size: 14px; }
.stbl th, .stbl td { padding: 6px 8px; border-bottom: 1px solid var(--line); text-align: right; white-space: nowrap; }
.stbl th { color: var(--muted); font-size: 12px; text-transform: uppercase; letter-spacing: 0.06em; font-weight: 600; vertical-align: bottom; }
.stbl td:first-child, .stbl th:first-child, .stbl td:nth-child(2), .stbl th:nth-child(2) { text-align: left; }
.stbl small { color: var(--muted); }
.verdict { font-size: 11px; font-weight: 700; letter-spacing: 0.06em; text-transform: uppercase; padding: 1px 6px; border-radius: 4px; border: 1px solid var(--line); color: var(--muted); }
.verdict.up { color: var(--good); border-color: var(--good); }
.verdict.down { color: var(--bad); border-color: var(--bad); }
.chart { width: 100%; height: auto; display: block; margin: 6px 0 14px; }
.stbl.sw td:nth-child(2), .stbl.sw th:nth-child(2) { text-align: right; }
.chart text { fill: var(--muted); font-family: var(--body); font-size: 13px; }
.chart text.lbl { fill: var(--ink); font-weight: 600; font-size: 14px; }
.chart text.val { fill: var(--ink); font-variant-numeric: tabular-nums; }
.chart .grid { stroke: var(--line); stroke-width: 1; }
.chart .parity { stroke: var(--muted); stroke-dasharray: 4 4; stroke-width: 1.5; }
.chart .bar { fill: var(--accent); }
.chart .ci { stroke: var(--ink); stroke-width: 2; }
.chart .band { fill: var(--accent); opacity: 0.18; }
.chart .line { fill: none; stroke: var(--accent); stroke-width: 2; }
.chart .dot { fill: var(--accent); stroke: var(--surface); stroke-width: 2; }
.chart .dot.best { fill: var(--ink); }
.chart .hit { fill: transparent; cursor: default; }
.notes { color: var(--muted); font-size: 14px; display: grid; gap: 4px; }
.sweep { display: grid; gap: 12px; }
.tip {
  position: fixed; z-index: 10; pointer-events: none; background: var(--surface-2); border: 1px solid var(--line); border-radius: 8px;
  padding: 7px 10px; font-size: 13px; color: var(--ink); max-width: 280px; box-shadow: 0 10px 30px -10px rgba(0,0,0,.7);
}

/* ---------- profiles ---------- */
.profgrid { display: grid; grid-template-columns: minmax(280px, 380px) minmax(0, 1fr); gap: 16px; align-items: start; }
@media (max-width: 860px) { .profgrid { grid-template-columns: 1fr; } }
.proflist { display: grid; gap: 8px; }
#profList { display: grid; gap: 6px; }
.prof-item {
  text-align: left; background: var(--bg); border: 1px solid var(--line); border-radius: 9px; padding: 9px 11px; cursor: pointer; display: grid; gap: 2px;
}
.prof-item:hover { border-color: var(--muted); }
.prof-item.sel { border-color: var(--accent-ink); }
.pi-name { font-weight: 700; font-size: 16px; }
.pi-tag { font-size: 12px; color: var(--muted); text-transform: uppercase; letter-spacing: 0.08em; }
.pi-desc { font-size: 13px; color: var(--muted); }
.profedit { display: grid; gap: 14px; }
.pe-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(260px, 1fr)); gap: 14px 22px; }
.pe-f { display: grid; gap: 4px; align-content: start; }
.pe-f > span { font-size: 13px; font-weight: 700; letter-spacing: 0.04em; color: var(--ink); }
.pe-f > small { color: var(--muted); font-size: 13px; }
.pe-f input[type="range"] { accent-color: var(--accent-ink); width: 100%; }
.pe-f output { font-variant-numeric: tabular-nums; color: var(--accent-ink); font-weight: 700; }
.pe-f.off { opacity: 0.4; }
.profedit textarea { min-height: 60px; resize: vertical; width: 100%; }
.profedit input[type="text"] { width: 100%; }
.pe-actions { display: flex; gap: 10px; flex-wrap: wrap; align-items: center; }

/* ---------- rules ---------- */
.rules { max-width: 1180px; }
.rules-cols { display: grid; grid-template-columns: 1fr 1fr; gap: 28px; }
@media (max-width: 860px) { .rules-cols { grid-template-columns: 1fr; } }
.rules p, .rules li { max-width: 64ch; }
.rules h3 { margin-top: 16px; }
.calls { padding-left: 20px; display: grid; gap: 8px; margin: 0; }

@media (max-width: 420px) {
  main { padding: 12px 16px 32px; }
  .seats { grid-template-columns: 1fr; }
}
@media (prefers-reduced-motion: reduce) {
  .card.fresh, .sh-deck i { animation: none; }
  * { transition: none !important; }
}

/* ---------- peek (cheat view) ---------- */
.peek:empty { display: none; }
.peek { border-style: dashed; border-color: color-mix(in oklab, var(--accent) 55%, var(--line)); display: grid; gap: 10px; }
.peek h3 { display: flex; align-items: baseline; gap: 8px; margin: 0; }
.cheat { font-size: 10px; letter-spacing: 0.12em; color: #1c1506; background: var(--accent); border-radius: 3px; padding: 1px 5px; }
.peek-row { display: flex; gap: 12px; align-items: flex-end; }
.peek-slot { display: grid; justify-items: center; gap: 5px; font-size: 12px; color: var(--muted); text-transform: uppercase; letter-spacing: 0.08em; font-weight: 600; }
.peek-slot:first-child .card { outline: 2px solid var(--accent); outline-offset: 2px; }
.card.back { background: repeating-linear-gradient(135deg, var(--surface-2) 0 6px, var(--surface) 6px 12px); box-shadow: inset 0 0 0 1.5px var(--line); color: var(--muted); font-size: 16px; }
.peek-note { margin: 0; font-size: 14px; }
.peek-note.bust, .peek-list .bust { color: var(--bad); }
.peek-note.flip7, .peek-list .flip7 { color: var(--accent-ink); }
.peek-note.save { color: var(--frost-ink); }
.peek-list { margin: 0; padding: 0; list-style: none; display: grid; gap: 4px; font-size: 14px; }
.peek-list b { color: var(--ink); }

/* ---------- ladder & overrides ---------- */
.ladder { border: 1px solid var(--line); border-radius: 10px; padding: 8px 12px; }
.ladder summary { cursor: pointer; font-weight: 700; font-size: 15px; }
.ladder[open] { display: grid; gap: 8px; }
.ladder .hint { margin: 4px 0 0; }
.ovr { grid-column: 1 / -1; font-size: 13px; color: var(--accent-ink); display: flex; gap: 8px; align-items: baseline; margin-top: -2px; }
.simrow .ovr button { border: 0; background: none; color: var(--muted); text-decoration: underline; cursor: pointer; padding: 0; font-size: 13px; }

/* ---------- setup additions ---------- */
.seat-tools { display: flex; flex-wrap: wrap; gap: 8px 12px; align-items: center; }
.helpers { grid-template-columns: repeat(auto-fill, minmax(230px, 1fr)); }
.helpers legend { display: flex; gap: 10px; align-items: baseline; }
.legend-note { text-transform: none; letter-spacing: 0; font-weight: 500; color: var(--muted); }
.lab-tag { color: var(--accent-ink); }
.pe-avail { display: grid; gap: 4px; background: var(--bg); border: 1px solid var(--line); border-radius: 9px; padding: 10px 12px; }
.pe-avail small { color: var(--muted); font-size: 13px; }

/* ---------- last card, shuffle notice, standings ---------- */
.lastline { display: flex; gap: 6px 10px; align-items: baseline; flex-wrap: wrap; font-size: 14px; color: var(--muted); padding-bottom: 10px; border-bottom: 1px solid var(--line); }
.lastline b { color: var(--ink); }
.ll-label { font-size: 11px; font-weight: 700; letter-spacing: 0.12em; text-transform: uppercase; }
.ll-val { font-family: var(--display); color: var(--ink); font-size: 15px; }
.ll-note { font-size: 13px; }
.card.last { outline: 2px solid var(--accent); outline-offset: 2px; }
.results tr.lead td { font-weight: 700; }
.results tr.lead td:nth-child(5) { color: var(--accent-ink); }
.fresh-shuffle { font-size: 12px; font-weight: 700; letter-spacing: 0.06em; text-transform: uppercase; color: var(--accent-ink); border: 1px solid var(--accent); border-radius: 999px; padding: 1px 9px; }
.trk-legend.fresh-note { color: var(--accent-ink); }
.shuffle-toast {
  position: fixed; top: 84px; left: 50%; transform: translateX(-50%); z-index: 20;
  display: flex; gap: 16px; align-items: center; max-width: calc(100vw - 32px);
  background: var(--surface); border: 1px solid var(--accent); border-radius: 12px; padding: 14px 20px;
  box-shadow: 0 18px 40px -16px rgba(0, 0, 0, 0.6); animation: toastin 0.25s ease-out;
}
.shuffle-toast[hidden] { display: none; }
.sh-txt { display: grid; gap: 2px; }
.sh-txt b { font-family: var(--display); font-weight: 400; font-size: 19px; }
.sh-txt span { color: var(--muted); font-size: 14px; }
.sh-deck { position: relative; width: 40px; height: 56px; flex-shrink: 0; }
.sh-deck i { position: absolute; inset: 0; border-radius: 6px; background: repeating-linear-gradient(135deg, var(--surface-2) 0 6px, var(--bg) 6px 12px); box-shadow: inset 0 0 0 1.5px var(--line), 0 2px 0 rgba(0, 0, 0, 0.3); }
.sh-deck i:nth-child(1) { animation: riffleL 0.8s ease-in-out infinite; }
.sh-deck i:nth-child(2) { animation: riffleR 0.8s ease-in-out infinite; }
@keyframes riffleL { 0%, 100% { transform: translateX(0) rotate(0); } 50% { transform: translateX(-13px) rotate(-9deg); } }
@keyframes riffleR { 0%, 100% { transform: translateX(0) rotate(0); } 50% { transform: translateX(13px) rotate(9deg); } }
@keyframes toastin { from { opacity: 0; transform: translate(-50%, -8px); } to { opacity: 1; transform: translate(-50%, 0); } }
@media (prefers-reduced-motion: reduce) { .shuffle-toast { animation: none; } }

/* ---------- your hand in the control panel ---------- */
.card.mini { width: 28px; height: 40px; font-size: 15px; border-radius: 5px; }
.card.mini.n::after { left: 4px; right: 4px; bottom: 4px; height: 2px; }
.card.mini.mod { font-size: 11px; }
.card.mini.act { width: auto; height: 22px; min-width: 0; font-size: 9px; padding: 0 6px; white-space: nowrap; }
.dock-hand { display: grid; grid-template-columns: minmax(0, 1fr) auto; gap: 10px; align-items: center; background: var(--bg); border-radius: 9px; padding: 7px 10px; }
.dh-cards { display: flex; flex-wrap: wrap; gap: 4px; align-items: center; min-height: 40px; }
.dh-pts { display: grid; justify-items: end; gap: 1px; font-size: 12px; color: var(--muted); font-variant-numeric: tabular-nums; white-space: nowrap; }
.dh-pts b { font-size: 17px; color: var(--accent-ink); }
.dh-pts .hot { color: var(--bad); font-weight: 700; }
.ctl-q small.inl { display: inline; margin-left: 6px; }

/* ---------- menu ---------- */
.menubtn {
  display: inline-flex; align-items: center; gap: 6px; background: var(--surface); border: 1px solid var(--line); border-radius: 9px;
  padding: 6px 10px; cursor: pointer; color: var(--ink); font-weight: 600; font-size: 14px;
}
.menubtn:hover { border-color: var(--muted); }
.menubtn[aria-expanded="true"] { border-color: var(--accent-ink); }
.menu {
  position: absolute; right: 20px; top: calc(100% - 6px); z-index: 30; width: 260px;
  background: var(--surface); border: 1px solid var(--line); border-radius: 12px; padding: 6px;
  display: grid; gap: 2px; box-shadow: 0 18px 40px -16px rgba(0, 0, 0, 0.6);
}
.menu[hidden] { display: none; }
.menu-tabs { display: none; gap: 2px; padding-bottom: 6px; margin-bottom: 4px; border-bottom: 1px solid var(--line); }
.menu-item { text-align: left; background: none; border: 0; border-radius: 8px; padding: 10px 12px; cursor: pointer; font-weight: 600; font-size: 15px; color: var(--ink); }
.menu-item:hover { background: var(--surface-2); }
.menu-item[aria-current="page"] { color: var(--accent-ink); }
.menu-item[aria-current="page"]::before { content: '▸ '; }
.mob-only { display: none; }

/* ---------- backup dialog ---------- */
.modal { position: fixed; inset: 0; z-index: 40; background: rgba(8, 10, 15, 0.6); display: grid; place-items: start center; padding: 40px 16px; overflow-y: auto; }
.modal[hidden] { display: none; }
.modal-card { width: min(640px, 100%); background: var(--surface); border: 1px solid var(--line); border-radius: 14px; padding: 16px 18px; display: grid; gap: 14px; box-shadow: 0 30px 60px -20px rgba(0, 0, 0, 0.7); }
.modal-head { display: flex; justify-content: space-between; align-items: center; gap: 12px; }
.modal-head h2 { margin: 0; }
.bk-sec { display: grid; gap: 8px; }
.bk-sec h3 { margin: 0; }
.bk-sec .hint { margin: 0; }
.bk-sec textarea { width: 100%; background: var(--bg); border: 1px solid var(--line); border-radius: 8px; padding: 8px 10px; color: var(--ink); font-family: ui-monospace, 'SF Mono', Menlo, Consolas, monospace; font-size: 12px; resize: vertical; }
.bk-sum { display: grid; gap: 8px; background: var(--bg); border-radius: 9px; padding: 10px 12px; }
.bk-sum p { margin: 0; }
.ok-msg { color: var(--good); margin: 0; font-weight: 600; }
.vh { position: absolute; width: 1px; height: 1px; overflow: hidden; clip: rect(0 0 0 0); white-space: nowrap; }
.pi-as { font-weight: 500; font-size: 13px; color: var(--accent-ink); margin-left: 4px; }

/* ---------- one column: controls first and pinned to the top while you scroll ---------- */
@media (max-width: 1020px) {
  .side { display: contents; }
  .control { order: -1; position: sticky; top: 0; z-index: 6; max-height: 62vh; max-height: 62dvh; overflow-y: auto; box-shadow: 0 14px 26px -18px rgba(0, 0, 0, 0.75); }
  .control.tall { max-height: 90vh; max-height: 90dvh; }
  .advisor, .tracker, .logbox, .peek { order: 1; }
}
@media (hover: none) { .keys { display: none; } }

/* ---------- phones ---------- */
@media (max-width: 720px) {
  .top { padding: 10px 16px; gap: 10px; flex-wrap: nowrap; }
  .brand { gap: 10px; flex-wrap: nowrap; min-width: 0; }
  .mark { font-size: 24px; }
  .sub { font-size: 11px; letter-spacing: 0.16em; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .tabs { display: none; }
  .menulbl { display: none; }
  .menubtn { padding: 7px 9px; }
  .menu { left: 12px; right: 12px; width: auto; }
  .menu-tabs { display: grid; }
  .mob-only { display: block; }
  .setup:not(.forced) { display: none; }
  main { padding-top: 10px; }
  .tablehead { font-size: 13px; gap: 2px 12px; }
  .control { padding: 10px 12px; gap: 8px; border-radius: 0 0 12px 12px; }
  .lastline { font-size: 13px; padding-bottom: 6px; }
  .ctl-q { font-size: 16px; }
  .btn.big { font-size: 19px; padding: 9px 0; }
  .choices { grid-template-columns: 1fr 1fr; gap: 6px; }
  .choice { display: grid; gap: 1px; padding: 7px 10px; }
  .choice.rec small::after { content: ' · pick'; }
  .shuffle-toast { top: auto; bottom: 16px; }
  .results td, .results th { padding: 3px 4px; }
  .control .ctl-q small:not(.inl) { display: none; }
  .modal { padding: 12px; }
}

</style>
</head>
<body>
<header class="top">
  <div class="brand">
    <span class="mark">Flip<span class="seven">7</span></span>
    <span class="sub">Table &amp; Strategy Lab</span>
  </div>
  <div class="topright">
  <nav class="tabs" role="tablist" aria-label="Sections">
    <button class="tab-btn" role="tab" id="tb-play" data-tab="play" aria-selected="true">Play</button>
    <button class="tab-btn" role="tab" id="tb-sim" data-tab="sim" aria-selected="false">Stress test</button>
    <button class="tab-btn" role="tab" id="tb-profiles" data-tab="profiles" aria-selected="false">Player profiles</button>
    <button class="tab-btn" role="tab" id="tb-rules" data-tab="rules" aria-selected="false">Rules</button>
  </nav>
  <button class="menubtn" id="menuBtn" type="button" aria-expanded="false" aria-controls="menu" aria-label="Menu">
    <svg viewBox="0 0 24 24" width="22" height="22" aria-hidden="true"><path d="M4 7h16M4 12h16M4 17h16" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" fill="none"/></svg><span class="menulbl">Menu</span>
  </button>
  </div>
  <div class="menu" id="menu" hidden>
    <div class="menu-tabs" aria-label="Sections">
      <button type="button" class="menu-item" data-go="play">Play</button>
      <button type="button" class="menu-item" data-go="sim">Stress test</button>
      <button type="button" class="menu-item" data-go="profiles">Player profiles</button>
      <button type="button" class="menu-item" data-go="rules">Rules</button>
    </div>
    <button type="button" class="menu-item mob-only" data-menu="setup">Table setup</button>
    <button type="button" class="menu-item" id="themeBtn" data-menu="theme" aria-pressed="false">Light mode</button>
    <button type="button" class="menu-item" data-menu="backup">Back up or move settings</button>
  </div>
</header>

<main>
  <!-- PLAY -->
  <section class="tab" id="tab-play" role="tabpanel" aria-labelledby="tb-play">
    <details class="setup panel" id="setupBox">
      <summary><span>Table setup</span><span class="setup-sum" id="setupSum"></span></summary>
      <div class="setup-body">
        <div class="setup-grid">
          <label class="fld" for="nPlayers">Players
            <select id="nPlayers"></select>
          </label>
          <label class="fld" for="target">Target score
            <input id="target" type="number" min="50" max="1000" step="10">
          </label>
          <label class="fld" for="speed">AI speed
            <select id="speed">
              <option value="instant">Instant</option>
              <option value="fast">Fast</option>
              <option value="normal">Normal</option>
              <option value="slow">Slow</option>
            </select>
          </label>
        </div>
        <div class="seat-tools">
          <button class="btn small" id="rndCount" type="button">Random seat count</button>
          <button class="btn small" id="rndPlayers" type="button">Random AI players</button>
          <label class="chk" for="rerollBetween"><input type="checkbox" id="rerollBetween"> Re-roll seat count and AI players for each new game</label>
        </div>
        <div class="seatcfg" id="seatCfg"></div>
        <fieldset class="house helpers">
          <legend>On-screen help <span class="legend-note">applies right away</span></legend>
          <label class="chk" for="showOdds"><input type="checkbox" id="showOdds"> Bust odds on each seat</label>
          <label class="chk" for="showCount"><input type="checkbox" id="showCount"> Card count</label>
          <label class="chk" for="showAdvisor"><input type="checkbox" id="showAdvisor"> Advisor</label>
          <label class="chk" for="explain"><input type="checkbox" id="explain"> Explain AI moves in the log</label>
          <label class="chk" for="peekToggle"><input type="checkbox" id="peekToggle"> Peek at the next three cards (cheat view)</label>
        </fieldset>
        <fieldset class="house">
          <legend>House rules</legend>
          <label class="chk" for="hrDiscard"><input type="checkbox" id="hrDiscard"> Last active player may discard a Flip Three instead of taking it</label>
          <label class="chk" for="hrDiscardFreeze"><input type="checkbox" id="hrDiscardFreeze"> Last active player may discard a Freeze instead of taking it</label>
          <label class="fld inline" for="hrOpener">Deal and play begin with
            <select id="hrOpener">
              <option value="rotate">Rulebook: the next player to the left each round</option>
              <option value="winnerLeft">The player left of last round's top scorer</option>
              <option value="winner">Last round's top scorer</option>
            </select>
          </label>
        </fieldset>
        <div class="setup-actions">
          <button class="btn primary" id="btnStart" type="button">Start new game</button>
          <span class="hint">Seats, target and house rules apply when you start a new game.</span>
        </div>
      </div>
    </details>

    <div class="playgrid">
      <div class="tablecol">
        <div class="tablehead" id="tableHead"></div>
        <div class="seats" id="seats"></div>
        <details class="panel sheet" id="sheetBox">
          <summary>Score sheet</summary>
          <div class="sheet-wrap" id="sheet"></div>
        </details>
      </div>
      <aside class="side">
        <div class="panel control" id="control" aria-live="polite"></div>
        <div class="panel advisor" id="advisor"></div>
        <div class="panel tracker" id="tracker"></div>
        <div class="panel logbox">
          <h3>Table log</h3>
          <ol class="log" id="log"></ol>
        </div>
        <div class="panel peek" id="peek"></div>
      </aside>
    </div>
  </section>

  <!-- STRESS TEST -->
  <section class="tab" id="tab-sim" role="tabpanel" aria-labelledby="tb-sim" hidden>
    <div class="simgrid">
      <div class="panel simcfg">
        <h2>Tournament</h2>
        <p class="lede">Every seat is an AI profile. The simulator plays the full game to the target score, many times over, and reports how often each seat wins.</p>
        <div id="simSeats" class="simseats"></div>
        <button class="btn small" id="simAdd" type="button">Add player</button>
        <details class="ladder" id="ladderBox">
          <summary>Fill the seats with a ladder</summary>
          <p class="hint">Every seat gets the same base profile, with one setting stepped from seat to seat. Example: Bank at 15, 20, 25 … 40.</p>
          <div class="setup-grid tight">
            <label class="fld" for="ldBase">Base profile
              <select id="ldBase"></select>
            </label>
            <label class="fld" for="ldParam">Setting
              <select id="ldParam"></select>
            </label>
            <label class="fld" for="ldFrom">From <span class="unit" id="ldUnit1"></span>
              <input id="ldFrom" type="number" step="any">
            </label>
            <label class="fld" for="ldTo">To <span class="unit" id="ldUnit2"></span>
              <input id="ldTo" type="number" step="any">
            </label>
            <label class="fld" for="ldStep">Step
              <input id="ldStep" type="number" step="any">
            </label>
          </div>
          <p class="warn" id="ldWarn" hidden></p>
          <div class="runrow"><button class="btn" id="ldFill" type="button">Fill seats</button><span class="hint" id="ldCount"></span></div>
        </details>
        <div class="setup-grid tight">
          <label class="fld" for="simGames">Games
            <input id="simGames" type="number" min="10" max="100000" step="100">
          </label>
          <label class="fld" for="simTarget">Target score
            <input id="simTarget" type="number" min="50" max="1000" step="10">
          </label>
          <label class="fld" for="simSeed">Seed (optional)
            <input id="simSeed" type="text" placeholder="random">
          </label>
        </div>
        <label class="chk" for="simRotate"><input type="checkbox" id="simRotate"> Shuffle seating each game (removes seat-order bias)</label>
        <label class="chk" for="simHrDiscard"><input type="checkbox" id="simHrDiscard"> House rule: last player may discard Flip Three</label>
        <label class="chk" for="simHrDiscardFreeze"><input type="checkbox" id="simHrDiscardFreeze"> House rule: last player may discard Freeze</label>
        <label class="fld inline" for="simHrOpener">Deal and play begin with
          <select id="simHrOpener">
            <option value="rotate">Rulebook: next player left</option>
            <option value="winnerLeft">Left of last round's top scorer</option>
            <option value="winner">Last round's top scorer</option>
          </select>
        </label>
        <div class="runrow">
          <button class="btn primary" id="simRun" type="button">Run tournament</button>
          <button class="btn" id="simStop" type="button" disabled>Stop</button>
          <div class="progress" id="simProg"><i></i><span></span></div>
        </div>
      </div>
      <div class="panel simres">
        <h2>Results</h2>
        <div id="simResults"><p class="muted">Run a tournament to see win rates.</p></div>
      </div>
    </div>

    <div class="panel sweep">
      <h2>Parameter sweep</h2>
      <p class="lede">Hold the rest of the table fixed and step one setting of one seat through a range. Uses the tournament seats above.</p>
      <div class="setup-grid">
        <label class="fld" for="swSeat">Seat to vary
          <select id="swSeat"></select>
        </label>
        <label class="fld" for="swParam">Setting
          <select id="swParam"></select>
        </label>
        <label class="fld" for="swFrom">From <span class="unit" id="swUnit1"></span>
          <input id="swFrom" type="number" step="any">
        </label>
        <label class="fld" for="swTo">To <span class="unit" id="swUnit2"></span>
          <input id="swTo" type="number" step="any">
        </label>
        <label class="fld" for="swStep">Step
          <input id="swStep" type="number" step="any">
        </label>
        <label class="fld" for="swGames">Games per value
          <input id="swGames" type="number" min="50" max="20000" step="50">
        </label>
      </div>
      <p class="warn" id="swWarn" hidden></p>
      <div class="runrow">
        <button class="btn primary" id="swRun" type="button">Run sweep</button>
        <button class="btn" id="swStop" type="button" disabled>Stop</button>
        <div class="progress" id="swProg"><i></i><span></span></div>
      </div>
      <div id="swResults"></div>
    </div>
  </section>

  <!-- PROFILES -->
  <section class="tab" id="tab-profiles" role="tabpanel" aria-labelledby="tb-profiles" hidden>
    <div class="profgrid">
      <div class="panel proflist">
        <h2>Profiles</h2>
        <p class="lede">Built-in profiles are fixed so test results stay comparable. Copy one to make your own.</p>
        <p class="lede">Every profile takes a free card: it never stays while the next card can't bust it, such as when it holds a Second Chance or no number cards. The one exception is a profile that reads the endgame staying to lock in a win.</p>
        <div id="profList"></div>
        <button class="btn small" id="profNew" type="button">New blank profile</button>
      </div>
      <div class="panel profedit" id="profEditor"></div>
    </div>
  </section>

  <!-- RULES -->
  <section class="tab" id="tab-rules" role="tabpanel" aria-labelledby="tb-rules" hidden>
    <div class="rules panel">
      <h2>How this table plays</h2>
      <div class="rules-cols">
        <div>
          <h3>The deck: 94 cards</h3>
          <p>Number cards 0 to 12, with as many copies of each number as its value: one 1, two 2s, up to twelve 12s, plus a single 0. There are six modifiers (+2, +4, +6, +8, +10, ×2) and three each of Freeze, Flip Three and Second Chance.</p>
          <h3>A round</h3>
          <p>The dealer deals one card face up to each player, starting on their left. Action cards resolve as they're dealt. Then, in the same order, each active player chooses to <b>hit</b> (take a card) or <b>stay</b> (bank this round's points). A second copy of a number you already hold is a <b>bust</b>: you score nothing this round.</p>
          <p>The round ends when nobody is active, or immediately when someone holds seven different numbers (<b>Flip 7</b>, +15 bonus).</p>
          <h3>Action cards</h3>
          <p><b>Freeze</b>: the target banks their points and leaves the round. <b>Flip Three</b>: the target takes the next three cards one at a time and stops early on a bust or a Flip 7. A Freeze or Flip Three drawn during it waits until all three cards are taken. <b>Second Chance</b>: keep it; it cancels one duplicate. You can hold only one. A spare goes to another active player, or to the discard pile if nobody can take it.</p>
          <p>Whoever draws an action card picks its target from the active players, themselves included. The last active player must target themselves.</p>
          <h3>Scoring</h3>
          <p>Add your numbers, double that sum for ×2, add any + modifiers, then add 15 for a Flip 7. First to the target score at the end of a round wins. If the leaders tie, everyone plays another round.</p>
        </div>
        <div>
          <h3>Gray areas, and how this table settles them</h3>
          <ol class="calls">
            <li><b>Deal interruptions.</b> Players knocked out during the deal (by a Freeze, say) are skipped. A player who already got cards from a Flip Three still gets their own deal card.</li>
            <li><b>Freeze on an empty hand.</b> Allowed. That player banks 0 for the round, which makes an early Freeze on the leader a strong play.</li>
            <li><b>Second Chance inside Flip Three.</b> It cancels the duplicate and the remaining flips continue.</li>
            <li><b>Waiting Freeze or Flip Three.</b> They resolve in the order drawn, chosen by the player who flipped them, even if their own waiting Freeze already froze them. A bust or a Flip 7 discards anything still waiting.</li>
            <li><b>Staying.</b> You need at least one card in front of you: a number, a modifier or a Second Chance. A Second Chance alone scores 0.</li>
            <li><b>Empty deck.</b> Discards are reshuffled when the draw pile runs out. Cards on the table stay put. If nothing is left to reshuffle, the round ends and everyone who hasn't busted scores.</li>
            <li><b>Turn order.</b> Play starts on the dealer's left and the deal passes left each round, unless you pick a house rule. With a house rule, a tie for top scorer goes to whoever played first that round.</li>
          </ol>
          <h3>House rules (off by default)</h3>
          <ul class="calls">
            <li><b>Last player may discard Flip Three.</b> The last active player can throw away a Flip Three instead of taking three cards. The publisher's FAQ says they must take it.</li>
            <li><b>Last player may discard Freeze.</b> The same, for a Freeze. Discarding is never worse than taking it, because you can still stay on your next turn, so the AI always discards.</li>
            <li><b>Deal and play begin left of the top scorer.</b> The player on the left of last round's top scorer gets the first card and opens. That puts the round winner last in turn order, with the most information.</li>
            <li><b>Deal and play begin with the top scorer.</b> Last round's top scorer gets the first card and opens, so they act first with the least information.</li>
          </ul>
          <h3>About the advisor</h3>
          <p>Bust odds come from an exact count of the cards left in the draw pile, which you could track yourself by watching every card since the last shuffle. "Hit, then choose well" is a three-card lookahead over your own draws. It ignores what opponents will draw in between and what they might target you with, so treat it as a strong hint, not the solved answer.</p>
          <h3>The peek (cheat view)</h3>
          <p>Off by default. Shows the top three cards of the draw pile in the order they'll be drawn. On your turn it tells you what a hit gives you. When you're aiming a Flip Three, it shows exactly what each possible target would get, since those are the three cards they'll take. With fewer than three cards left, the rest come from a reshuffle that hasn't happened yet, so they show as unknown. AI players never see the peek.</p>
          <h3>Limited-memory card counting</h3>
          <p>A middle ground between counting only this round's table and counting every card since the shuffle. The profile remembers some card types exactly since the shuffle, set by its memory bandwidth. Level 1 covers the 0, the 1, the ×2 and Second Chance. Level 2 adds the 2s, the +10 and Freeze. Level 3 adds the 3s, the +8 and Flip Three. Each level after that adds the next number and the next smaller modifier, up to level 12, which is a full count. For everything else it only knows what's on the table this round, plus how big the draw pile looks.</p>
        </div>
      </div>
      <p class="fine">Unofficial fan-made practice tool. Flip 7 was designed by Eric Olsen and is published by The Op Games.</p>
    </div>
  </section>
</main>
<div class="tip" id="tip" role="tooltip" hidden></div>
<div class="shuffle-toast" id="shuffleToast" role="status" aria-live="polite" hidden></div>
<div class="modal" id="backupModal" hidden>
  <div class="modal-card" role="dialog" aria-modal="true" aria-labelledby="bkTitle">
    <div class="modal-head"><h2 id="bkTitle">Back up or move settings</h2><button class="btn small" id="bkClose" type="button">Close</button></div>
    <section class="bk-sec">
      <h3>Export</h3>
      <p class="hint">Includes your seats and names, custom profiles, table names, Lab-only choices, house rules, help switches, theme and Stress test setup. A game in progress and test results aren't included.</p>
      <textarea id="bkOut" readonly rows="4" aria-label="Exported settings"></textarea>
      <div class="runrow"><button class="btn primary" id="bkCopy" type="button">Copy</button><button class="btn" id="bkDownload" type="button" hidden>Download file</button><span class="hint" id="bkCopyMsg" role="status"></span></div>
    </section>
    <section class="bk-sec">
      <h3>Import</h3>
      <p class="hint">Paste exported text or choose a saved file. You'll see what it holds before anything changes.</p>
      <textarea id="bkIn" rows="4" placeholder="Paste exported text here" aria-label="Settings to import"></textarea>
      <div class="runrow"><button class="btn" id="bkFileBtn" type="button">Choose file</button><input type="file" id="bkFile" accept=".json,.txt,application/json,text/plain" class="vh" tabindex="-1" aria-hidden="true"><button class="btn" id="bkCheck" type="button">Check</button></div>
      <div id="bkPreview" role="status"></div>
    </section>
  </div>
</div>
<script>
/* Flip 7 rules engine. Pure logic, no DOM. Shared namespace: F7. */
var F7 = (typeof F7 !== 'undefined') ? F7 : {};
(function (F7) {
  'use strict';

  // Card type index t:
  //   0..12  Number cards (value = t)
  //   13..17 +2, +4, +6, +8, +10
  //   18     x2
  //   19     Freeze
  //   20     Flip Three
  //   21     Second Chance
  const NTYPES = 22;
  const T_X2 = 18, T_FREEZE = 19, T_FLIP3 = 20, T_SECOND = 21;
  const PLUS_VALUES = [2, 4, 6, 8, 10];

  const FULL_COUNTS = (function () {
    const c = new Array(NTYPES).fill(0);
    for (let v = 0; v <= 12; v++) c[v] = Math.max(1, v);
    for (let t = 13; t <= 18; t++) c[t] = 1;
    c[T_FREEZE] = c[T_FLIP3] = c[T_SECOND] = 3;
    return c;
  })();

  const isNum = t => t <= 12;
  const isPlus = t => t >= 13 && t <= 17;
  const plusVal = t => PLUS_VALUES[t - 13];
  const isMod = t => t >= 13 && t <= 18;
  const isAction = t => t >= 19;

  function cardLabel(t) {
    if (isNum(t)) return String(t);
    if (isPlus(t)) return '+' + plusVal(t);
    if (t === T_X2) return '×2';
    if (t === T_FREEZE) return 'Freeze';
    if (t === T_FLIP3) return 'Flip Three';
    return 'Second Chance';
  }

  // Seedable RNG (mulberry32)
  function mulberry32(a) {
    return function () {
      a |= 0; a = a + 0x6D2B79F5 | 0;
      let t = Math.imul(a ^ a >>> 15, 1 | a);
      t = t + Math.imul(t ^ t >>> 7, 61 | t) ^ t;
      return ((t ^ t >>> 14) >>> 0) / 4294967296;
    };
  }
  function hashStr(s) {
    let h = 2166136261;
    for (let i = 0; i < s.length; i++) { h ^= s.charCodeAt(i); h = Math.imul(h, 16777619); }
    return h >>> 0;
  }
  function makeRng(seed) {
    if (seed === undefined || seed === null || seed === '') seed = (Math.random() * 4294967296) >>> 0;
    else if (typeof seed === 'string') seed = /^\d+$/.test(seed) ? Number(seed) : hashStr(seed);
    return mulberry32(seed >>> 0);
  }
  function shuffle(a, rng) {
    for (let i = a.length - 1; i > 0; i--) {
      const j = Math.floor(rng() * (i + 1));
      const x = a[i]; a[i] = a[j]; a[j] = x;
    }
    return a;
  }

  function buildDeck() {
    const d = [];
    let id = 0;
    for (let t = 0; t < NTYPES; t++) for (let k = 0; k < FULL_COUNTS[t]; k++) d.push({ id: id++, t });
    return d;
  }

  function newStats() {
    return { rounds: 0, busts: 0, flip7s: 0, stays: 0, frozen: 0, flip3s: 0, scoredRounds: 0, points: 0, secondUsed: 0 };
  }

  function newPlayer(seat, cfg) {
    return {
      seat, name: cfg.name || ('Player ' + (seat + 1)),
      controller: cfg.controller || 'ai', profileId: cfg.profileId != null ? cfg.profileId : null,
      total: 0, nums: [], mods: [], acts: [], pending: [], sc: null, bustCard: null,
      status: 'active', flip7: false, roundPts: 0, stats: newStats()
    };
  }

  function createGame(cfg) {
    const rng = makeRng(cfg.seed);
    const n = cfg.players.length;
    return {
      cfg, rng, quiet: !!cfg.quiet,
      target: cfg.target || 200,
      house: Object.assign({ lastMayDiscardFlip3: false, lastMayDiscardFreeze: false, opener: 'rotate' }, cfg.house || {}),
      players: cfg.players.map((p, i) => newPlayer(i, p)),
      deck: shuffle(buildDeck(), rng), discard: [], roundSeen: [],
      dealer: cfg.dealer != null ? cfg.dealer : Math.floor(rng() * n),
      order: [], round: 0, phase: 'idle', roundOver: false, deckOut: false,
      over: false, winner: -1, capped: false, history: [], turn: -1, reshuffles: 0,
      maxRounds: cfg.maxRounds || 400
    };
  }

  function ev(e, o) { o = o || {}; o.type = 'event'; o.e = e; return o; }

  function roundScore(p) {
    if (p.status === 'busted') return 0;
    let s = 0, x2 = false, plus = 0;
    for (const c of p.nums) s += c.t;
    for (const c of p.mods) { if (c.t === T_X2) x2 = true; else plus += plusVal(c.t); }
    if (x2) s *= 2;
    s += plus;
    if (p.flip7) s += 15;
    return s;
  }

  function activeSeats(S) {
    const a = [];
    for (const p of S.players) if (p.status === 'active') a.push(p.seat);
    return a;
  }

  function toDiscard(S, card) { S.discard.push(card); S.roundSeen.push(card); }

  function* drawCard(S) {
    if (S.deck.length === 0) {
      if (S.discard.length === 0) return null;
      S.deck = shuffle(S.discard, S.rng);
      S.discard = [];
      S.roundSeen = [];
      S.reshuffles++;
      if (!S.quiet) yield ev('reshuffle', { n: S.deck.length });
    }
    return S.deck.pop();
  }

  function* giveCard(S, i, ctx) {
    const card = yield* drawCard(S);
    const p = S.players[i];
    if (!card) {
      S.roundOver = true; S.deckOut = true;
      if (!S.quiet) yield ev('deckOut', {});
      return;
    }
    S.lastCard = card;
    if (!S.quiet) yield ev('draw', { p: i, card, ctx: ctx ? ctx.kind : (S.phase === 'deal' ? 'deal' : 'hit'), k: ctx ? ctx.k : 0 });
    const t = card.t;
    if (isNum(t)) { yield* applyNumber(S, i, card); return; }
    if (isMod(t)) { p.mods.push(card); return; }
    if (t === T_SECOND) { yield* handleSecond(S, i, card); return; }
    if (ctx && ctx.queue) {
      ctx.queue.push(card); p.pending.push(card);
      if (!S.quiet) yield ev('queued', { p: i, card });
      return;
    }
    yield* resolveAction(S, i, card);
  }

  function* applyNumber(S, i, card) {
    const p = S.players[i];
    const v = card.t;
    if (p.nums.some(c => c.t === v)) {
      if (p.sc) {
        const sc = p.sc; p.sc = null;
        toDiscard(S, sc); toDiscard(S, card);
        p.stats.secondUsed++;
        if (!S.quiet) yield ev('secondUsed', { p: i, card });
        return;
      }
      p.status = 'busted'; p.bustCard = card; p.stats.busts++;
      if (!S.quiet) yield ev('bust', { p: i, card });
      return;
    }
    p.nums.push(card);
    if (p.nums.length >= 7) {
      p.flip7 = true; S.roundOver = true; p.stats.flip7s++;
      if (!S.quiet) yield ev('flip7', { p: i });
    }
  }

  function* handleSecond(S, i, card) {
    const p = S.players[i];
    if (!p.sc && p.status === 'active') {
      p.sc = card;
      if (!S.quiet) yield ev('secondKept', { p: i });
      return;
    }
    const c = S.players.filter(q => q.seat !== i && q.status === 'active' && !q.sc).map(q => q.seat);
    if (c.length === 0) {
      toDiscard(S, card);
      if (!S.quiet) yield ev('secondDiscarded', { p: i });
      return;
    }
    let tgt = c[0];
    if (c.length > 1) {
      const ans = yield { type: 'decision', kind: 'giveSecond', p: i, card, options: c };
      if (c.includes(ans)) tgt = ans;
    }
    S.players[tgt].sc = card;
    if (!S.quiet) yield ev('secondGiven', { p: i, target: tgt });
  }

  function* chooseTarget(S, drawer, card) {
    const c = activeSeats(S);
    if (c.length === 0) {
      toDiscard(S, card);
      if (!S.quiet) yield ev('actionDiscarded', { p: drawer, card, why: 'none' });
      return -1;
    }
    if (c.length === 1) {
      const mayDiscard = (card.t === T_FLIP3 && S.house.lastMayDiscardFlip3) || (card.t === T_FREEZE && S.house.lastMayDiscardFreeze);
      if (c[0] === drawer && mayDiscard) {
        const ans = yield { type: 'decision', kind: 'lastDiscard', p: drawer, card, options: ['self', 'discard'] };
        if (ans === 'discard') {
          toDiscard(S, card);
          if (!S.quiet) yield ev('actionDiscarded', { p: drawer, card, why: 'house' });
          return -1;
        }
      }
      return c[0];
    }
    const ans = yield { type: 'decision', kind: 'target', p: drawer, card, options: c };
    return c.includes(ans) ? ans : c[0];
  }

  function* resolveAction(S, drawer, card) {
    const tgt = yield* chooseTarget(S, drawer, card);
    if (tgt < 0) return;
    const q = S.players[tgt];
    q.acts.push(card);
    if (card.t === T_FREEZE) {
      q.status = 'frozen'; q.stats.frozen++;
      if (!S.quiet) yield ev('freeze', { p: drawer, target: tgt });
    } else {
      q.stats.flip3s++;
      if (!S.quiet) yield ev('flip3', { p: drawer, target: tgt });
      yield* doFlipThree(S, tgt);
    }
  }

  function* doFlipThree(S, t) {
    const q = S.players[t];
    const ctx = { kind: 'flip3', queue: [], k: 0 };
    for (let k = 0; k < 3; k++) {
      if (S.roundOver || q.status !== 'active') break;
      ctx.k = k + 1;
      yield* giveCard(S, t, ctx);
    }
    if (!S.quiet) yield ev('flip3Done', { p: t });
    for (const card of ctx.queue) {
      const ix = q.pending.indexOf(card);
      if (ix >= 0) q.pending.splice(ix, 1);
      if (S.roundOver || q.status === 'busted') {
        toDiscard(S, card);
        if (!S.quiet) yield ev('actionDiscarded', { p: t, card, why: S.roundOver ? 'roundOver' : 'busted' });
        continue;
      }
      yield* resolveAction(S, t, card);
    }
  }

  function* playRound(S) {
    const n = S.players.length;
    S.round++; S.roundOver = false; S.deckOut = false; S.roundSeen = [];
    for (const p of S.players) {
      p.nums = []; p.mods = []; p.acts = []; p.pending = []; p.sc = null; p.bustCard = null;
      p.status = 'active'; p.flip7 = false; p.roundPts = 0; p.stats.rounds++;
    }
    const order = [];
    for (let k = 1; k <= n; k++) order.push((S.dealer + k) % n);
    S.order = order;
    S.phase = 'deal';
    if (!S.quiet) yield ev('roundStart', { round: S.round, dealer: S.dealer });

    for (const i of order) {
      if (S.roundOver) break;
      if (S.players[i].status !== 'active') continue;
      S.turn = i;
      yield* giveCard(S, i, null);
    }

    S.phase = 'turns';
    while (!S.roundOver) {
      let acted = false;
      for (const i of order) {
        if (S.roundOver) break;
        const p = S.players[i];
        if (p.status !== 'active') continue;
        acted = true; S.turn = i;
        const canStay = p.nums.length + p.mods.length + (p.sc ? 1 : 0) > 0;
        let choice = 'hit';
        if (canStay) choice = yield { type: 'decision', kind: 'hitStay', p: i };
        if (choice === 'stay') {
          p.status = 'stayed'; p.stats.stays++;
          if (!S.quiet) yield ev('stay', { p: i, pts: roundScore(p) });
        } else {
          yield* giveCard(S, i, null);
        }
      }
      if (!acted) break;
    }
    S.turn = -1;
    S.phase = 'scoring';

    const scores = S.players.map(roundScore);
    S.players.forEach((p, k) => {
      p.roundPts = scores[k];
      p.total += scores[k];
      if (p.status !== 'busted') { p.stats.scoredRounds++; p.stats.points += scores[k]; }
    });
    S.history.push({
      round: S.round, dealer: S.dealer, scores: scores.slice(),
      totals: S.players.map(p => p.total), statuses: S.players.map(p => p.status),
      flip7: S.players.findIndex(p => p.flip7)
    });
    if (!S.quiet) yield ev('roundEnd', { round: S.round, scores });

    // Clear the table into the discard pile
    for (const p of S.players) {
      for (const c of p.nums) S.discard.push(c);
      for (const c of p.mods) S.discard.push(c);
      for (const c of p.acts) S.discard.push(c);
      for (const c of p.pending) S.discard.push(c);
      if (p.sc) S.discard.push(p.sc);
      if (p.bustCard) S.discard.push(p.bustCard);
      p.nums = []; p.mods = []; p.acts = []; p.pending = []; p.sc = null; p.bustCard = null;
    }
    S.roundSeen = [];

    if (S.house.opener === 'winnerLeft' || S.house.opener === 'winner') {
      // Top scorer this round; ties go to whoever came first in turn order
      let best = -1, bi = S.dealer;
      for (const i of order) if (scores[i] > best) { best = scores[i]; bi = i; }
      // Play starts left of the dealer, so 'winner' seats the dealer on the top scorer's right
      S.dealer = S.house.opener === 'winner' ? (bi - 1 + n) % n : bi;
    } else {
      S.dealer = (S.dealer + 1) % n;
    }
    S.phase = 'between';
  }

  function* playGame(S) {
    while (!S.over) {
      yield* playRound(S);
      let max = -Infinity;
      for (const p of S.players) if (p.total > max) max = p.total;
      if (max >= S.target) {
        const leaders = S.players.filter(p => p.total === max);
        if (leaders.length === 1) { S.over = true; S.winner = leaders[0].seat; }
        else if (!S.quiet) yield ev('tie', { seats: leaders.map(p => p.seat), score: max });
      }
      if (!S.over && S.round >= S.maxRounds) {
        const leaders = S.players.filter(p => p.total === max);
        S.over = true; S.capped = true;
        S.winner = leaders[Math.floor(S.rng() * leaders.length)].seat;
      }
    }
    S.phase = 'over';
    if (!S.quiet) yield ev('gameEnd', { winner: S.winner });
  }

  Object.assign(F7, {
    NTYPES, T_X2, T_FREEZE, T_FLIP3, T_SECOND, PLUS_VALUES, FULL_COUNTS,
    isNum, isPlus, plusVal, isMod, isAction, cardLabel,
    makeRng, shuffle, buildDeck, createGame, playGame, playRound, roundScore, activeSeats
  });
})(F7);

/* Flip 7 AI: adjustable player profiles, decision heuristics, advisor explanations. */
(function (F7) {
  'use strict';
  const { NTYPES, T_X2, T_FREEZE, T_FLIP3, T_SECOND, FULL_COUNTS, PLUS_VALUES, roundScore } = F7;

  const DEFAULTS = {
    model: 'threshold',   // 'threshold' | 'ev' | 'random'
    riskCap: 0.25,        // threshold: hit only while bust chance <= this
    bankAt: 0,            // threshold: stay once round score >= this (0 = off)
    bankWith: 0,          // threshold: once holding this many numbers, stay at a lower target (0 = off)
    bankWithFloor: 0.5,   // threshold: that lower target, as a share of Bank at (Bank at off = no floor)
    evDepth: 2,           // ev: cards of lookahead (1-3)
    evMargin: 0,          // ev: hit only if hit value >= stay value * (1 + margin)
    stayStart: 0.1,       // random: chance to stay holding one number card
    stayStep: 0.15,       // random: added to that chance for each further number card
    counting: 'table',    // 'none' | 'table' | 'limited' | 'full'
    countBandwidth: 2,    // limited: memory tiers tracked exactly since the shuffle (1-12)
    chase7At: 0,          // with this many unique numbers, push harder for Flip 7 (0 = off)
    chaseBoost: 0.2,      // how much harder: + to risk cap, - to EV margin
    endgame: true,        // reads the race to the target score
    comeback: 0,          // 0-1: extra aggression when trailing late
    targeting: 'smart'    // 'smart' | 'leader' | 'random'
  };

  const PRESETS = [
    { id: 'cautious', name: 'Cautious', desc: 'Stops at 15% bust risk or 20 points. Counts only its own cards and aims Action cards at the leader.',
      model: 'threshold', riskCap: 0.15, bankAt: 20, counting: 'none', endgame: false, targeting: 'leader' },
    { id: 'balanced', name: 'Balanced', desc: 'Hits up to 25% bust risk, banks at 30. Tracks cards on the table and reads the endgame.',
      model: 'threshold', riskCap: 0.25, bankAt: 30, counting: 'table', endgame: true, targeting: 'smart' },
    { id: 'bold', name: 'Bold', desc: 'Hits up to 40% bust risk with no point target. Reads the endgame.',
      model: 'threshold', riskCap: 0.40, bankAt: 0, counting: 'table', endgame: true, targeting: 'smart' },
    { id: 'bank25', name: 'Bank at 25', desc: 'Ignores the odds entirely: hits until the round is worth 25, then stays.',
      model: 'threshold', riskCap: 1, bankAt: 25, counting: 'none', endgame: false, targeting: 'leader' },
    { id: 'counter', name: 'Card Counter', desc: 'Tracks every card since the last shuffle and hits whenever a two-card lookahead says hitting is worth more than staying. Gets bolder when trailing late.',
      model: 'ev', evDepth: 2, evMargin: 0, counting: 'full', endgame: true, comeback: 0.5, targeting: 'smart' },
    { id: 'chaser', name: 'Seven Chaser', desc: 'Plays like Balanced without a point target, then raises its risk cap by 20 points once it holds five different numbers.',
      model: 'threshold', riskCap: 0.27, bankAt: 0, chase7At: 5, chaseBoost: 0.2, counting: 'table', endgame: true, targeting: 'smart' },
    { id: 'coinflip', name: 'Coin Flip', desc: 'Baseline for testing: decides at random, staying more often the more numbers it holds (10% with one, 15 points more for each number after that). Ignores the odds and targets at random.',
      model: 'random', stayStart: 0.1, stayStep: 0.15, counting: 'none', endgame: false, targeting: 'random' }
  ].map(p => Object.assign({}, DEFAULTS, p, { builtin: true }));

  function withDefaults(p) { return Object.assign({}, DEFAULTS, p); }

  // ---------- card knowledge ----------
  function addCards(arr, cards, sign) { for (const c of cards) arr[c.t] += sign; }

  // Memory tiers for 'limited' counting: the bandwidth level at which each card type is tracked exactly.
  // Level 1: 0, 1, x2, Second Chance. Level 2 adds 2, +10, Freeze. Level 3 adds 3, +8, Flip Three.
  // Level 4 adds 4, +6. Level 5 adds 5, +4. Level 6 adds 6, +2. Levels 7-12 add 7-12 (12 = full count).
  const TIER = (function () {
    const a = new Array(NTYPES).fill(99);
    a[0] = 1; a[1] = 1;
    for (let v = 2; v <= 12; v++) a[v] = v;
    a[T_X2] = 1; a[T_SECOND] = 1;
    a[17] = 2; a[T_FREEZE] = 2;
    a[16] = 3; a[T_FLIP3] = 3;
    a[15] = 4; a[14] = 5; a[13] = 6;
    return a;
  })();
  const MAX_BANDWIDTH = 12;

  function unseenCounts(S, i, mode, bandwidth) {
    if (mode === 'full') {
      const c = new Array(NTYPES).fill(0);
      for (const card of S.deck) c[card.t]++;
      return c;
    }
    if (mode === 'limited') {
      // Tracked types: exact count left in the draw pile since the shuffle.
      // Untracked types: only this round's table is known; the visible pile size is
      // shared out among them in proportion to that estimate. Counts may be fractional.
      const level = Math.max(1, Math.min(MAX_BANDWIDTH, Math.round(bandwidth) || 1));
      const est = tableEstimate(S, i, true);
      const c = new Array(NTYPES).fill(0);
      for (const card of S.deck) if (TIER[card.t] <= level) c[card.t]++;
      let tracked = 0, estSum = 0;
      for (let t = 0; t < NTYPES; t++) { if (TIER[t] <= level) tracked += c[t]; else estSum += est[t]; }
      const k = estSum > 0 ? Math.max(0, S.deck.length - tracked) / estSum : 0;
      for (let t = 0; t < NTYPES; t++) if (TIER[t] > level) c[t] = est[t] * k;
      return c;
    }
    return tableEstimate(S, i, mode === 'table');
  }

  function tableEstimate(S, i, wholeTable) {
    const c = new Array(NTYPES).fill(0);
    for (let t = 0; t < NTYPES; t++) c[t] = FULL_COUNTS[t];
    const subtractPlayer = q => {
      addCards(c, q.nums, -1); addCards(c, q.mods, -1); addCards(c, q.acts, -1); addCards(c, q.pending, -1);
      if (q.sc) c[q.sc.t]--;
      if (q.bustCard) c[q.bustCard.t]--;
    };
    if (wholeTable) {
      for (const q of S.players) subtractPlayer(q);
      addCards(c, S.roundSeen, -1);
    } else {
      subtractPlayer(S.players[i]);
    }
    for (let t = 0; t < NTYPES; t++) if (c[t] < 0) c[t] = 0;
    return c;
  }
  const sumArr = a => { let s = 0; for (const x of a) s += x; return s; };

  // Compact hand summary
  function handOf(p) {
    let mask = 0, sum = 0, n = 0, x2 = false, plus = 0;
    for (const c of p.nums) { mask |= 1 << c.t; sum += c.t; n++; }
    for (const c of p.mods) { if (c.t === T_X2) x2 = true; else plus += PLUS_VALUES[c.t - 13]; }
    return { mask, sum, n, x2, plus, sc: !!p.sc };
  }
  function scoreOf(h) { return (h.x2 ? h.sum * 2 : h.sum) + h.plus; }
  function bustChance(h, counts, N) {
    if (N <= 0) return 0;
    let b = 0;
    for (let v = 0; v <= 12; v++) if (h.mask & (1 << v)) b += counts[v];
    return b / N;
  }

  // Expectimax over your own next draws. Opponents' draws are ignored.
  function V(h, counts, N, d) {
    const st = scoreOf(h);
    if (d <= 0 || N <= 0) return st;
    const hv = hitValue(h, counts, N, d);
    return hv > st ? hv : st;
  }
  function hitValue(h, counts, N, d) {
    if (N <= 0) return scoreOf(h);
    let tot = 0;
    for (let t = 0; t < NTYPES; t++) {
      const c = counts[t];
      if (!(c > 0)) continue;
      const pr = c / N;
      const take = c < 1 ? c : 1; // estimated counts can be fractional
      const N2 = N - take;
      counts[t] -= take;
      let val;
      if (t <= 12) {
        if (h.mask & (1 << t)) {
          val = h.sc ? V({ mask: h.mask, sum: h.sum, n: h.n, x2: h.x2, plus: h.plus, sc: false }, counts, N2, d - 1) : 0;
        } else {
          const h2 = { mask: h.mask | (1 << t), sum: h.sum + t, n: h.n + 1, x2: h.x2, plus: h.plus, sc: h.sc };
          val = h2.n >= 7 ? scoreOf(h2) + 15 : V(h2, counts, N2, d - 1);
        }
      } else if (t <= 17) {
        val = V({ mask: h.mask, sum: h.sum, n: h.n, x2: h.x2, plus: h.plus + PLUS_VALUES[t - 13], sc: h.sc }, counts, N2, d - 1);
      } else if (t === T_X2) {
        val = V({ mask: h.mask, sum: h.sum, n: h.n, x2: true, plus: h.plus, sc: h.sc }, counts, N2, d - 1);
      } else if (t === T_SECOND) {
        val = V(h.sc ? h : { mask: h.mask, sum: h.sum, n: h.n, x2: h.x2, plus: h.plus, sc: true }, counts, N2, d - 1);
      } else {
        val = V(h, counts, N2, d - 1); // Freeze / Flip Three: assume aimed elsewhere
      }
      counts[t] += take;
      tot += pr * val;
    }
    return tot;
  }

  const pct = x => Math.round(x * 100) + '%';
  const nm = (S, i) => S.players[i].name;

  // ---------- race awareness ----------
  function raceInfo(S, i) {
    const me = S.players[i];
    let lockedBest = -1, lockedSeat = -1, otherBest = -1, otherSeat = -1, leaderTotal = -1;
    for (const q of S.players) {
      if (q.seat === i) continue;
      const qs = q.status === 'busted' ? 0 : roundScore(q);
      const qf = q.total + qs;
      if (q.status !== 'active' && qf > lockedBest) { lockedBest = qf; lockedSeat = q.seat; }
      if (qf > otherBest) { otherBest = qf; otherSeat = q.seat; }
      if (q.total > leaderTotal) leaderTotal = q.total;
    }
    return { lockedBest, lockedSeat, otherBest, otherSeat, leaderTotal, deficit: leaderTotal - me.total };
  }

  function effectiveKnobs(S, i, prof, h) {
    let riskCap = prof.riskCap, bankAt = prof.bankAt, margin = prof.evMargin;
    const notes = [];
    let chasing = false;
    if (prof.chase7At > 0 && h.n >= prof.chase7At && h.n < 7) {
      chasing = true;
      riskCap += prof.chaseBoost; margin -= prof.chaseBoost;
      notes.push('chasing Flip 7 with ' + h.n + ' numbers');
    }
    if (prof.comeback > 0) {
      const r = raceInfo(S, i);
      if (r.deficit > 0 && r.leaderTotal >= 0.5 * S.target) {
        const f = prof.comeback * Math.min(1, r.deficit / 80);
        riskCap += 0.25 * f; margin -= 0.3 * f;
        if (bankAt > 0) bankAt += Math.round(20 * f);
        if (f > 0.05) notes.push('trailing by ' + r.deficit);
      }
    }
    return { riskCap, bankAt, margin, notes, chasing };
  }

  // ---------- Hit or Stay ----------
  function decideHitStay(S, i, prof, rng) {
    prof = withDefaults(prof);
    const p = S.players[i];
    const h = handOf(p);
    const counts = unseenCounts(S, i, prof.counting, prof.countBandwidth);
    const N = sumArr(counts);
    const cur = scoreOf(h);
    const rawBust = bustChance(h, counts, N);
    const bust = h.sc ? 0 : rawBust;
    const info = { cur, bust: rawBust, protectedBySecond: h.sc };

    if (prof.endgame) {
      const r = raceInfo(S, i);
      const myFinal = p.total + cur;
      if (r.lockedBest >= S.target && myFinal < r.lockedBest) {
        return { choice: 'hit', info, reason: `Staying at ${myFinal} loses to ${nm(S, r.lockedSeat)}'s ${r.lockedBest}, so hitting is the only way to win.` };
      }
      if (myFinal >= S.target && myFinal > r.otherBest) {
        return { choice: 'stay', info, reason: `Staying finishes at ${myFinal}, ahead of everyone. The rest have to beat it.` };
      }
    }

    // A free card: the next card can't bust it, so staying only throws points away.
    // Applies to every profile. (Staying to lock in a win is handled above for profiles that read the endgame.)
    if (h.sc) {
      return { choice: 'hit', info, reason: `Free card: its Second Chance cancels a duplicate, and staying would just discard it.` };
    }
    if (rawBust < 1e-9) {
      return { choice: 'hit', info, reason: h.n === 0
        ? `Free card: with no number cards it can't bust.`
        : `Free card: no copies of its numbers are left to draw, so it can't bust.` };
    }

    const k = effectiveKnobs(S, i, prof, h);
    const why = k.notes.length ? ' (' + k.notes.join(', ') + ')' : '';

    if (prof.model === 'random') {
      const stayP = Math.max(0, Math.min(1, prof.stayStart + prof.stayStep * Math.max(0, h.n - 1)));
      const stay = rng() < stayP;
      return { choice: stay ? 'stay' : 'hit', info, reason: `Random: ${pct(stayP)} chance to stay holding ${h.n} number${h.n === 1 ? '' : 's'}.` };
    }

    if (prof.model === 'ev') {
      const hv = hitValue(h, counts, N, Math.max(1, Math.min(3, prof.evDepth | 0)));
      info.hitValue = hv;
      const need = cur * (1 + k.margin);
      if (hv > need + 1e-9) {
        return { choice: 'hit', info, reason: `Hitting is worth about ${hv.toFixed(1)} vs ${cur} for staying${why}.` };
      }
      return { choice: 'stay', info, reason: `Staying (${cur}) beats hitting (about ${hv.toFixed(1)})${why}.` };
    }

    // threshold model
    if (k.bankAt > 0 && cur >= k.bankAt) {
      return { choice: 'stay', info, reason: `Round is worth ${cur}, at or above its bank target of ${k.bankAt}${why}.` };
    }
    let lowHand = '';
    if (prof.bankWith > 0 && h.n >= prof.bankWith && !k.chasing) {
      const floor = Math.round(Math.max(0, k.bankAt) * prof.bankWithFloor);
      if (cur >= floor) {
        return { choice: 'stay', info, reason: floor > 0
          ? `Holds ${h.n} numbers (bank with ${prof.bankWith}) and the round is worth ${cur}, at or above its low-hand floor of ${floor}${why}.`
          : `Holds ${h.n} numbers, its bank-with limit of ${prof.bankWith}${why}.` };
      }
      lowHand = ` (${h.n} numbers, but only ${cur} points, under its low-hand floor of ${floor})`;
    }
    if (bust <= k.riskCap + 1e-9) {
      return { choice: 'hit', info, reason: `Bust chance ${pct(bust)} is within its ${pct(Math.min(1, k.riskCap))} cap${why}${lowHand}.` };
    }
    return { choice: 'stay', info, reason: `Bust chance ${pct(bust)} is over its ${pct(k.riskCap)} cap${why}.` };
  }

  // ---------- Action targeting ----------
  function threat(S, j) {
    const tots = S.players.map(p => p.total);
    const max = Math.max.apply(null, tots);
    const mean = sumArr(tots) / tots.length;
    const q = S.players[j];
    let w = 1 + Math.max(0, q.total - mean) / (S.target * 0.25);
    if (q.total === max && max > 0) w += 0.5;
    if (q.total >= 0.75 * S.target) w += 0.5;
    return w;
  }

  // Rough odds for a player forced to take three cards
  function flip3Odds(S, j, counts, N) {
    const q = S.players[j];
    const h = handOf(q);
    let H = 0, numCnt = 0, numVal = 0, extra = 0;
    for (let v = 0; v <= 12; v++) {
      const c = counts[v];
      if (h.mask & (1 << v)) H += c;
      else { numCnt += c; numVal += c * v; extra += c * Math.max(0, c - 1); }
    }
    const qAdd = numCnt ? extra / numCnt : 0;
    let plusEV = 0;
    for (let t = 13; t <= 17; t++) plusEV += counts[t] * PLUS_VALUES[t - 13];
    plusEV = N ? plusEV / N : 0;
    let alive = [1, 0, 0, 0], bust = 0, f7 = 0;
    for (let k = 0; k < 3; k++) {
      const nxt = [0, 0, 0, 0];
      const left = Math.max(1, N - k);
      for (let m = 0; m <= k; m++) {
        const pr = alive[m];
        if (!pr) continue;
        const pd = Math.min(0.95, (H + m * qAdd) / left);
        const pn = Math.min(1 - pd, Math.max(0, numCnt - m * 6) / left);
        const po = Math.max(0, 1 - pd - pn);
        bust += pr * pd;
        nxt[m] += pr * po;
        if (h.n + m + 1 >= 7) f7 += pr * pn; else nxt[m + 1] += pr * pn;
      }
      alive = nxt;
    }
    if (h.sc) bust *= 0.25;
    const aliveSum = sumArr(alive) || 1e-9;
    let expNew = 0;
    for (let m = 0; m < 4; m++) expNew += m * alive[m];
    expNew /= aliveSum;
    const avgNew = numCnt ? numVal / numCnt : 0;
    const gain = avgNew * expNew * (h.x2 ? 2 : 1) + 3 * plusEV;
    return { bust: Math.min(1, bust), flip7: f7, gain, cur: scoreOf(h) };
  }

  function rankTargets(S, drawer, card, prof) {
    prof = withDefaults(prof);
    const counts = unseenCounts(S, drawer, prof.counting === 'none' ? 'table' : prof.counting, prof.countBandwidth);
    const N = sumArr(counts);
    const T = S.target;
    const cands = F7.activeSeats(S);
    const out = [];
    const me = S.players[drawer];
    for (const j of cands) {
      const q = S.players[j];
      const h = handOf(q);
      const cur = scoreOf(h);
      const self = j === drawer;
      let value, reason;
      if (card.t === T_FREEZE) {
        const bp = h.sc ? 0 : bustChance(h, counts, N);
        if (self) {
          const d = decideHitStay(S, j, Object.assign({}, prof, { model: prof.model === 'random' ? 'threshold' : prof.model }), Math.random);
          if (d.choice === 'stay') {
            value = 3 + 0.15 * cur; reason = `You'd stay anyway; freezing yourself banks ${cur} and keeps you out of Flip Three range.`;
            if (me.total + cur >= T) { value += 40; reason = `Banks ${me.total + cur}, which reaches ${T}.`; }
          } else {
            value = -Math.max(0, 22 - cur) * (1 - bp); reason = `You'd still want to draw; this ends your round at ${cur}.`;
          }
        } else {
          const potential = Math.max(0, 22 - cur) * (1 - bp);
          value = threat(S, j) * (potential - 0.6 * bp * cur);
          if (q.total + cur >= T) { value -= 50; reason = `Freezing ${q.name} locks in ${q.total + cur}, past ${T}. Avoid.`; }
          else if (cur <= 8) reason = `${q.name} has only ${cur} this round; freezing caps the round there.`;
          else if (bp >= 0.3) reason = `${q.name} faces a ${pct(bp)} bust chance; freezing would rescue ${cur} points.`;
          else reason = `Caps ${q.name} at ${cur} (bust chance ${pct(bp)}).`;
        }
      } else { // Flip Three
        const o = flip3Odds(S, j, counts, N);
        if (self) {
          value = (1 - o.bust) * o.gain + o.flip7 * 15 - o.bust * cur;
          const r = raceInfo(S, drawer);
          if (r.lockedBest >= T && me.total + cur < r.lockedBest) value += 20;
          reason = `Three cards for you: about ${pct(o.bust)} to bust, about +${o.gain.toFixed(0)} if you survive.`;
        } else {
          value = threat(S, j) * (o.bust * cur - (1 - o.bust) * o.gain * 0.8 - o.flip7 * (15 + o.gain));
          if (q.total + cur >= T) value += 30 * o.bust;
          reason = `${q.name}: about ${pct(o.bust)} to bust and lose ${cur}${o.flip7 > 0.02 ? `, ${pct(o.flip7)} to hit Flip 7` : ''}; otherwise about +${o.gain.toFixed(0)} for them.`;
        }
      }
      out.push({ seat: j, value, reason, self });
    }
    out.sort((a, b) => b.value - a.value);
    return out;
  }

  function decideTarget(S, req, prof, rng) {
    prof = withDefaults(prof);
    const opts = req.options;
    if (prof.targeting === 'random') {
      const t = opts[Math.floor(rng() * opts.length)];
      return { choice: t, reason: 'Random target.' };
    }
    if (prof.targeting === 'leader') {
      const others = opts.filter(j => j !== req.p);
      if (!others.length) return { choice: opts[0], reason: 'No one else is active.' };
      others.sort((a, b) => (S.players[b].total + roundScore(S.players[b])) - (S.players[a].total + roundScore(S.players[a])));
      return { choice: others[0], reason: `Aimed at the leader, ${nm(S, others[0])}.` };
    }
    const ranked = rankTargets(S, req.p, req.card, prof);
    return { choice: ranked[0].seat, reason: ranked[0].reason, ranked };
  }

  function decideGiveSecond(S, req, prof, rng) {
    prof = withDefaults(prof);
    const opts = req.options.slice();
    if (prof.targeting === 'random') return { choice: opts[Math.floor(rng() * opts.length)], reason: 'Random recipient.' };
    opts.sort((a, b) => (S.players[a].total + roundScore(S.players[a])) - (S.players[b].total + roundScore(S.players[b])));
    return { choice: opts[0], reason: `Gives it to ${nm(S, opts[0])}, the smallest threat.` };
  }

  function decideLastDiscard(S, req, prof, rng) {
    prof = withDefaults(prof);
    if (prof.targeting === 'random') return { choice: rng() < 0.5 ? 'self' : 'discard', reason: 'Random.' };
    if (req.card.t === T_FREEZE) {
      return { choice: 'discard', reason: 'Discards the Freeze: it can still stay on its next turn, so taking it gains nothing.' };
    }
    const ranked = rankTargets(S, req.p, req.card, prof);
    const v = ranked.length ? ranked[0].value : -1;
    if (v >= 0) return { choice: 'self', reason: 'Three more cards are worth the risk: ' + ranked[0].reason };
    return { choice: 'discard', reason: 'Discards it: ' + (ranked[0] ? ranked[0].reason : '') };
  }

  function decide(S, req, prof, rng) {
    rng = rng || Math.random;
    switch (req.kind) {
      case 'hitStay': return decideHitStay(S, req.p, prof, rng);
      case 'target': return decideTarget(S, req, prof, rng);
      case 'giveSecond': return decideGiveSecond(S, req, prof, rng);
      case 'lastDiscard': return decideLastDiscard(S, req, prof, rng);
    }
    throw new Error('Unknown decision ' + req.kind);
  }

  // Advisor numbers for a seat (always uses a full count; the human can see every card)
  function analyzeSeat(S, i, depth) {
    const p = S.players[i];
    const h = handOf(p);
    const full = unseenCounts(S, i, 'full');
    const naive = unseenCounts(S, i, 'none');
    const N = sumArr(full);
    const cur = scoreOf(h);
    const r = {
      cur, n: h.n, sc: h.sc, deckSize: N,
      bustFull: bustChance(h, full, N),
      bustNaive: bustChance(h, naive, sumArr(naive)),
      oneCard: hitValue(h, full.slice(), N, 1),
      look: hitValue(h, full.slice(), N, depth || 3)
    };
    let pf7 = 0;
    if (h.n === 6 && N) {
      for (let v = 0; v <= 12; v++) if (!(h.mask & (1 << v))) pf7 += full[v];
      pf7 /= N;
    }
    r.flip7Next = pf7;
    return r;
  }

  F7.AI = { DEFAULTS, PRESETS, TIER, MAX_BANDWIDTH, withDefaults, decide, decideHitStay, rankTargets, analyzeSeat, unseenCounts, handOf, scoreOf, bustChance, hitValue, flip3Odds };
})(F7);

/* Flip 7 batch simulator: plays AI-only games quickly and aggregates results. */
(function (F7) {
  'use strict';

  function runGame(entrants, opts, seed) {
    const rng = F7.makeRng(seed);
    const n = entrants.length;
    const seating = [];
    for (let k = 0; k < n; k++) seating.push(k);
    if (opts.rotate !== false) F7.shuffle(seating, rng);
    const players = seating.map(k => ({ name: entrants[k].name, controller: 'ai', profileId: k }));
    const S = F7.createGame({
      players, target: opts.target || 200, house: opts.house || {}, quiet: true,
      seed: Math.floor(rng() * 4294967296), maxRounds: opts.maxRounds || 400
    });
    const aiRng = F7.makeRng(Math.floor(rng() * 4294967296));
    const profs = seating.map(k => entrants[k].profile);
    const g = F7.playGame(S);
    let r = g.next();
    while (!r.done) {
      const v = r.value;
      let ans;
      if (v.type === 'decision') ans = F7.AI.decide(S, v, profs[v.p], aiRng).choice;
      r = g.next(ans);
    }
    return { S, seating };
  }

  function emptyAgg(entrants) {
    return {
      games: 0, rounds: 0, capped: 0,
      rows: entrants.map(e => ({
        name: e.name, profileName: e.profile.name, wins: 0, totalFinal: 0, rounds: 0, busts: 0, flip7s: 0,
        stays: 0, frozen: 0, flip3s: 0, scoredRounds: 0, points: 0, seatWins: {}
      }))
    };
  }

  function addGame(agg, res) {
    const { S, seating } = res;
    agg.games++;
    agg.rounds += S.round;
    if (S.capped) agg.capped++;
    S.players.forEach((p, seat) => {
      const row = agg.rows[seating[seat]];
      row.totalFinal += p.total;
      const st = p.stats;
      row.rounds += st.rounds; row.busts += st.busts; row.flip7s += st.flip7s; row.stays += st.stays;
      row.frozen += st.frozen; row.flip3s += st.flip3s; row.scoredRounds += st.scoredRounds; row.points += st.points;
      if (S.winner === seat) row.wins++;
    });
  }

  // Wilson score interval
  function wilson(k, n, z) {
    z = z || 1.96;
    if (!n) return { p: 0, lo: 0, hi: 0 };
    const p = k / n, z2 = z * z;
    const den = 1 + z2 / n;
    const center = (p + z2 / (2 * n)) / den;
    const half = z * Math.sqrt(p * (1 - p) / n + z2 / (4 * n * n)) / den;
    return { p, lo: Math.max(0, center - half), hi: Math.min(1, center + half) };
  }

  function summarize(agg) {
    return {
      games: agg.games, avgRounds: agg.games ? agg.rounds / agg.games : 0, capped: agg.capped,
      rows: agg.rows.map(r => {
        const ci = wilson(r.wins, agg.games);
        return {
          name: r.name, profileName: r.profileName, wins: r.wins, winRate: ci.p, lo: ci.lo, hi: ci.hi,
          avgFinal: agg.games ? r.totalFinal / agg.games : 0,
          ptsPerRound: r.rounds ? r.points / r.rounds : 0,
          bustRate: r.rounds ? r.busts / r.rounds : 0,
          flip7Rate: r.rounds ? r.flip7s / r.rounds : 0,
          frozenRate: r.rounds ? r.frozen / r.rounds : 0,
          flip3Rate: r.rounds ? r.flip3s / r.rounds : 0
        };
      })
    };
  }

  // A batch that can run in time-sliced steps (browser) or all at once (Node)
  function createBatch(entrants, opts, nGames) {
    const baseSeed = opts.seed !== undefined && opts.seed !== '' ? opts.seed : String(Math.random());
    const seedRng = F7.makeRng(baseSeed);
    const agg = emptyAgg(entrants);
    let done = 0;
    return {
      agg,
      get done() { return done; },
      total: nGames,
      step(budgetMs) {
        const t0 = Date.now();
        while (done < nGames) {
          addGame(agg, runGame(entrants, opts, Math.floor(seedRng() * 4294967296)));
          done++;
          if (budgetMs !== undefined && Date.now() - t0 >= budgetMs) break;
        }
        return done >= nGames;
      },
      summary() { return summarize(agg); }
    };
  }

  function runBatch(entrants, opts, nGames) {
    const b = createBatch(entrants, opts, nGames);
    b.step();
    return b.summary();
  }

  F7.Sim = { runGame, createBatch, runBatch, wilson, summarize };
})(F7);

if (typeof module !== 'undefined' && module.exports) module.exports = F7;

/* Flip 7 Strategy Lab: UI */
(function () {
  'use strict';
  const $ = (s, r) => (r || document).querySelector(s);
  const $$ = (s, r) => Array.from((r || document).querySelectorAll(s));
  const esc = s => String(s).replace(/[&<>"']/g, c => ({ '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#39;' }[c]));
  const pct = (x, d) => (x * 100).toFixed(d || 0) + '%';
  const store = {
    get(k, d) { try { const v = localStorage.getItem(k); return v ? JSON.parse(v) : d; } catch (e) { return d; } },
    set(k, v) { try { localStorage.setItem(k, JSON.stringify(v)); } catch (e) { /* storage unavailable */ } }
  };
  const STORE_KEY = 'f7lab.v1';
  const NAMES = ['Ava', 'Ben', 'Cal', 'Dee', 'Eli', 'Fay', 'Gus', 'Hal', 'Ivy', 'Jo', 'Kit', 'Lou', 'Max', 'Ned', 'Ona', 'Pia',
    'Quin', 'Rae', 'Sam', 'Tess', 'Uma', 'Vic', 'Wes', 'Yara', 'Zed', 'Ada', 'Bo', 'Cyd', 'Dot', 'Edie', 'Finn', 'Gia',
    'Hank', 'Iris', 'Jude', 'Kai', 'Lena', 'Mo', 'Nia', 'Otto'];
  const DEFAULT_PROFILE_CYCLE = ['counter', 'balanced', 'bold', 'chaser', 'cautious', 'bank25', 'balanced', 'bold'];
  const SPEEDS = { instant: 0, fast: 260, normal: 650, slow: 1200 };
  const MODEL_LABEL = { threshold: 'Rules of thumb', ev: 'Expected value', random: 'Random' };

  // ---------------- state ----------------
  let customProfiles = [];
  let settings = {
    seats: [
      { name: 'You', kind: 'human', profileId: 'balanced' },
      { name: 'Ava', kind: 'ai', profileId: 'counter' },
      { name: 'Ben', kind: 'ai', profileId: 'balanced' },
      { name: 'Cal', kind: 'ai', profileId: 'bold' }
    ],
    target: 200, speed: 'normal', showOdds: true, showCount: true, showAdvisor: true, explain: true, peek: false,
    rerollBetween: false, theme: 'dark',
    house: { lastMayDiscardFlip3: false, lastMayDiscardFreeze: false, opener: 'rotate' }
  };
  let simSettings = {
    entrants: [
      { name: 'Card Counter', profileId: 'counter' },
      { name: 'Balanced', profileId: 'balanced' },
      { name: 'Bold', profileId: 'bold' },
      { name: 'Seven Chaser', profileId: 'chaser' }
    ],
    games: 2000, target: 200, seed: '', rotate: true,
    house: { lastMayDiscardFlip3: false, lastMayDiscardFreeze: false, opener: 'rotate' },
    sweep: { seat: 1, param: 'riskCap', from: 0.1, to: 0.5, step: 0.05, games: 600 },
    ladder: { base: 'balanced', param: 'bankAt', from: 15, to: 40, step: 5 }
  };
  let hiddenProfiles = [];
  let profileNames = {};      // profile id -> table name (optional)
  let currentTab = 'play';
  let selectedProfileId = 'counter';

  function allProfiles() { return F7.AI.PRESETS.concat(customProfiles); }
  function profileById(id) { return allProfiles().find(p => p.id === id) || F7.AI.PRESETS.find(p => p.id === 'balanced'); }
  function persist() { store.set(STORE_KEY, { settings, simSettings, customProfiles, hiddenProfiles, profileNames }); }
  const isAvailable = p => !hiddenProfiles.includes(p.id);
  function tableProfiles() {
    const a = allProfiles().filter(isAvailable);
    return a.length ? a : allProfiles();
  }
  const low = x => (x || '').trim().toLowerCase();
  const tableNameOf = id => (profileNames[id] || '').trim();
  function namesTaken(exceptIdx) {
    return new Set(settings.seats.filter((x, i) => i !== exceptIdx).map(x => low(x.name)));
  }
  // A random name that isn't at the table and isn't any profile's table name
  function poolName(taken) {
    const reserved = new Set(Object.values(profileNames).map(low).filter(Boolean));
    const free = NAMES.filter(n => !taken.has(low(n)) && !reserved.has(low(n)));
    if (free.length) return free[Math.floor(Math.random() * free.length)];
    let k = 1;
    while (taken.has('player ' + k)) k++;
    return 'Player ' + k;
  }
  function nameFor(pid, taken) {
    const tn = tableNameOf(pid);
    if (!tn) return poolName(taken);
    if (!taken.has(low(tn))) return tn;
    let k = 2;
    while (taken.has(low(tn + ' ' + k))) k++;
    return tn + ' ' + k;
  }
  // A named profile stands for one person, so it sits at most once per table
  function pickProfile(taken, namedInUse) {
    const avail = tableProfiles();
    let c = avail.filter(p => !(tableNameOf(p.id) && (namedInUse.has(p.id) || taken.has(low(tableNameOf(p.id))))));
    if (!c.length) c = avail;
    return c[Math.floor(Math.random() * c.length)].id;
  }
  function namedAtTable(exceptIdx) {
    const out = new Set();
    settings.seats.forEach((x, i) => { if (i !== exceptIdx && x.kind === 'ai' && tableNameOf(x.profileId)) out.add(x.profileId); });
    return out;
  }
  function addAiSeat() {
    const taken = namesTaken(-1);
    const pid = pickProfile(taken, namedAtTable(-1));
    settings.seats.push({ kind: 'ai', profileId: pid, name: nameFor(pid, taken) });
  }

  // ---------------- theme ----------------
  function applyTheme() {
    const light = settings.theme === 'light';
    document.documentElement.classList.toggle('light', light);
    const b = $('#themeBtn');
    if (b) { b.textContent = light ? 'Switch to dark mode' : 'Switch to light mode'; b.setAttribute('aria-pressed', String(light)); }
  }

  function profileOptionsHTML(selected) {
    const pre = F7.AI.PRESETS.map(p => `<option value="${p.id}"${p.id === selected ? ' selected' : ''}>${esc(p.name)}</option>`).join('');
    const cus = customProfiles.map(p => `<option value="${p.id}"${p.id === selected ? ' selected' : ''}>${esc(p.name)}</option>`).join('');
    return `<optgroup label="Built-in">${pre}</optgroup>` + (cus ? `<optgroup label="Custom">${cus}</optgroup>` : '');
  }

  // ---------------- tooltip ----------------
  const tip = () => $('#tip');
  let tipTimer = null;
  function showTip(el, cx, cy) {
    const t = tip();
    t.innerHTML = el.getAttribute('data-tip');
    t.hidden = false;
    const w = t.offsetWidth, h = t.offsetHeight;
    let x = cx + 14, y = cy + 14;
    if (x + w > window.innerWidth - 8) x = Math.max(8, cx - w - 14);
    if (y + h > window.innerHeight - 8) y = Math.max(8, cy - h - 14);
    t.style.left = x + 'px'; t.style.top = y + 'px';
  }
  document.addEventListener('mousemove', e => {
    if (tipTimer) return; // a tap-opened tip stays until its timer ends
    const el = e.target.closest ? e.target.closest('[data-tip]') : null;
    const t = tip();
    if (!t) return;
    if (!el) { t.hidden = true; return; }
    showTip(el, e.clientX, e.clientY);
  });
  // Touch screens have no hover: a tap shows the tip for a few seconds
  document.addEventListener('pointerdown', e => {
    if (e.pointerType === 'mouse') return;
    const el = e.target.closest ? e.target.closest('[data-tip]') : null;
    const t = tip();
    if (!t) return;
    clearTimeout(tipTimer); tipTimer = null;
    if (!el) { t.hidden = true; return; }
    showTip(el, e.clientX, e.clientY);
    tipTimer = setTimeout(() => { t.hidden = true; tipTimer = null; }, 3200);
  });

  // ---------------- tabs ----------------
  function showTab(tab) {
    currentTab = tab;
    $$('.tab-btn').forEach(b => b.setAttribute('aria-selected', String(b.dataset.tab === tab)));
    $$('.tab').forEach(s => { s.hidden = s.id !== 'tab-' + tab; });
    $$('.menu-tabs .menu-item').forEach(b => b.setAttribute('aria-current', b.dataset.go === tab ? 'page' : 'false'));
    if (tab === 'profiles') renderProfiles();
    if (tab === 'sim') renderSimSetup();
  }

  // =========================================================
  // PLAY
  // =========================================================
  const G = { S: null, gen: null, waiting: null, timer: null, paused: false, resumeFn: null, aiRng: null, freshId: null, lastDraw: null, over: false, shuffledRound: 0, shuffleTimer: null };

  function hasHuman() { return G.S && G.S.players.some(p => p.controller === 'human'); }
  function pname(i) { return G.S.players[i].name; }

  function newGame() {
    if (G.timer) clearTimeout(G.timer);
    G.timer = null; G.paused = false; G.resumeFn = null;
    const players = settings.seats.map((s, i) => ({ name: (s.name || '').trim() || NAMES[i], controller: s.kind, profileId: s.profileId }));
    G.S = F7.createGame({ players, target: settings.target, house: Object.assign({}, settings.house) });
    G.gen = F7.playGame(G.S);
    G.aiRng = F7.makeRng();
    G.waiting = null; G.lastDraw = null; G.over = false; G.freshId = null; G.shuffledRound = 0;
    clearTimeout(G.shuffleTimer); $('#shuffleToast').hidden = true;
    $('#log').innerHTML = '';
    step();
  }

  function shufflePause() {
    const base = SPEEDS[settings.speed] || 0;
    return base ? Math.max(1500, base * 2.5) : 1200;
  }
  function eventDelay(v) {
    const base = SPEEDS[settings.speed] || 0;
    if (v.e === 'reshuffle') return shufflePause();
    if (!base) return 0;
    switch (v.e) {
      case 'draw': return base;
      case 'roundStart': return base * 0.8;
      case 'bust': case 'flip7': case 'freeze': case 'flip3': case 'secondUsed': return base * 1.1;
      case 'roundEnd': return base * 3.5;
      case 'stay': return base * 0.6;
      default: return base * 0.35;
    }
  }
  function decisionDelay(v) {
    const base = SPEEDS[settings.speed] || 0;
    return v.kind === 'hitStay' ? base * 0.55 : base * 0.8;
  }

  function scheduleNext(fn, ms) {
    G.resumeFn = fn;
    if (G.paused) { renderControl(); return; }
    G.timer = setTimeout(runResume, ms);
  }
  function runResume() {
    G.timer = null;
    const f = G.resumeFn; G.resumeFn = null;
    if (f) f();
  }
  function togglePause() {
    G.paused = !G.paused;
    if (G.paused && G.timer) { clearTimeout(G.timer); G.timer = null; }
    if (!G.paused && G.resumeFn && !G.timer) G.timer = setTimeout(runResume, 60);
    renderControl();
  }

  function step(ans) {
    const gen = G.gen;
    let r = gen.next(ans);
    while (true) {
      if (gen !== G.gen) return;
      if (r.done) { G.over = true; G.waiting = null; renderAll(); return; }
      const v = r.value;
      if (v.type === 'event') {
        handleEvent(v);
        if (v.e === 'roundEnd' && hasHuman()) { G.waiting = { kind: 'roundEnd' }; renderAll(); return; }
        const d = eventDelay(v);
        if (d > 0) { renderAll(); scheduleNext(() => step(), d); return; }
        r = gen.next();
        continue;
      }
      const p = G.S.players[v.p];
      if (p.controller === 'human') { G.waiting = v; renderAll(); focusControl(); return; }
      const res = F7.AI.decide(G.S, v, profileById(p.profileId), G.aiRng);
      if (settings.explain && res.reason) logLine(`${p.name}: ${res.reason}`, 'reason');
      const d = decisionDelay(v);
      if (d > 0) { G.waiting = Object.assign({ ai: true }, v); renderAll(); scheduleNext(() => { G.waiting = null; step(res.choice); }, d); return; }
      r = gen.next(res.choice);
    }
  }

  function answer(choice) {
    const w = G.waiting;
    if (!w || w.ai) return;
    G.waiting = null;
    if (w.kind === 'roundEnd') step(); else step(choice);
  }

  function focusControl() {
    const b = $('#control .autofocus');
    if (b && currentTab === 'play') { try { b.focus({ preventScroll: true }); } catch (e) { /* ignore */ } }
  }

  // ---------------- log ----------------
  let logSeq = 0;
  function logLine(text, cls) {
    const ol = $('#log');
    const li = document.createElement('li');
    li.dataset.seq = String(++logSeq);
    if (cls) li.className = cls;
    li.innerHTML = text;
    ol.insertBefore(li, ol.firstChild);
    while (ol.children.length > 400) ol.removeChild(ol.lastChild);
  }
  const lbl = c => F7.cardLabel(c.t);
  function handleEvent(v) {
    const S = G.S;
    switch (v.e) {
      case 'roundStart':
        G.lastDraw = null;
        logLine(`Round ${v.round}. ${esc(pname(v.dealer))} deals.`, 'round'); break;
      case 'draw':
        G.lastDraw = { p: v.p, card: v.card, ctx: v.ctx, k: v.k };
        G.freshId = v.card.id;
        if (v.ctx === 'deal') logLine(`${esc(pname(v.p))} is dealt ${esc(lbl(v.card))}.`);
        else if (v.ctx === 'flip3') logLine(`${esc(pname(v.p))} flips ${esc(lbl(v.card))} (${v.k} of 3).`);
        else logLine(`${esc(pname(v.p))} hits: ${esc(lbl(v.card))}.`);
        break;
      case 'bust': logLine(`${esc(pname(v.p))} busts on a second ${v.card.t}.`, 'bad'); break;
      case 'secondUsed': logLine(`${esc(pname(v.p))}'s Second Chance cancels the duplicate ${v.card.t}.`, 'good'); break;
      case 'secondKept': logLine(`${esc(pname(v.p))} keeps the Second Chance.`); break;
      case 'secondGiven': logLine(`${esc(pname(v.p))} already holds one and passes the Second Chance to ${esc(pname(v.target))}.`); break;
      case 'secondDiscarded': logLine(`Nobody can take the spare Second Chance, so it's discarded.`); break;
      case 'flip7': logLine(`${esc(pname(v.p))} flips 7 different numbers! The round ends (+15).`, 'good'); break;
      case 'stay': logLine(`${esc(pname(v.p))} stays with ${v.pts}.`); break;
      case 'freeze':
        logLine(v.p === v.target ? `${esc(pname(v.p))} freezes themselves at ${F7.roundScore(S.players[v.target])}.`
          : `${esc(pname(v.p))} freezes ${esc(pname(v.target))} at ${F7.roundScore(S.players[v.target])}.`); break;
      case 'flip3':
        logLine(v.p === v.target ? `${esc(pname(v.p))} plays Flip Three on themselves.` : `${esc(pname(v.p))} plays Flip Three on ${esc(pname(v.target))}.`); break;
      case 'queued': logLine(`${esc(lbl(v.card))} waits until the three cards are done.`); break;
      case 'actionDiscarded': {
        const why = { house: `${esc(pname(v.p))} discards the ${esc(lbl(v.card))} (house rule).`, busted: `${esc(lbl(v.card))} is discarded because ${esc(pname(v.p))} busted.`,
          roundOver: `${esc(lbl(v.card))} is discarded; the round is over.`, none: `No active players left, so ${esc(lbl(v.card))} is discarded.` }[v.why];
        logLine(why); break;
      }
      case 'reshuffle':
        G.shuffledRound = S.round;
        logLine(`Draw pile empty: shuffling ${v.n} discards into a new draw pile.`, 'round');
        showShuffle(v.n);
        break;
      case 'deckOut': logLine(`No cards left anywhere to draw. The round ends.`, 'bad'); break;
      case 'roundEnd': {
        const parts = S.players.map((p, i) => `${esc(p.name)} ${v.scores[i]}`).join(' · ');
        logLine(`Round ${v.round} scores: ${parts}`); break;
      }
      case 'tie': logLine(`Tied at ${v.score}: ${v.seats.map(i => esc(pname(i))).join(' and ')}. Everyone plays another round.`, 'round'); break;
      case 'gameEnd': logLine(`${esc(pname(v.winner))} wins with ${S.players[v.winner].total}.`, 'round'); break;
    }
  }

  // ---------------- rendering ----------------
  function cardHTML(c, extra) {
    const t = c.t;
    let cls = 'card', body, style = '', title;
    if (F7.isNum(t)) {
      const copies = Math.max(1, t);
      cls += ' n'; body = String(t); style = ` style="--h:${(copies / 12).toFixed(3)}"`;
      title = `${t}: ${copies} ${copies === 1 ? 'copy' : 'copies'} in the deck`;
    } else if (F7.isMod(t)) {
      cls += ' mod'; body = esc(F7.cardLabel(t)); title = t === F7.T_X2 ? 'Doubles your number total' : 'Adds to your round score';
    } else {
      cls += ' act a' + t; body = t === 19 ? 'FREEZE' : t === 20 ? 'FLIP 3' : '2ND CHANCE'; title = F7.cardLabel(t);
    }
    if (c.id === G.freshId) cls += ' fresh';
    if (G.lastDraw && c.id === G.lastDraw.card.id) cls += ' last';
    if (extra) cls += ' ' + extra;
    return `<span class="${cls}"${style} title="${esc(title)}">${body}</span>`;
  }

  function seatStatus(p, i) {
    const S = G.S;
    const deciding = G.waiting && G.waiting.p === i && G.waiting.kind !== 'roundEnd';
    if (p.flip7) return ['flip7', 'Flip 7'];
    if (p.status === 'busted') return ['busted', 'Busted'];
    if (p.status === 'stayed') return ['stayed', 'Stayed'];
    if (p.status === 'frozen') return ['frozen', 'Frozen'];
    if (S.phase === 'scoring') return ['stayed', 'Scored'];
    if (S.phase === 'between' || S.phase === 'over' || S.phase === 'idle') return ['active', 'Waiting'];
    return deciding ? ['deciding', 'Deciding'] : ['active', 'In play'];
  }

  function renderHead() {
    const S = G.S;
    const leader = S.players.reduce((a, p) => p.total > a.total ? p : a, S.players[0]);
    $('#tableHead').innerHTML =
      `<span class="rnd">Round ${Math.max(1, S.round)}</span>` +
      `<span>Dealer <b>${esc(pname(S.dealer))}</b></span>` +
      `<span>Opens <b>${esc(pname((S.dealer + 1) % S.players.length))}</b></span>` +
      `<span>Draw pile <b>${S.deck.length}</b></span>` +
      `<span>Discard <b>${S.discard.length}</b></span>` +
      `<span>Race to <b>${S.target}</b></span>` +
      (leader.total > 0 ? `<span>Leader <b>${esc(leader.name)} ${leader.total}</b></span>` : '') +
      (G.shuffledRound === S.round && S.round > 0 ? `<span class="fresh-shuffle" title="The discards were shuffled into a new draw pile this round">Reshuffled this round</span>` : '');
  }

  function renderSeats() {
    const S = G.S;
    const showOdds = !!settings.showOdds;
    const html = S.players.map((p, i) => {
      const [scls, slabel] = seatStatus(p, i);
      const human = p.controller === 'human';
      const prof = profileById(p.profileId);
      const turn = (G.waiting && G.waiting.p === i && G.waiting.kind !== 'roundEnd') || (S.turn === i && S.phase === 'turns');
      const isDealer = S.dealer === i && S.phase !== 'between';
      const live = F7.roundScore(p);
      const wouldBe = p.status === 'busted' ? F7.roundScore(Object.assign({}, p, { status: 'active' })) : live;
      const inRound = p.nums.length + p.mods.length + p.acts.length + (p.sc ? 1 : 0) > 0 || p.status !== 'active';
      let rp = '';
      if (p.status === 'busted') rp = `<span class="rp lost" title="Points lost to the bust">${wouldBe}</span>`;
      else if (inRound && S.phase !== 'between') rp = `<span class="rp pos" title="This round">+${live}</span>`;
      const w = Math.min(100, p.total / S.target * 100);
      const w2 = p.status === 'busted' || S.phase === 'between' ? 0 : Math.min(100 - w, live / S.target * 100);
      const nums = p.nums.map(c => cardHTML(c)).join('') + (p.bustCard ? cardHTML(p.bustCard, 'bustcard') : '');
      const extras = p.mods.map(c => cardHTML(c)).join('') + (p.sc ? cardHTML(p.sc) : '') +
        p.acts.map(c => cardHTML(c)).join('') + p.pending.map(c => cardHTML(c, 'queued')).join('');
      let odds = '';
      if (showOdds && p.status === 'active' && S.phase === 'turns') {
        const a = F7.AI.analyzeSeat(S, i, 1);
        const b = p.sc ? 0 : a.bustFull;
        odds = `<span class="odds${b >= 0.3 ? ' hot' : ''}" title="Chance the next card busts ${esc(p.name)}">${p.sc ? 'covered' : pct(b) + ' bust'}</span>`;
      }
      const row = nums + (nums && extras ? '<span class="sep" aria-hidden="true"></span>' : '') + extras;
      return `<article class="seat st-${scls}${turn ? ' turn' : ''}${human ? ' human' : ''}${p.status === 'busted' ? ' busted' : ''}">
        <header class="seat-h">
          <div class="who"><span class="nm" title="${esc(p.name)}">${esc(p.name)}</span><span class="prof" title="${human ? 'Human player' : esc(prof.name)}">${human ? 'Human player' : esc(prof.name)}</span></div>
          <div class="score" title="${p.total} of ${S.target} points"><span class="tot">${p.total}</span><span class="of">/${S.target}</span></div>
          <div class="badges"><span class="pill ${scls}">${slabel}</span>${isDealer ? '<span class="dealer-tag" title="Dealer this round">Dealer</span>' : ''}</div>
        </header>
        <div class="meter" aria-hidden="true"><i style="width:${w.toFixed(1)}%"></i><i class="add" style="width:${w2.toFixed(1)}%"></i></div>
        <div class="hand">
          <div class="cards">${row || '<span class="empty">No cards yet</span>'}</div>
          <div class="rpcol">${rp}${odds}</div>
        </div>
      </article>`;
    }).join('');
    $('#seats').innerHTML = html;
  }

  function lastLineHTML() {
    const d = G.lastDraw, S = G.S;
    if (!d) {
      const t = G.over || S.phase === 'between' || S.phase === 'scoring' ? `${S.deck.length} cards in the draw pile` : `Shuffled and ready: ${S.deck.length} cards in the draw pile`;
      return `<div class="lastline"><span class="ll-label">Draw pile</span><span>${t}</span></div>`;
    }
    const v = `<span class="ll-val">${esc(F7.cardLabel(d.card.t))}</span>`;
    const what = d.ctx === 'deal' ? `was dealt ${v}` : d.ctx === 'flip3' ? `flipped ${v} <span class="ll-note">(Flip Three, ${d.k} of 3)</span>` : `hit and drew ${v}`;
    return `<div class="lastline"><span class="ll-label">Last card</span><span><b>${esc(pname(d.p))}</b> ${what}</span></div>`;
  }

  function showShuffle(n) {
    if (currentTab !== 'play') return;
    const t = $('#shuffleToast');
    t.innerHTML = `<div class="sh-deck" aria-hidden="true"><i></i><i></i><i></i></div>
      <div class="sh-txt"><b>Shuffling the deck</b><span>${n} discards become the new draw pile. Card counts start over.</span></div>`;
    t.hidden = false;
    clearTimeout(G.shuffleTimer);
    G.shuffleTimer = setTimeout(() => { t.hidden = true; }, Math.max(2600, shufflePause() + 600));
  }

  function seatBrief(i) {
    const p = G.S.players[i];
    return `${p.total} total · ${F7.roundScore(p)} this round`;
  }

  function renderControl() {
    const S = G.S, w = G.waiting;
    let body = '';
    if (G.over) {
      const win = S.players[S.winner];
      const ranked = S.players.slice().sort((a, b) => b.total - a.total);
      body = `<div class="winner">${esc(win.name)} wins</div>
        <p class="muted">${S.round} rounds${S.capped ? ' (stopped at the round cap)' : ''}.</p>
        <table class="results"><tr><th>Player</th><th class="n">Final</th><th class="n">Busts</th><th class="n">Flip 7s</th></tr>
        ${ranked.map(p => `<tr><td>${esc(p.name)}</td><td class="n">${p.total}</td><td class="n">${p.stats.busts}</td><td class="n">${p.stats.flip7s}</td></tr>`).join('')}</table>
        <div class="ctl-row"><button class="btn primary autofocus" data-act="rematch" type="button">Play again</button><button class="btn" data-act="setup" type="button">Change setup</button></div>`;
    } else if (w && !w.ai && w.kind === 'roundEnd') {
      const h = S.history[S.history.length - 1];
      const max = Math.max.apply(null, S.players.map(p => p.total));
      const ending = max >= S.target && S.players.filter(p => p.total === max).length === 1;
      const label = s => ({ busted: 'Busted', stayed: 'Stayed', frozen: 'Frozen', active: 'Scored' }[s] || s);
      const ranked = S.players.map((p, i) => i).sort((a, b) => h.totals[b] - h.totals[a] || a - b);
      const top = h.totals[ranked[0]];
      body = `<p class="ctl-q">Round ${h.round} is over.<small>Standings, highest total first</small></p>
        <table class="results"><tr><th class="n">#</th><th>Player</th><th>Result</th><th class="n">Round</th><th class="n">Total</th><th class="n">Behind</th></tr>
        ${ranked.map(i => {
          const st = h.flip7 === i ? 'flip7' : h.statuses[i];
          const place = 1 + h.totals.filter(t => t > h.totals[i]).length;
          return `<tr${place === 1 ? ' class="lead"' : ''}><td class="n">${place}</td><td>${esc(S.players[i].name)}</td><td class="st-${st}">${st === 'flip7' ? 'Flip 7' : label(st)}</td><td class="n">${h.scores[i]}</td><td class="n">${h.totals[i]}</td><td class="n">${place === 1 ? '' : '−' + (top - h.totals[i])}</td></tr>`;
        }).join('')}</table>
        <div class="ctl-row"><button class="btn primary autofocus" data-act="continue" type="button">${ending ? 'See the result' : 'Deal round ' + (h.round + 1)}</button><span class="keys"><span class="kbd">Enter</span></span></div>`;
    } else if (w && !w.ai) {
      const me = S.players[w.p];
      if (w.kind === 'hitStay') {
        const a = F7.AI.analyzeSeat(S, w.p, 1);
        const mini = me.nums.map(c => cardHTML(c, 'mini')).join('') + me.mods.map(c => cardHTML(c, 'mini')).join('') +
          (me.sc ? cardHTML(me.sc, 'mini') : '') + me.acts.map(c => cardHTML(c, 'mini')).join('');
        const bust = settings.showOdds ? `<span class="${!me.sc && a.bustFull >= 0.3 ? 'hot' : ''}">${me.sc ? 'Second Chance covers a bust' : pct(a.bustFull) + ' bust'}</span>` : '';
        body = `<p class="ctl-q">${esc(me.name)}, hit or stay? <small class="inl">${me.total} banked</small></p>
          <div class="dock-hand" aria-label="Your hand"><div class="dh-cards">${mini || '<span class="empty">No cards yet</span>'}</div>
            <div class="dh-pts"><b>+${a.cur}</b>${bust}</div></div>
          <div class="hitstay"><button class="btn big hit autofocus" data-ans="hit" type="button">Hit</button><button class="btn big stay" data-ans="stay" type="button">Stay</button></div>
          <span class="keys"><span class="kbd">H</span> hit · <span class="kbd">S</span> stay</span>`;
      } else if (w.kind === 'target' || w.kind === 'giveSecond') {
        const q = w.kind === 'target'
          ? `${esc(me.name)}, you drew <b>${esc(F7.cardLabel(w.card.t))}</b>. Who gets it?`
          : `You already hold a Second Chance. Who gets this one?`;
        let rec = -1;
        if (settings.showAdvisor && w.kind === 'target') {
          const r = F7.AI.rankTargets(S, w.p, w.card, profileById('counter'));
          if (r.length) rec = r[0].seat;
        }
        body = `<p class="ctl-q">${q}</p><div class="choices">${w.options.map((j, k) =>
          `<button class="btn choice${j === rec ? ' rec' : ''}${k === 0 ? ' autofocus' : ''}" data-ans="${j}" type="button"><span>${j === w.p ? 'Yourself' : esc(pname(j))}</span><small>${seatBrief(j)}</small></button>`).join('')}</div>`;
      } else if (w.kind === 'lastDiscard') {
        const fr = w.card.t === F7.T_FREEZE;
        body = fr
          ? `<p class="ctl-q">You're the last active player and drew <b>Freeze</b>.<small>House rule: freeze yourself and bank ${F7.roundScore(me)}, or discard it and keep playing.</small></p>
          <div class="hitstay"><button class="btn big hit" data-ans="self" type="button">Freeze</button><button class="btn big stay autofocus" data-ans="discard" type="button">Discard</button></div>`
          : `<p class="ctl-q">You're the last active player and drew <b>Flip Three</b>.<small>House rule: take three cards yourself, or discard it.</small></p>
          <div class="hitstay"><button class="btn big hit autofocus" data-ans="self" type="button">Flip 3</button><button class="btn big stay" data-ans="discard" type="button">Discard</button></div>`;
      }
    } else {
      const turn = w && w.ai ? w.p : S.turn;
      const txt = S.phase === 'deal' ? 'Dealing…' : turn >= 0 ? `${esc(pname(turn))} is thinking…` : 'Playing…';
      body = `<div class="ctl-row"><span class="muted">${txt}</span><button class="btn small" data-act="pause" type="button">${G.paused ? 'Resume' : 'Pause'}</button><span class="keys"><span class="kbd">P</span></span></div>`;
    }
    $('#control').innerHTML = lastLineHTML() + body;
    $('#control').classList.toggle('tall', !!(G.over || (w && !w.ai && w.kind === 'roundEnd')));
  }

  function verdictRows(S, i) {
    return allProfiles().filter(p => p.model !== 'random').map(p => {
      const d = F7.AI.decideHitStay(S, i, p, Math.random);
      return `<tr><td>${esc(p.name)}</td><td><span class="call ${d.choice}">${d.choice.toUpperCase()}</span></td><td class="why">${esc(d.reason)}</td></tr>`;
    }).join('');
  }

  function renderAdvisor() {
    const el = $('#advisor');
    const S = G.S, w = G.waiting;
    if (!settings.showAdvisor || !w || w.ai || w.kind === 'roundEnd' || G.over) {
      el.innerHTML = settings.showAdvisor && hasHuman() && !G.over ? `<h3>Advisor</h3><p class="muted">Odds and profile calls show up here on your turn.</p>` : '';
      return;
    }
    if (w.kind === 'lastDiscard' && w.card.t === F7.T_FREEZE) {
      el.innerHTML = `<h3>Advisor</h3><p>Discard it. Staying on your next turn banks the same ${F7.roundScore(S.players[w.p])} points, so discarding only keeps your options open.</p>`;
      return;
    }
    if (w.kind === 'hitStay') {
      const a = F7.AI.analyzeSeat(S, w.p, 3);
      const best = Math.max(a.cur, a.look);
      const lookCall = a.look > a.cur + 1e-9 ? 'hit' : 'stay';
      const bust = a.sc ? 0 : a.bustFull;
      el.innerHTML = `<h3>Advisor</h3>
        <div class="adv-stats">
          <div class="stat"><span>Bust chance</span><b class="${bust >= 0.3 ? 'hot' : bust <= 0.12 ? 'ok' : ''}">${a.sc ? '0%' : pct(bust)}</b><small>${a.sc ? `Second Chance covers it (${pct(a.bustFull)} to use it)` : `Fresh-deck guess: ${pct(a.bustNaive)}`}</small></div>
          <div class="stat"><span>Stay now</span><b>${a.cur}</b><small>Points banked this round</small></div>
          <div class="stat"><span>Hit once</span><b>${a.oneCard.toFixed(1)}</b><small>Average if you take one card, then stop</small></div>
          <div class="stat"><span>Hit, then choose well</span><b class="${lookCall === 'hit' ? 'ok' : ''}">${best.toFixed(1)}</b><small>3-card lookahead says <b>${lookCall}</b></small></div>
          ${a.n === 6 ? `<div class="stat"><span>Flip 7 next card</span><b class="ok">${pct(a.flip7Next)}</b><small>Any number you don't hold</small></div>` : ''}
        </div>
        <table class="verdicts">${verdictRows(S, w.p)}</table>`;
    } else if (w.kind === 'target' || w.kind === 'lastDiscard') {
      const r = F7.AI.rankTargets(S, w.p, w.card, profileById('counter'));
      el.innerHTML = `<h3>Advisor: where ${esc(F7.cardLabel(w.card.t))} hurts most</h3>
        <ol class="ranked">${r.map(x => `<li><b>${x.self ? 'Yourself' : esc(pname(x.seat))}</b> <span class="v">(${x.value >= 0 ? '+' : ''}${x.value.toFixed(1)})</span><br><span class="muted">${esc(x.reason)}</span></li>`).join('')}</ol>
        <p class="muted" style="margin-top:8px">Scores are the Card Counter's rough estimate of points swung, weighted by how much of a threat each player is.</p>`;
    } else if (w.kind === 'giveSecond') {
      const opts = w.options.slice().sort((a, b) => (S.players[a].total + F7.roundScore(S.players[a])) - (S.players[b].total + F7.roundScore(S.players[b])));
      el.innerHTML = `<h3>Advisor</h3><p>Hand it to the smallest threat: <b>${esc(pname(opts[0]))}</b> (${seatBrief(opts[0])}).</p>`;
    }
  }

  function renderTracker() {
    const el = $('#tracker');
    const S = G.S;
    if (!settings.showCount) { el.innerHTML = ''; return; }
    const counts = new Array(F7.NTYPES).fill(0);
    for (const c of S.deck) counts[c.t]++;
    let focus = -1;
    if (G.waiting && !G.waiting.ai && G.waiting.p !== undefined) focus = G.waiting.p;
    else if (S.turn >= 0) focus = S.turn;
    const held = new Set(focus >= 0 && S.phase === 'turns' ? S.players[focus].nums.map(c => c.t) : []);
    let cols = '';
    for (let v = 0; v <= 12; v++) {
      const full = F7.FULL_COUNTS[v];
      const rem = counts[v];
      cols += `<div class="trk-col${held.has(v) ? ' risk' : ''}" data-tip="<b>${v}</b>: ${rem} of ${full} left in the draw pile${held.has(v) ? `<br>${esc(pname(focus))} holds a ${v}: drawing one busts` : ''}">
        <div class="bar"><i class="full" style="height:${(full / 12 * 100).toFixed(1)}%"></i><i class="rem" style="height:${(rem / 12 * 100).toFixed(1)}%"></i></div>
        <b>${v}</b><small>${rem}</small></div>`;
    }
    const chip = (t, name) => `<span class="chip${counts[t] === 0 ? ' gone' : ''}">${name}${F7.FULL_COUNTS[t] > 1 ? ' ' + counts[t] + '/' + F7.FULL_COUNTS[t] : ''}</span>`;
    el.innerHTML = `<h3>Card count</h3>
      <div class="trk-nums">${cols}</div>
      <div class="trk-chips">${chip(13, '+2')}${chip(14, '+4')}${chip(15, '+6')}${chip(16, '+8')}${chip(17, '+10')}${chip(18, '×2')}${chip(19, 'Freeze')}${chip(20, 'Flip 3')}${chip(21, '2nd Chance')}</div>
      ${G.shuffledRound === S.round && (S.phase === 'deal' || S.phase === 'turns') ? `<p class="trk-legend fresh-note">Reshuffled this round. The cards on the table now aren't in the draw pile; they go to the discard pile when the round ends.</p>` : ''}
      <p class="trk-legend">${S.deck.length} cards left to draw, ${S.discard.length} in the discard pile. The pale bar is each number's full count and the solid bar is what's left${held.size ? `, and red marks numbers ${esc(pname(focus))} already holds` : ''}.</p>`;
  }

  function renderSheet() {
    const S = G.S;
    if (!S.history.length) { $('#sheet').innerHTML = '<p class="muted">Scores appear after the first round.</p>'; return; }
    const head = `<tr><th>Round</th>${S.players.map(p => `<th>${esc(p.name)}</th>`).join('')}</tr>`;
    const rows = S.history.map(h => `<tr><td>${h.round}</td>${h.scores.map((s, i) => {
      const cls = h.statuses[i] === 'busted' ? 'bust' : h.flip7 === i ? 'f7' : '';
      return `<td class="${cls}" title="${h.statuses[i] === 'busted' ? 'Busted' : h.flip7 === i ? 'Flip 7' : ''}">${h.statuses[i] === 'busted' ? '0' : s} <small>${h.totals[i]}</small></td>`;
    }).join('')}</tr>`).join('');
    const tot = `<tr class="tot"><td>Total</td>${S.players.map(p => `<td>${p.total}</td>`).join('')}</tr>`;
    $('#sheet').innerHTML = `<table>${head}${rows}${tot}</table>`;
  }

  // ---------------- peek (cheat view) ----------------
  function handScore(nums, mods, flip7) { return F7.roundScore({ status: 'active', nums, mods, flip7 }); }

  function peekHit(p, card) {
    const t = card.t, L = F7.cardLabel(t);
    if (F7.isNum(t)) {
      if (p.nums.some(c => c.t === t)) {
        return p.sc ? { cls: 'save', text: `Hit and you get a second ${t}. Your Second Chance cancels it.` }
          : { cls: 'bust', text: `Hit and you get a second ${t}: bust, losing ${F7.roundScore(p)}.` };
      }
      const f7 = p.nums.length + 1 >= 7;
      const after = handScore(p.nums.concat([card]), p.mods, f7);
      return f7 ? { cls: 'flip7', text: `Hit and you get a ${t}: Flip 7, worth ${after} this round.` }
        : { cls: '', text: `Hit and you get a ${t}: safe, the round goes to ${after}.` };
    }
    if (F7.isMod(t)) return { cls: '', text: `Hit and you get ${L}: the round goes to ${handScore(p.nums, p.mods.concat([card]), false)}.` };
    if (t === F7.T_SECOND) return { cls: '', text: p.sc ? 'Hit and you get a spare Second Chance to hand to someone.' : 'Hit and you get a Second Chance.' };
    return { cls: '', text: `Hit and you draw ${L}, and pick who it hits.` };
  }

  // What a Flip Three would do to player q, given the known top cards
  function peekFlip3(q, cards) {
    const nums = q.nums.slice(), mods = q.mods.slice();
    let sc = !!q.sc;
    const before = F7.roundScore(q);
    const seen = [];
    for (let k = 0; k < 3; k++) {
      const c = cards[k];
      if (!c) return { cls: '', text: `${seen.length ? seen.join(', ') + ', then ' : ''}${3 - k} unknown card${3 - k > 1 ? 's' : ''} after the reshuffle (now at ${handScore(nums, mods, false)})` };
      const t = c.t;
      if (F7.isNum(t)) {
        if (nums.some(x => x.t === t)) {
          if (sc) { sc = false; seen.push(`a ${t} cancelled by Second Chance`); continue; }
          return { cls: 'bust', text: `busts on card ${k + 1} (a second ${t}), losing ${before}` };
        }
        nums.push(c);
        if (nums.length >= 7) return { cls: 'flip7', text: `hits Flip 7 on card ${k + 1}: ${handScore(nums, mods, true)} this round, and the round ends` };
        seen.push(String(t));
      } else if (F7.isMod(t)) { mods.push(c); seen.push(F7.cardLabel(t)); }
      else if (t === F7.T_SECOND) { sc = true; seen.push('Second Chance'); }
      else seen.push(`${F7.cardLabel(t)} (they aim it after)`);
    }
    return { cls: '', text: `survives with ${seen.join(', ')}: ${before} → ${handScore(nums, mods, false)}` };
  }

  function renderPeek() {
    const el = $('#peek');
    const S = G.S;
    if (!settings.peek || !S || G.over) { el.innerHTML = ''; return; }
    const top = [];
    for (let k = 0; k < 3 && k < S.deck.length; k++) top.push(S.deck[S.deck.length - 1 - k]);
    const names = ['Next', '2nd', '3rd'];
    let slots = '';
    for (let k = 0; k < 3; k++) {
      const c = top[k];
      slots += c ? `<div class="peek-slot" data-card="${esc(F7.cardLabel(c.t))}">${cardHTML(c)}<span>${names[k]}</span></div>`
        : `<div class="peek-slot" data-card="?"><span class="card back">?</span><span>${S.discard.length ? 'Reshuffle' : 'None left'}</span></div>`;
    }
    let note = '';
    const w = G.waiting;
    if (w && !w.ai && w.kind === 'hitStay') {
      const r = top[0] ? peekHit(S.players[w.p], top[0]) : { cls: '', text: 'Hit and you draw from a reshuffle that hasn’t happened yet.' };
      note = `<p class="peek-note ${r.cls}">${esc(r.text)}</p>`;
    } else if (w && !w.ai && (w.kind === 'target' || w.kind === 'lastDiscard') && w.card.t === F7.T_FLIP3) {
      const opts = w.kind === 'lastDiscard' ? [w.p] : w.options;
      note = `<ul class="peek-list">${opts.map(j => {
        const r = peekFlip3(S.players[j], top);
        return `<li class="${r.cls}"><b>${j === w.p ? 'You' : esc(pname(j))}</b> ${esc(r.text)}</li>`;
      }).join('')}</ul>`;
    } else if (w && !w.ai && (w.kind === 'target' || w.kind === 'lastDiscard')) {
      note = `<p class="peek-note muted">A Freeze stops its target before they can take any of these.</p>`;
    }
    el.innerHTML = `<h3>Next three cards <span class="cheat">PEEK</span></h3><div class="peek-row">${slots}</div>${note}`;
  }

  function renderAll() {
    if (!G.S) return;
    renderHead(); renderSeats(); renderControl(); renderPeek(); renderAdvisor(); renderTracker(); renderSheet();
    G.freshId = null;
  }

  // ---------------- setup panel ----------------
  function renderSetup() {
    $('#nPlayers').innerHTML = [3, 4, 5, 6, 7, 8].map(n => `<option value="${n}"${n === settings.seats.length ? ' selected' : ''}>${n}</option>`).join('');
    $('#target').value = settings.target;
    $('#speed').value = settings.speed;
    $('#showOdds').checked = !!settings.showOdds;
    $('#showCount').checked = !!settings.showCount;
    $('#showAdvisor').checked = !!settings.showAdvisor;
    $('#explain').checked = settings.explain;
    $('#peekToggle').checked = !!settings.peek;
    $('#rerollBetween').checked = !!settings.rerollBetween;
    $('#hrDiscard').checked = settings.house.lastMayDiscardFlip3;
    $('#hrDiscardFreeze').checked = !!settings.house.lastMayDiscardFreeze;
    $('#hrOpener').value = settings.house.opener;
    $('#seatCfg').innerHTML = settings.seats.map((s, i) => `<div class="seatrow">
      <span class="num">${i + 1}</span>
      <input type="text" id="seatName${i}" value="${esc(s.name)}" aria-label="Seat ${i + 1} name" maxlength="18">
      <select id="seatKind${i}" aria-label="Seat ${i + 1} player">
        <option value="human"${s.kind === 'human' ? ' selected' : ''}>Human player</option>
        ${seatProfileOptions(s)}
      </select></div>`).join('');
    updateSetupSummary();
  }
  function seatProfileOptions(s) {
    const opt = p => `<option value="ai:${p.id}"${s.kind === 'ai' && s.profileId === p.id ? ' selected' : ''}>${esc(p.name)}</option>`;
    const pre = F7.AI.PRESETS.filter(isAvailable), cus = customProfiles.filter(isAvailable);
    const cur = s.kind === 'ai' ? allProfiles().find(p => p.id === s.profileId) : null;
    return (pre.length ? `<optgroup label="AI: built-in">${pre.map(opt).join('')}</optgroup>` : '') +
      (cus.length ? `<optgroup label="AI: custom">${cus.map(opt).join('')}</optgroup>` : '') +
      (cur && !isAvailable(cur) ? `<optgroup label="Lab only (in use)">${opt(cur)}</optgroup>` : '');
  }
  function rerollPlayers() {
    const taken = new Set(settings.seats.filter(x => x.kind === 'human').map(x => low(x.name)));
    const named = new Set();
    settings.seats.forEach(x => {
      if (x.kind !== 'ai') return;
      x.profileId = pickProfile(taken, named);
      if (tableNameOf(x.profileId)) named.add(x.profileId);
      x.name = nameFor(x.profileId, taken);
      taken.add(low(x.name));
    });
  }
  function rerollSeatCount() {
    const humans = settings.seats.filter(s => s.kind === 'human').length;
    const n = Math.max(humans, 3 + Math.floor(Math.random() * 6));
    while (settings.seats.length < n) addAiSeat();
    for (let i = settings.seats.length - 1; i >= 0 && settings.seats.length > n; i--) {
      if (settings.seats[i].kind === 'ai') settings.seats.splice(i, 1);
    }
    return n;
  }
  function updateSetupSummary() {
    const humans = settings.seats.filter(s => s.kind === 'human').length;
    const house = [settings.house.lastMayDiscardFlip3 ? 'Flip Three discard' : '', settings.house.lastMayDiscardFreeze ? 'Freeze discard' : '',
      { winnerLeft: 'left of top scorer opens', winner: 'top scorer opens' }[settings.house.opener] || ''].filter(Boolean);
    $('#setupSum').textContent = `${settings.seats.length} players · ${humans ? humans + ' human' : 'all AI'} · to ${settings.target}${house.length ? ' · house rules: ' + house.join(', ') : ''}${settings.rerollBetween ? ' · re-roll on Play again' : ''}${settings.peek ? ' · peek on' : ''}`;
  }
  function bindSetup() {
    $('#nPlayers').addEventListener('change', e => {
      const n = +e.target.value;
      while (settings.seats.length < n) addAiSeat();
      settings.seats.length = n;
      renderSetup(); persist();
    });
    $('#target').addEventListener('change', e => { settings.target = Math.max(50, Math.min(1000, Math.round(+e.target.value || 200))); e.target.value = settings.target; updateSetupSummary(); persist(); });
    $('#speed').addEventListener('change', e => { settings.speed = e.target.value; persist(); });
    [['#showOdds', 'showOdds'], ['#showCount', 'showCount'], ['#showAdvisor', 'showAdvisor']].forEach(([id, k]) =>
      $(id).addEventListener('change', e => { settings[k] = e.target.checked; persist(); if (G.S) renderAll(); }));
    $('#rerollBetween').addEventListener('change', e => { settings.rerollBetween = e.target.checked; updateSetupSummary(); persist(); });
    $('#rndCount').addEventListener('click', () => { rerollSeatCount(); renderSetup(); persist(); });
    $('#rndPlayers').addEventListener('click', () => { rerollPlayers(); renderSetup(); persist(); });
    $('#hrDiscardFreeze').addEventListener('change', e => { settings.house.lastMayDiscardFreeze = e.target.checked; updateSetupSummary(); persist(); });
    $('#explain').addEventListener('change', e => { settings.explain = e.target.checked; persist(); });
    $('#peekToggle').addEventListener('change', e => { settings.peek = e.target.checked; updateSetupSummary(); persist(); if (G.S) renderPeek(); });
    $('#hrDiscard').addEventListener('change', e => { settings.house.lastMayDiscardFlip3 = e.target.checked; updateSetupSummary(); persist(); });
    $('#hrOpener').addEventListener('change', e => { settings.house.opener = e.target.value; updateSetupSummary(); persist(); });
    $('#seatCfg').addEventListener('input', e => {
      const m = e.target.id.match(/^seatName(\d)$/);
      if (m) { settings.seats[+m[1]].name = e.target.value; persist(); }
    });
    $('#seatCfg').addEventListener('change', e => {
      const m = e.target.id.match(/^seatKind(\d)$/);
      if (!m) return;
      const idx = +m[1], s = settings.seats[idx];
      const oldTn = s.kind === 'ai' ? tableNameOf(s.profileId) : '';
      if (e.target.value === 'human') s.kind = 'human';
      else {
        s.kind = 'ai'; s.profileId = e.target.value.slice(3);
        const taken = namesTaken(idx);
        // Seats take a profile's table name; a seat that carried the old profile's name gets a fresh one
        if (tableNameOf(s.profileId)) s.name = nameFor(s.profileId, taken);
        else if (oldTn && low(s.name).startsWith(low(oldTn))) s.name = poolName(taken);
      }
      renderSetup(); persist();
    });
    $('#btnStart').addEventListener('click', () => { $('#setupBox').open = false; newGame(); });
  }

  // control clicks and keys
  function bindPlay() {
    $('#control').addEventListener('click', e => {
      const b = e.target.closest('button');
      if (!b) return;
      if (b.dataset.ans !== undefined) {
        const v = b.dataset.ans;
        answer(/^\d+$/.test(v) ? +v : v);
      } else if (b.dataset.act === 'continue') answer();
      else if (b.dataset.act === 'pause') { if (e.detail === 0) togglePause(); }
      else if (b.dataset.act === 'rematch') {
        if (settings.rerollBetween) { rerollSeatCount(); rerollPlayers(); renderSetup(); persist(); }
        newGame();
        if (settings.rerollBetween) logLine(`New table: ${G.S.players.map(p => esc(p.name) + (p.controller === 'human' ? '' : ' (' + esc(profileById(p.profileId).name) + ')')).join(', ')}.`, 'round');
      }
      else if (b.dataset.act === 'setup') openSetup();
    });
    $('#control').addEventListener('pointerdown', e => {
      const b = e.target.closest('button[data-act="pause"]');
      if (b) { e.preventDefault(); togglePause(); }
    });
    document.addEventListener('keydown', e => {
      if (currentTab !== 'play' || e.metaKey || e.ctrlKey || e.altKey) return;
      const tag = (e.target.tagName || '').toLowerCase();
      if (tag === 'input' || tag === 'select' || tag === 'textarea') return;
      if ((e.key === 'p' || e.key === 'P') && !G.over && (!G.waiting || G.waiting.ai)) { e.preventDefault(); togglePause(); return; }
      const w = G.waiting;
      if (!w || w.ai) return;
      if (w.kind === 'hitStay') {
        if (e.key === 'h' || e.key === 'H') { e.preventDefault(); answer('hit'); }
        else if (e.key === 's' || e.key === 'S') { e.preventDefault(); answer('stay'); }
      } else if (w.kind === 'roundEnd' && e.key === 'Enter' && tag !== 'button') { e.preventDefault(); answer(); }
    });
  }

  // =========================================================
  // STRESS TEST
  // =========================================================
  const SWEEP = {
    bankAt: { label: 'Bank at (points, 0 = off)', name: 'bank at', short: 'Bank', models: ['threshold'], pct: false, def: [15, 40, 5], min: 0, max: 300 },
    bankWith: { label: 'Bank with (numbers held, 0 = off)', name: 'bank with', short: 'Bank with', models: ['threshold'], pct: false, def: [3, 6, 1], min: 0, max: 6, int: true },
    bankWithFloor: { label: 'Low-hand floor (share of Bank at)', name: 'low-hand floor', short: 'Floor', models: ['threshold'], pct: true, def: [0, 1, 0.25], min: 0, max: 1 },
    riskCap: { label: 'Risk cap (max bust chance)', name: 'risk cap', short: 'Risk', models: ['threshold'], pct: true, def: [0.1, 0.5, 0.05], min: 0, max: 1 },
    countBandwidth: { label: 'Memory bandwidth (limited counting)', name: 'memory bandwidth', short: 'Memory', models: ['threshold', 'ev'], pct: false, def: [1, 12, 1], min: 1, max: 12, int: true },
    evMargin: { label: 'EV margin', name: 'EV margin', short: 'Margin', models: ['ev'], pct: true, def: [-0.2, 0.4, 0.1], min: -1, max: 2 },
    evDepth: { label: 'Lookahead depth (cards)', name: 'lookahead', short: 'Depth', models: ['ev'], pct: false, def: [1, 3, 1], min: 1, max: 3, int: true },
    chase7At: { label: 'Flip 7 chase starts at (numbers held)', name: 'Flip 7 chase at', short: 'Chase at', models: ['threshold', 'ev'], pct: false, def: [3, 6, 1], min: 0, max: 6, int: true },
    chaseBoost: { label: 'Flip 7 chase boost', name: 'chase boost', short: 'Boost', models: ['threshold', 'ev'], pct: true, def: [0, 0.5, 0.1], min: 0, max: 1 },
    comeback: { label: 'Comeback aggression', name: 'comeback', short: 'Comeback', models: ['threshold', 'ev'], pct: true, def: [0, 1, 0.25], min: 0, max: 1 },
    stayStart: { label: 'Stay chance with one number', name: 'starting stay chance', short: 'Stay', models: ['random'], pct: true, def: [0, 0.3, 0.05], min: 0, max: 1 },
    stayStep: { label: 'Stay chance added per number', name: 'stay chance step', short: 'Step', models: ['random'], pct: true, def: [0.05, 0.25, 0.05], min: 0, max: 1 }
  };
  const fmtParam = (key, v) => SWEEP[key].pct ? (key === 'evMargin' && v > 0 ? '+' : '') + Math.round(v * 100) + '%'
    : ((key === 'bankAt' || key === 'chase7At' || key === 'bankWith') && v === 0 ? 'off' : String(+v.toFixed(2)));

  // Set one setting on a profile copy, switching on anything it depends on
  function applyParam(prof, k, v) {
    prof[k] = v;
    if (k === 'chaseBoost' && !prof.chase7At) prof.chase7At = 5;
    if (k === 'countBandwidth') prof.counting = 'limited';
    if (k === 'bankWithFloor' && !prof.bankWith) prof.bankWith = 4;
  }
  function paramIssue(prof, k) {
    const P = SWEEP[k];
    if (!P.models.includes(prof.model)) return { block: true, msg: `${prof.name} uses the ${MODEL_LABEL[prof.model]} model, so ${P.name} has no effect on it. Pick a different profile or setting.` };
    if (k === 'bankWithFloor' && !(prof.bankAt > 0)) return { block: true, msg: `${prof.name} has Bank at off, so the low-hand floor has nothing to scale. Give it a Bank at first, or vary Bank with instead.` };
    if (k === 'bankWithFloor' && !prof.bankWith) return { block: false, msg: `${prof.name} has Bank with off, so it's switched on at 4 numbers for these runs.` };
    if (k === 'chaseBoost' && !prof.chase7At) return { block: false, msg: `${prof.name} has Flip 7 chasing off, so it's switched on at 5 numbers for these runs.` };
    if (k === 'countBandwidth' && prof.counting !== 'limited') return { block: false, msg: `${prof.name} doesn't use limited memory, so its card counting is switched to Limited memory for these runs.` };
    return null;
  }
  function rangeValues(k, from, to, step) {
    const P = SWEEP[k], out = [];
    if (!(step > 0) || to < from) return out;
    for (let i = 0; i < 400; i++) {
      let v = from + i * step;
      if (v > to + 1e-9) break;
      v = P.int ? Math.round(v) : +v.toFixed(4);
      v = Math.max(P.min, Math.min(P.max, v));
      if (!out.includes(v)) out.push(v);
    }
    return out;
  }
  function effectiveProfile(e) {
    const base = profileById(e.profileId);
    const prof = Object.assign({}, base);
    const tags = [];
    if (e.overrides) for (const k of Object.keys(e.overrides)) {
      if (!SWEEP[k]) continue;
      applyParam(prof, k, e.overrides[k]);
      tags.push(`${SWEEP[k].name} ${fmtParam(k, e.overrides[k])}`);
    }
    if (tags.length) prof.name = `${base.name} (${tags.join(', ')})`;
    return prof;
  }

  let simJob = null, sweepJob = null;

  function renderSimSetup() {
    const rows = simSettings.entrants.map((e, i) => `<div class="simrow">
      <input type="text" id="simName${i}" value="${esc(e.name)}" aria-label="Seat ${i + 1} label" maxlength="22">
      <select id="simProf${i}" aria-label="Seat ${i + 1} profile">${profileOptionsHTML(e.profileId)}</select>
      <button type="button" data-rm="${i}" aria-label="Remove seat ${i + 1}" ${simSettings.entrants.length <= 3 ? 'disabled' : ''}>✕</button>
      ${e.overrides && Object.keys(e.overrides).length ? `<div class="ovr"><span>${Object.keys(e.overrides).filter(k => SWEEP[k]).map(k => `${esc(SWEEP[k].name)} ${fmtParam(k, e.overrides[k])}`).join(', ')}</span><button type="button" data-clr="${i}">clear</button></div>` : ''}</div>`).join('');
    $('#simSeats').innerHTML = rows;
    $('#simAdd').disabled = simSettings.entrants.length >= 8;
    $('#simGames').value = simSettings.games;
    $('#simTarget').value = simSettings.target;
    $('#simSeed').value = simSettings.seed;
    $('#simRotate').checked = simSettings.rotate;
    $('#simHrDiscard').checked = simSettings.house.lastMayDiscardFlip3;
    $('#simHrDiscardFreeze').checked = !!simSettings.house.lastMayDiscardFreeze;
    $('#simHrOpener').value = simSettings.house.opener;
    renderSweepSetup();
    renderLadderSetup();
  }

  function renderSweepSetup() {
    const sw = simSettings.sweep;
    if (sw.seat >= simSettings.entrants.length) sw.seat = 0;
    $('#swSeat').innerHTML = simSettings.entrants.map((e, i) => `<option value="${i}"${i === sw.seat ? ' selected' : ''}>Seat ${i + 1}: ${esc(e.name)}</option>`).join('');
    $('#swParam').innerHTML = Object.keys(SWEEP).map(k => `<option value="${k}"${k === sw.param ? ' selected' : ''}>${SWEEP[k].label}</option>`).join('');
    const P = SWEEP[sw.param];
    const show = v => P.pct ? +(v * 100).toFixed(2) : v;
    $('#swFrom').value = show(sw.from); $('#swTo').value = show(sw.to); $('#swStep').value = show(sw.step);
    $('#swUnit1').textContent = $('#swUnit2').textContent = P.pct ? '(%)' : '';
    $('#swGames').value = sw.games;
    sweepWarning();
  }

  function sweepWarning() {
    const sw = simSettings.sweep;
    const prof = effectiveProfile(simSettings.entrants[sw.seat]);
    const el = $('#swWarn');
    const iss = paramIssue(prof, sw.param);
    let msg = iss ? iss.msg : '', block = !!(iss && iss.block);
    const vals = sweepValues();
    if (!block && vals.length > 25) { msg = `That range makes ${vals.length} values; the sweep is capped at 25. Widen the step.`; block = true; }
    if (!block && vals.length < 2) { msg = 'The range needs at least two values.'; block = true; }
    el.textContent = msg; el.hidden = !msg;
    return { msg, block };
  }

  // ---------------- ladder ----------------
  function renderLadderSetup() {
    const ld = simSettings.ladder;
    if (!allProfiles().some(p => p.id === ld.base)) ld.base = 'balanced';
    $('#ldBase').innerHTML = profileOptionsHTML(ld.base);
    $('#ldParam').innerHTML = Object.keys(SWEEP).map(k => `<option value="${k}"${k === ld.param ? ' selected' : ''}>${SWEEP[k].label}</option>`).join('');
    const P = SWEEP[ld.param];
    const show = v => P.pct ? +(v * 100).toFixed(2) : v;
    $('#ldFrom').value = show(ld.from); $('#ldTo').value = show(ld.to); $('#ldStep').value = show(ld.step);
    $('#ldUnit1').textContent = $('#ldUnit2').textContent = P.pct ? '(%)' : '';
    ladderWarning();
  }
  function ladderValues() { const ld = simSettings.ladder; return rangeValues(ld.param, ld.from, ld.to, ld.step); }
  function ladderWarning() {
    const ld = simSettings.ladder;
    const prof = profileById(ld.base);
    const vals = ladderValues();
    const iss = paramIssue(prof, ld.param);
    let msg = iss ? iss.msg : '', block = !!(iss && iss.block);
    if (!block && vals.length > 8) { msg = `That makes ${vals.length} seats; the table holds 8. Narrow the range or widen the step.`; block = true; }
    if (!block && vals.length < 3) { msg = `That makes ${vals.length} seat${vals.length === 1 ? '' : 's'}; a game needs at least 3.`; block = true; }
    $('#ldWarn').textContent = msg; $('#ldWarn').hidden = !msg;
    $('#ldFill').disabled = block;
    $('#ldCount').textContent = block ? '' : `${vals.length} seats: ${vals.map(v => fmtParam(ld.param, v)).join(', ')}. Replaces the seats above.`;
    return { msg, block };
  }
  function fillLadder() {
    if (ladderWarning().block) return;
    const ld = simSettings.ladder;
    simSettings.entrants = ladderValues().map(v => ({
      name: `${SWEEP[ld.param].short} ${fmtParam(ld.param, v)}`, profileId: ld.base, overrides: { [ld.param]: v }
    }));
    simSettings.sweep.seat = 0;
    renderSimSetup(); persist();
  }

  function sweepValues() { const sw = simSettings.sweep; return rangeValues(sw.param, sw.from, sw.to, sw.step); }

  function bindSim() {
    $('#simSeats').addEventListener('input', e => {
      const m = e.target.id.match(/^simName(\d)$/);
      if (m) { simSettings.entrants[+m[1]].name = e.target.value; persist(); }
    });
    $('#simSeats').addEventListener('change', e => {
      const m = e.target.id.match(/^simProf(\d)$/);
      if (m) {
        const ent = simSettings.entrants[+m[1]];
        const old = profileById(ent.profileId).name;
        const hadOverrides = !!ent.overrides;
        ent.profileId = e.target.value;
        delete ent.overrides;
        if (!ent.name || ent.name === old || hadOverrides) ent.name = profileById(ent.profileId).name;
        renderSimSetup(); persist();
      }
    });
    $('#simSeats').addEventListener('click', e => {
      const c = e.target.closest('button[data-clr]');
      if (c) {
        const ent = simSettings.entrants[+c.dataset.clr];
        delete ent.overrides; ent.name = profileById(ent.profileId).name;
        renderSimSetup(); persist(); return;
      }
      const b = e.target.closest('button[data-rm]');
      if (!b || simSettings.entrants.length <= 3) return;
      simSettings.entrants.splice(+b.dataset.rm, 1);
      renderSimSetup(); persist();
    });
    $('#simAdd').addEventListener('click', () => {
      if (simSettings.entrants.length >= 8) return;
      const id = DEFAULT_PROFILE_CYCLE[simSettings.entrants.length % DEFAULT_PROFILE_CYCLE.length];
      simSettings.entrants.push({ name: profileById(id).name, profileId: id });
      renderSimSetup(); persist();
    });
    $('#simGames').addEventListener('change', e => { simSettings.games = Math.max(10, Math.min(100000, Math.round(+e.target.value || 1000))); e.target.value = simSettings.games; persist(); });
    $('#simTarget').addEventListener('change', e => { simSettings.target = Math.max(50, Math.min(1000, Math.round(+e.target.value || 200))); e.target.value = simSettings.target; persist(); });
    $('#simSeed').addEventListener('change', e => { simSettings.seed = e.target.value.trim(); persist(); });
    $('#simRotate').addEventListener('change', e => { simSettings.rotate = e.target.checked; persist(); });
    $('#simHrDiscard').addEventListener('change', e => { simSettings.house.lastMayDiscardFlip3 = e.target.checked; persist(); });
    $('#simHrDiscardFreeze').addEventListener('change', e => { simSettings.house.lastMayDiscardFreeze = e.target.checked; persist(); });
    $('#simHrOpener').addEventListener('change', e => { simSettings.house.opener = e.target.value; persist(); });
    $('#simRun').addEventListener('click', runSim);
    $('#simStop').addEventListener('click', () => { if (simJob) simJob.stopped = true; });

    $('#swSeat').addEventListener('change', e => { simSettings.sweep.seat = +e.target.value; sweepWarning(); persist(); });
    $('#swParam').addEventListener('change', e => {
      const sw = simSettings.sweep; sw.param = e.target.value;
      const d = SWEEP[sw.param].def; sw.from = d[0]; sw.to = d[1]; sw.step = d[2];
      renderSweepSetup(); persist();
    });
    const rd = id => { const P = SWEEP[simSettings.sweep.param]; const v = +$(id).value; return P.pct ? v / 100 : v; };
    ['#swFrom', '#swTo', '#swStep'].forEach(id => $(id).addEventListener('change', () => {
      const sw = simSettings.sweep; sw.from = rd('#swFrom'); sw.to = rd('#swTo'); sw.step = rd('#swStep'); sweepWarning(); persist();
    }));
    $('#swGames').addEventListener('change', e => { simSettings.sweep.games = Math.max(50, Math.min(20000, Math.round(+e.target.value || 500))); e.target.value = simSettings.sweep.games; persist(); });
    $('#swRun').addEventListener('click', runSweep);
    $('#ldBase').addEventListener('change', e => { simSettings.ladder.base = e.target.value; ladderWarning(); persist(); });
    $('#ldParam').addEventListener('change', e => {
      const ld = simSettings.ladder; ld.param = e.target.value;
      const d = SWEEP[ld.param].def; ld.from = d[0]; ld.to = d[1]; ld.step = d[2];
      renderLadderSetup(); persist();
    });
    const rdl = id => { const P = SWEEP[simSettings.ladder.param]; const v = +$(id).value; return P.pct ? v / 100 : v; };
    ['#ldFrom', '#ldTo', '#ldStep'].forEach(id => $(id).addEventListener('change', () => {
      const ld = simSettings.ladder; ld.from = rdl('#ldFrom'); ld.to = rdl('#ldTo'); ld.step = rdl('#ldStep'); ladderWarning(); persist();
    }));
    $('#ldFill').addEventListener('click', fillLadder);
    $('#swStop').addEventListener('click', () => { if (sweepJob) sweepJob.stopped = true; });
  }

  function simOpts() {
    return { target: simSettings.target, seed: simSettings.seed, rotate: simSettings.rotate, house: Object.assign({}, simSettings.house) };
  }
  function entrantsFor() {
    return simSettings.entrants.map(e => ({ name: e.name || profileById(e.profileId).name, profile: effectiveProfile(e) }));
  }
  function setProg(sel, frac, text) {
    $(sel + ' i').style.width = (frac * 100).toFixed(1) + '%';
    $(sel + ' span').textContent = text;
  }

  function runSim() {
    if (simJob || sweepJob) return;
    const ents = entrantsFor();
    const n = simSettings.games;
    const batch = F7.Sim.createBatch(ents, simOpts(), n);
    simJob = { batch, stopped: false, t0: Date.now(), lastRender: 0 };
    $('#simRun').disabled = true; $('#simStop').disabled = false; $('#swRun').disabled = true;
    const tick = () => {
      const job = simJob;
      const done = job.batch.step(45);
      setProg('#simProg', job.batch.done / n, `${job.batch.done.toLocaleString()} of ${n.toLocaleString()} games`);
      const now = Date.now();
      if (done || job.stopped || now - job.lastRender > 400) { renderSimResults(job.batch.summary(), !(done || job.stopped)); job.lastRender = now; }
      if (done || job.stopped) {
        setProg('#simProg', job.batch.done / n, `${job.batch.done.toLocaleString()} games in ${((now - job.t0) / 1000).toFixed(1)} s${job.stopped && !done ? ' (stopped)' : ''}`);
        simJob = null; $('#simRun').disabled = false; $('#simStop').disabled = true; $('#swRun').disabled = false;
        return;
      }
      setTimeout(tick, 0);
    };
    tick();
  }

  function winChart(rows, parity) {
    const W = 640, rowH = 34, top = 26, left = 150, right = 70;
    const H = top + rows.length * rowH + 30;
    const maxV = Math.max(parity, ...rows.map(r => r.hi));
    const xMax = Math.min(1, Math.ceil((maxV + 0.02) * 10) / 10);
    const X = v => left + (W - left - right) * (v / xMax);
    let g = '';
    for (let t = 0; t <= xMax + 1e-9; t += 0.1) {
      g += `<line class="grid" x1="${X(t)}" x2="${X(t)}" y1="${top - 6}" y2="${H - 24}"/><text x="${X(t)}" y="${H - 8}" text-anchor="middle">${Math.round(t * 100)}%</text>`;
    }
    const bars = rows.map((r, i) => {
      const y = top + i * rowH;
      const bw = Math.max(2, X(r.winRate) - left);
      const tipTxt = `<b>${esc(r.name)}</b><br>Wins ${pct(r.winRate, 1)} (95% range ${pct(r.lo, 1)} to ${pct(r.hi, 1)})<br>${r.wins.toLocaleString()} wins`;
      return `<g data-tip="${esc(tipTxt)}">
        <rect class="hit" x="0" y="${y}" width="${W}" height="${rowH}"/>
        <text class="lbl" x="${left - 10}" y="${y + rowH / 2 + 5}" text-anchor="end">${esc(r.name.length > 18 ? r.name.slice(0, 17) + '…' : r.name)}</text>
        <path class="bar" d="M${left},${y + 8} h${bw - 4} a4,4 0 0 1 4,4 v${rowH - 24} a4,4 0 0 1 -4,4 h${-(bw - 4)} z"/>
        <line class="ci" x1="${X(r.lo)}" x2="${X(r.hi)}" y1="${y + rowH / 2}" y2="${y + rowH / 2}"/>
        <line class="ci" x1="${X(r.lo)}" x2="${X(r.lo)}" y1="${y + rowH / 2 - 5}" y2="${y + rowH / 2 + 5}"/>
        <line class="ci" x1="${X(r.hi)}" x2="${X(r.hi)}" y1="${y + rowH / 2 - 5}" y2="${y + rowH / 2 + 5}"/>
        <text class="val" x="${X(r.hi) + 8}" y="${y + rowH / 2 + 5}">${pct(r.winRate, 1)}</text></g>`;
    }).join('');
    const par = `<line class="parity" x1="${X(parity)}" x2="${X(parity)}" y1="${top - 10}" y2="${H - 24}"/><text x="${X(parity) + 5}" y="${top - 12}">Even share ${pct(parity, 1)}</text>`;
    return `<svg class="chart" viewBox="0 0 ${W} ${H}" style="max-width:${W}px" role="img" aria-label="Win rate by seat with 95% intervals">${g}${par}${bars}</svg>`;
  }

  function renderSimResults(sum, running) {
    const n = sum.rows.length, parity = 1 / n;
    const rows = sum.rows.slice().sort((a, b) => b.winRate - a.winRate);
    const verdict = r => r.lo > parity ? '<span class="verdict up">Above even</span>' : r.hi < parity ? '<span class="verdict down">Below even</span>' : '<span class="verdict">Within noise</span>';
    const halfAtParity = 1.96 * Math.sqrt(parity * (1 - parity) / Math.max(1, sum.games));
    const table = `<div class="tblwrap"><table class="stbl">
      <tr><th>Seat</th><th>Profile</th><th>Win rate</th><th>95% range</th><th>vs even</th><th>Avg final</th><th>Points / round</th><th>Bust rate</th><th>Flip 7 rate</th><th>Frozen / round</th><th>Flip 3 taken / round</th></tr>
      ${rows.map(r => `<tr><td><b>${esc(r.name)}</b></td><td>${esc(r.profileName)}</td><td>${pct(r.winRate, 1)}</td><td><small>${pct(r.lo, 1)} to ${pct(r.hi, 1)}</small></td><td>${verdict(r)}</td>
        <td>${r.avgFinal.toFixed(0)}</td><td>${r.ptsPerRound.toFixed(1)}</td><td>${pct(r.bustRate)}</td><td>${pct(r.flip7Rate, 1)}</td><td>${r.frozenRate.toFixed(2)}</td><td>${r.flip3Rate.toFixed(2)}</td></tr>`).join('')}
    </table></div>`;
    $('#simResults').innerHTML = winChart(rows, parity) + table +
      `<div class="notes" style="margin-top:10px">
        <span>${sum.games.toLocaleString()} games${running ? ' so far' : ''}, ${sum.avgRounds.toFixed(1)} rounds per game on average.${sum.capped ? ` ${sum.capped} hit the round cap.` : ''}</span>
        <span>At this sample size, luck alone moves an even-share seat by about ±${(halfAtParity * 100).toFixed(1)} points. "Above even" and "Below even" mean the whole 95% range sits on one side of ${pct(parity, 1)}.</span>
        <span>Points per round counts busted rounds as zero. A high-variance profile can win more often with fewer points per round, because the race rewards big swings near the finish.</span>
      </div>`;
  }

  function runSweep() {
    if (simJob || sweepJob) return;
    if (sweepWarning().block) return;
    const sw = simSettings.sweep;
    const values = sweepValues();
    const base = entrantsFor();
    const per = sw.games;
    sweepJob = { stopped: false, values, idx: 0, batch: null, results: [], t0: Date.now(), param: sw.param, seat: sw.seat, n: base.length };
    $('#swRun').disabled = true; $('#swStop').disabled = false; $('#simRun').disabled = true;
    const mkBatch = v => {
      const ents = base.map(e => ({ name: e.name, profile: Object.assign({}, e.profile) }));
      const tp = ents[sw.seat].profile;
      applyParam(tp, sw.param, v);
      const o = simOpts();
      if (o.seed) o.seed = o.seed + ':' + sw.param;
      return F7.Sim.createBatch(ents, o, per);
    };
    const tick = () => {
      const job = sweepJob;
      if (!job.batch) job.batch = mkBatch(job.values[job.idx]);
      const done = job.batch.step(45);
      const completed = job.idx * per + job.batch.done;
      setProg('#swProg', completed / (job.values.length * per), `Value ${job.idx + 1} of ${job.values.length} · ${completed.toLocaleString()} games`);
      if (done) {
        const s = job.batch.summary();
        const r = s.rows[job.seat];
        job.results.push({ value: job.values[job.idx], winRate: r.winRate, lo: r.lo, hi: r.hi, ptsPerRound: r.ptsPerRound, bustRate: r.bustRate, flip7Rate: r.flip7Rate });
        job.idx++; job.batch = null;
        renderSweepResults(job);
      }
      if (job.stopped || job.idx >= job.values.length) {
        setProg('#swProg', 1, `${job.results.length} values in ${((Date.now() - job.t0) / 1000).toFixed(1)} s${job.stopped && job.idx < job.values.length ? ' (stopped)' : ''}`);
        sweepJob = null; $('#swRun').disabled = false; $('#swStop').disabled = true; $('#simRun').disabled = false;
        return;
      }
      setTimeout(tick, 0);
    };
    $('#swResults').innerHTML = '';
    tick();
  }

  function sweepChart(job) {
    const res = job.results, parity = 1 / job.n;
    const W = 760, H = 300, L = 52, R = 20, T = 26, B = 44;
    const m = Math.max(job.values.length - 1, 1);
    const X = k => L + (W - L - R) * (k / m);
    const yMax = Math.min(1, Math.ceil((Math.max(parity, ...res.map(r => r.hi)) + 0.03) * 20) / 20);
    const Y = v => T + (H - T - B) * (1 - v / yMax);
    let g = '';
    const stepY = yMax > 0.5 ? 0.1 : 0.05;
    for (let t = 0; t <= yMax + 1e-9; t += stepY) g += `<line class="grid" x1="${L}" x2="${W - R}" y1="${Y(t)}" y2="${Y(t)}"/><text x="${L - 8}" y="${Y(t) + 4}" text-anchor="end">${Math.round(t * 100)}%</text>`;
    const every = Math.ceil(job.values.length / 10);
    job.values.forEach((v, k) => { if (k % every === 0 || k === job.values.length - 1) g += `<text x="${X(k)}" y="${H - B + 18}" text-anchor="middle">${fmtParam(job.param, v)}</text>`; });
    g += `<text x="${(L + W - R) / 2}" y="${H - 6}" text-anchor="middle">${esc(SWEEP[job.param].label)}</text>`;
    const par = `<line class="parity" x1="${L}" x2="${W - R}" y1="${Y(parity)}" y2="${Y(parity)}"/><text x="${W - R}" y="${Y(parity) - 6}" text-anchor="end">Even share ${pct(parity, 1)}</text>`;
    let band = '', line = '', dots = '';
    if (res.length) {
      band = `<path class="band" d="M${res.map((r, k) => `${X(k)},${Y(r.hi)}`).join(' L')} L${res.map((r, k) => `${X(k)},${Y(r.lo)}`).reverse().join(' L')} Z"/>`;
      line = `<path class="line" d="M${res.map((r, k) => `${X(k)},${Y(r.winRate)}`).join(' L')}"/>`;
      const best = res.reduce((a, r, k) => r.winRate > res[a].winRate ? k : a, 0);
      dots = res.map((r, k) => `<g data-tip="${esc(`<b>${fmtParam(job.param, r.value)}</b><br>Wins ${pct(r.winRate, 1)} (${pct(r.lo, 1)} to ${pct(r.hi, 1)})<br>${r.ptsPerRound.toFixed(1)} points per round · bust ${pct(r.bustRate)}`)}">
        <rect class="hit" x="${X(k) - 14}" y="${T}" width="28" height="${H - T - B}"/><circle class="dot${k === best ? ' best' : ''}" cx="${X(k)}" cy="${Y(r.winRate)}" r="5"/></g>`).join('');
      dots += `<text class="val" x="${X(best)}" y="${Y(res[best].hi) - 8}" text-anchor="middle">Best ${fmtParam(job.param, res[best].value)}</text>`;
    }
    return `<svg class="chart" viewBox="0 0 ${W} ${H}" style="max-width:${W}px" role="img" aria-label="Win rate across the sweep">${g}${par}${band}${line}${dots}</svg>`;
  }

  function renderSweepResults(job) {
    const ent = simSettings.entrants[job.seat];
    const res = job.results;
    const best = res.reduce((a, r) => (!a || r.winRate > a.winRate) ? r : a, null);
    const overlap = best ? res.filter(r => r !== best && r.hi >= best.lo).length : 0;
    $('#swResults').innerHTML = `<p class="muted">Seat ${job.seat + 1} (${esc(ent.name)}) with ${esc(SWEEP[job.param].label.toLowerCase())} varied. Everything else is held fixed.</p>` +
      sweepChart(job) +
      `<div class="tblwrap"><table class="stbl sw"><tr><th>Value</th><th>Win rate</th><th>95% range</th><th>Points / round</th><th>Bust rate</th><th>Flip 7 rate</th></tr>
      ${res.map(r => `<tr><td>${r === best ? '<b>' + fmtParam(job.param, r.value) + '</b>' : fmtParam(job.param, r.value)}</td><td>${pct(r.winRate, 1)}</td><td><small>${pct(r.lo, 1)} to ${pct(r.hi, 1)}</small></td><td>${r.ptsPerRound.toFixed(1)}</td><td>${pct(r.bustRate)}</td><td>${pct(r.flip7Rate, 1)}</td></tr>`).join('')}</table></div>` +
      (best && res.length > 2 ? `<p class="notes" style="margin-top:10px">${overlap ? `${overlap} other value${overlap > 1 ? 's' : ''} overlap the best one's 95% range, so the peak isn't sharply defined at ${job.results.length ? (simSettings.sweep.games).toLocaleString() : ''} games per value. Raise games per value to tighten it.` : `The best value's 95% range clears every other value.`}</p>` : '');
  }

  // =========================================================
  // PROFILES
  // =========================================================
  const FIELDS = [
    { k: 'model', label: 'Decision model', type: 'select', opts: [['threshold', 'Rules of thumb'], ['ev', 'Expected value'], ['random', 'Random (baseline)']],
      help: 'Rules of thumb use a risk cap and a bank target. Expected value looks ahead over the cards left in the deck.' },
    { k: 'riskCap', label: 'Risk cap', type: 'range', min: 0, max: 100, step: 1, pct: true, models: ['threshold'], help: 'Hits only while the chance that the next card busts is at or below this.' },
    { k: 'bankAt', label: 'Bank at', type: 'number', min: 0, max: 300, step: 1, models: ['threshold'], help: 'Stays once the round is worth this many points. 0 turns it off.' },
    { k: 'bankWith', label: 'Bank with', type: 'select', opts: [['0', 'Off'], ['2', '2 numbers'], ['3', '3 numbers'], ['4', '4 numbers'], ['5', '5 numbers'], ['6', '6 numbers']], num: true, models: ['threshold'], help: 'Once it holds this many different numbers, it stays at a lower target set by the low-hand floor. Flip 7 chasing takes priority.' },
    { k: 'bankWithFloor', label: 'Low-hand floor', type: 'range', min: 0, max: 100, step: 5, pct: true, models: ['threshold'], when: p => p.bankWith > 0, whenNote: ' Only used when Bank with is on.', help: p => floorHelp(p) },
    { k: 'evDepth', label: 'Lookahead', type: 'select', opts: [['1', '1 card'], ['2', '2 cards'], ['3', '3 cards (slower)']], num: true, models: ['ev'], help: 'How many of its own future draws it plans for.' },
    { k: 'evMargin', label: 'EV margin', type: 'range', min: -30, max: 50, step: 1, pct: true, models: ['ev'], help: 'Hits only if hitting beats staying by this much. Positive is cautious, negative is greedy.' },
    { k: 'stayStart', label: 'Stay chance with one number', type: 'range', min: 0, max: 100, step: 1, pct: true, models: ['random'], help: 'How likely it is to stay when it holds one number card.' },
    { k: 'stayStep', label: 'Stay chance added per number', type: 'range', min: 0, max: 50, step: 1, pct: true, models: ['random'], help: 'Added to that chance for each further number card, so it stays more often as its hand grows.' },
    { k: 'counting', label: 'Card counting', type: 'select', opts: [['none', 'Own cards only'], ['table', 'Table this round'], ['limited', 'Limited memory'], ['full', 'Every card since the shuffle']], help: 'What it knows when it works out bust odds: just its own cards (as if the deck were fresh), everything on the table this round, some cards remembered since the shuffle (set by memory bandwidth), or every card since the shuffle.' },
    { k: 'countBandwidth', label: 'Memory bandwidth', type: 'range', min: 1, max: 12, step: 1, when: p => p.counting === 'limited', whenNote: ' Only used with Limited memory counting.', help: p => bandwidthHelp(p.countBandwidth) },
    { k: 'chase7At', label: 'Chase Flip 7 at', type: 'select', opts: [['0', 'Off'], ['3', '3 numbers'], ['4', '4 numbers'], ['5', '5 numbers'], ['6', '6 numbers']], num: true, models: ['threshold', 'ev'], help: 'Once it holds this many different numbers, it pushes harder for the +15.' },
    { k: 'chaseBoost', label: 'Chase boost', type: 'range', min: 0, max: 50, step: 1, pct: true, models: ['threshold', 'ev'], when: p => p.chase7At > 0, whenNote: ' Only used when Chase Flip 7 is on.', help: 'How much harder: added to the risk cap and taken off the EV margin.' },
    { k: 'comeback', label: 'Comeback aggression', type: 'range', min: 0, max: 100, step: 5, pct: true, models: ['threshold', 'ev'], help: 'Extra risk when it trails a leader who is past halfway. Scales with the deficit.' },
    { k: 'endgame', label: 'Reads the endgame', type: 'check', help: 'Keeps hitting when staying is a sure loss to someone already past the target, and stays when its banked total would finish ahead of everyone.' },
    { k: 'targeting', label: 'Action card targeting', type: 'select', opts: [['smart', 'Smart'], ['leader', 'Always the leader'], ['random', 'Random']], help: 'Where its Freeze and Flip Three go, and who gets a spare Second Chance. Smart estimates the points swung for each player.' }
  ];

  function trackedList(level) {
    const T = F7.AI.TIER;
    const top = Math.min(12, level);
    const nums = top >= 12 ? 'every number' : top === 1 ? 'the 0 and the 1' : `numbers 0 to ${top}`;
    const others = [[18, '×2'], [17, '+10'], [16, '+8'], [15, '+6'], [14, '+4'], [13, '+2'], [21, 'Second Chance'], [19, 'Freeze'], [20, 'Flip Three']]
      .filter(([t]) => T[t] <= level).map(x => x[1]);
    return [nums].concat(others).join(', ');
  }
  function floorHelp(p) {
    if (!(p.bankWith > 0)) return 'The lower target, as a share of Bank at. A hand with enough numbers but fewer points than this keeps drawing.';
    if (!(p.bankAt > 0)) return `Bank at is off, so there's no floor: it stays as soon as it holds ${p.bankWith} numbers, whatever they're worth.`;
    const floor = Math.round(p.bankAt * p.bankWithFloor);
    return `With Bank at ${p.bankAt}, it stays at ${p.bankWith} or more numbers once the round is worth ${floor}. A lower-value hand keeps drawing if its risk cap allows.`;
  }
  function bandwidthHelp(level) {
    level = Math.max(1, Math.min(12, Math.round(level) || 1));
    return level >= 12 ? 'Level 12 remembers every card since the shuffle: the same as a full count.'
      : `Level ${level} remembers exactly, since the shuffle: ${trackedList(level)}. For other cards it knows only this round's table and the size of the draw pile.`;
  }

  function renderProfiles() {
    const list = allProfiles();
    if (!list.some(p => p.id === selectedProfileId)) selectedProfileId = list[0].id;
    $('#profList').innerHTML = list.map(p => `<button type="button" class="prof-item${p.id === selectedProfileId ? ' sel' : ''}" data-id="${p.id}">
      <span class="pi-name">${esc(p.name)}${tableNameOf(p.id) ? ` <span class="pi-as">plays as ${esc(tableNameOf(p.id))}</span>` : ''}</span><span class="pi-tag">${p.builtin ? 'Built-in' : 'Custom'} · ${MODEL_LABEL[p.model]}${isAvailable(p) ? '' : ' · <span class="lab-tag">Lab only</span>'}</span>
      <span class="pi-desc">${esc(p.desc || '')}</span></button>`).join('');
    renderEditor();
  }

  function renderEditor() {
    const p = profileById(selectedProfileId);
    const ro = !!p.builtin;
    const fld = f => {
      const modelOff = !!(f.models && !f.models.includes(p.model));
      const off = modelOff || !!(f.when && !f.when(p));
      const offNote = modelOff ? ' Not used by this model.' : f.whenNote;
      const help = typeof f.help === 'function' ? f.help(p) : f.help;
      const v = p[f.k];
      let ctl;
      if (f.type === 'select') ctl = `<select id="pe_${f.k}" ${ro ? 'disabled' : ''}>${f.opts.map(([val, txt]) => `<option value="${val}"${String(v) === val ? ' selected' : ''}>${txt}</option>`).join('')}</select>`;
      else if (f.type === 'range') { const sv = f.pct ? Math.round(v * 100) : v; ctl = `<div class="ctl-row"><input type="range" id="pe_${f.k}" min="${f.min}" max="${f.max}" step="${f.step}" value="${sv}" ${ro ? 'disabled' : ''}><output id="po_${f.k}">${sv}${f.pct ? '%' : ''}</output></div>`; }
      else if (f.type === 'number') ctl = `<input type="number" id="pe_${f.k}" min="${f.min}" max="${f.max}" step="${f.step}" value="${v}" ${ro ? 'disabled' : ''} style="max-width:120px">`;
      else ctl = `<label class="chk"><input type="checkbox" id="pe_${f.k}" ${v ? 'checked' : ''} ${ro ? 'disabled' : ''}> On</label>`;
      return `<label class="pe-f${off ? ' off' : ''}" for="pe_${f.k}"><span>${f.label}</span>${ctl}<small id="ph_${f.k}">${help}${off ? offNote : ''}</small></label>`;
    };
    $('#profEditor').innerHTML = `
      <div class="pe-actions"><h2 style="margin:0">${esc(p.name)}</h2><span class="pill">${ro ? 'Built-in' : 'Custom'}</span></div>
      <label class="pe-avail" for="pe_available"><span class="chk"><input type="checkbox" id="pe_available" ${isAvailable(p) ? 'checked' : ''}> Available at the table</span>
        <small>Off keeps this profile out of the Table setup list and random picks. It's still available in Stress test.</small></label>
      <label class="pe-avail" for="pe_tname"><span>Table name <span class="muted">(optional)</span></span>
        <input type="text" id="pe_tname" value="${esc(tableNameOf(p.id))}" maxlength="18" placeholder="For example, Kris">
        <small>Seats playing this profile use this name, for a profile modeled on someone you play with. A named profile sits at the table at most once when players are randomized. Leave it blank for random names.</small></label>
      ${ro ? `<p class="lede">Built-in profiles can't be changed, so earlier test results stay comparable. Copy it to experiment.</p>`
        : `<label class="pe-f" for="pe_name"><span>Name</span><input type="text" id="pe_name" value="${esc(p.name)}" maxlength="28"></label>
           <label class="pe-f" for="pe_desc"><span>Notes</span><textarea id="pe_desc" maxlength="300">${esc(p.desc || '')}</textarea></label>`}
      <div class="pe-grid">${FIELDS.map(fld).join('')}</div>
      <div class="pe-actions">
        <button class="btn primary" type="button" data-pa="copy">Make an editable copy</button>
        ${ro ? '' : '<button class="btn" type="button" data-pa="delete">Delete profile</button><span class="hint">Changes save as you edit.</span>'}
      </div>`;
  }

  function bindProfiles() {
    $('#profList').addEventListener('click', e => {
      const b = e.target.closest('.prof-item');
      if (!b) return;
      selectedProfileId = b.dataset.id; renderProfiles();
    });
    $('#profNew').addEventListener('click', () => {
      const p = Object.assign({}, F7.AI.DEFAULTS, { id: 'c_' + Date.now().toString(36), name: 'New profile', desc: '' });
      customProfiles.push(p); selectedProfileId = p.id; afterProfileChange(); renderProfiles();
    });
    const onEdit = e => {
      const p = profileById(selectedProfileId);
      if (e.target.id === 'pe_tname') {
        const v = e.target.value.trim();
        if (v) profileNames[p.id] = v; else delete profileNames[p.id];
        persist();
        const item = $(`.prof-item[data-id="${p.id}"] .pi-name`);
        if (item) item.innerHTML = esc(p.name) + (v ? ` <span class="pi-as">plays as ${esc(v)}</span>` : '');
        return;
      }
      if (e.target.id === 'pe_available') {
        if (e.type !== 'change') return;
        hiddenProfiles = hiddenProfiles.filter(id => id !== p.id);
        if (!e.target.checked) hiddenProfiles.push(p.id);
        persist(); renderProfiles(); renderSetup();
        return;
      }
      if (p.builtin) return;
      const id = e.target.id;
      if (id === 'pe_name') { p.name = e.target.value || 'Untitled'; afterProfileChange(true); return; }
      if (id === 'pe_desc') { p.desc = e.target.value; afterProfileChange(true); return; }
      const f = FIELDS.find(x => 'pe_' + x.k === id);
      if (!f) return;
      let v;
      if (f.type === 'check') v = e.target.checked;
      else if (f.type === 'range') { v = +e.target.value; const o = $('#po_' + f.k); if (o) o.textContent = v + (f.pct ? '%' : ''); if (f.pct) v = v / 100; }
      else if (f.type === 'number') v = Math.max(f.min, Math.min(f.max, +e.target.value || 0));
      else v = f.num ? +e.target.value : e.target.value;
      p[f.k] = v;
      if (f.k === 'countBandwidth') { const h = $('#ph_countBandwidth'); if (h) h.textContent = bandwidthHelp(v); }
      if (f.k === 'bankAt' || f.k === 'bankWithFloor') { const h = $('#ph_bankWithFloor'); if (h) h.textContent = floorHelp(p) + (p.bankWith > 0 ? '' : ' Only used when Bank with is on.'); }
      afterProfileChange(true);
      if ((f.k === 'model' || f.k === 'counting' || f.k === 'bankWith' || f.k === 'chase7At') && e.type === 'change') renderEditor();
    };
    $('#profEditor').addEventListener('input', onEdit);
    $('#profEditor').addEventListener('change', onEdit);
    $('#profEditor').addEventListener('click', e => {
      const b = e.target.closest('button[data-pa]');
      if (!b) return;
      const p = profileById(selectedProfileId);
      if (b.dataset.pa === 'copy') {
        const c = Object.assign({}, p, { id: 'c_' + Date.now().toString(36), name: 'Copy of ' + p.name, builtin: false });
        delete c.builtin;
        if (!isAvailable(p)) hiddenProfiles.push(c.id);
        customProfiles.push(c); selectedProfileId = c.id; afterProfileChange(); renderProfiles();
      } else if (b.dataset.pa === 'delete' && !p.builtin) {
        customProfiles = customProfiles.filter(x => x.id !== p.id);
        hiddenProfiles = hiddenProfiles.filter(id => id !== p.id);
        delete profileNames[p.id];
        settings.seats.forEach(s => { if (s.profileId === p.id) s.profileId = 'balanced'; });
        simSettings.entrants.forEach(s => { if (s.profileId === p.id) s.profileId = 'balanced'; });
        selectedProfileId = 'balanced'; afterProfileChange(); renderProfiles();
      }
    });
  }

  function afterProfileChange(light) {
    persist();
    if (light) {
      const p = profileById(selectedProfileId);
      const item = $(`.prof-item[data-id="${p.id}"]`);
      if (item) { item.querySelector('.pi-name').innerHTML = esc(p.name) + (tableNameOf(p.id) ? ` <span class="pi-as">plays as ${esc(tableNameOf(p.id))}</span>` : ''); item.querySelector('.pi-desc').textContent = p.desc || ''; }
      return;
    }
    renderSetup();
    renderSimSetup();
  }

  // =========================================================
  // BOOT
  // =========================================================
  const clone = o => JSON.parse(JSON.stringify(o));
  const DEFAULTS_SNAPSHOT = { settings: clone(settings), simSettings: clone(simSettings) };
  const isObj = o => o && typeof o === 'object' && !Array.isArray(o);
  const MODELS = ['threshold', 'ev', 'random'];

  // Load saved or imported data over the current state, then repair anything malformed
  function loadState(src) {
    if (!isObj(src)) return;
    if (Array.isArray(src.customProfiles)) {
      const seen = new Set();
      customProfiles = src.customProfiles.filter(isObj).map((p, k) => {
        const c = Object.assign({}, F7.AI.DEFAULTS, p, { builtin: false });
        if (typeof c.id !== 'string' || !c.id || seen.has(c.id) || F7.AI.PRESETS.some(b => b.id === c.id)) c.id = 'c_' + Date.now().toString(36) + k;
        seen.add(c.id);
        c.name = String(c.name || 'Untitled').slice(0, 28);
        if (!MODELS.includes(c.model)) c.model = 'threshold';
        for (const key of Object.keys(F7.AI.DEFAULTS)) if (typeof F7.AI.DEFAULTS[key] === 'number' && !isFinite(+c[key])) c[key] = F7.AI.DEFAULTS[key];
        return c;
      });
    }
    if (Array.isArray(src.hiddenProfiles)) hiddenProfiles = src.hiddenProfiles.filter(x => typeof x === 'string');
    if (isObj(src.profileNames)) {
      profileNames = {};
      for (const k of Object.keys(src.profileNames)) { const v = String(src.profileNames[k] || '').trim().slice(0, 18); if (v) profileNames[k] = v; }
    }
    if (isObj(src.settings) && Array.isArray(src.settings.seats) && src.settings.seats.length >= 3) {
      const so = src.settings;
      settings = Object.assign(settings, so, { house: Object.assign({}, settings.house, so.house) });
      // Older saves had one switch for odds, card count and advisor
      if (so.showCount === undefined) settings.showCount = so.showOdds !== false;
      if (so.showAdvisor === undefined) settings.showAdvisor = so.showOdds !== false;
    }
    if (isObj(src.simSettings) && Array.isArray(src.simSettings.entrants) && src.simSettings.entrants.length >= 3) simSettings = Object.assign(simSettings, src.simSettings, { house: Object.assign({}, simSettings.house, src.simSettings.house), sweep: Object.assign({}, simSettings.sweep, src.simSettings.sweep), ladder: Object.assign({}, simSettings.ladder, src.simSettings.ladder) });
    repairState();
  }
  function repairState() {
    const ids = new Set(allProfiles().map(p => p.id));
    const fixSeat = (x, i) => ({ name: String((x && x.name) || NAMES[i]).slice(0, 18), kind: x && x.kind === 'human' ? 'human' : 'ai', profileId: x && ids.has(x.profileId) ? x.profileId : 'balanced' });
    settings.seats = (Array.isArray(settings.seats) ? settings.seats : []).slice(0, 8).map(fixSeat);
    while (settings.seats.length < 3) settings.seats.push(fixSeat(null, settings.seats.length));
    settings.target = Math.max(50, Math.min(1000, Math.round(+settings.target || 200)));
    if (!SPEEDS.hasOwnProperty(settings.speed)) settings.speed = 'normal';
    if (!['rotate', 'winnerLeft', 'winner'].includes(settings.house.opener)) settings.house.opener = 'rotate';
    if (settings.theme !== 'light') settings.theme = 'dark';
    simSettings.entrants = (Array.isArray(simSettings.entrants) ? simSettings.entrants : []).filter(isObj).slice(0, 8).map(e => {
      const out = { name: String(e.name || '').slice(0, 22), profileId: ids.has(e.profileId) ? e.profileId : 'balanced' };
      if (isObj(e.overrides)) out.overrides = e.overrides;
      return out;
    });
    while (simSettings.entrants.length < 3) simSettings.entrants.push({ name: 'Balanced', profileId: 'balanced' });
    simSettings.games = Math.max(10, Math.min(100000, Math.round(+simSettings.games || 2000)));
    simSettings.target = Math.max(50, Math.min(1000, Math.round(+simSettings.target || 200)));
    if (!ids.has(simSettings.ladder.base)) simSettings.ladder.base = 'balanced';
    for (const box of [simSettings.sweep, simSettings.ladder]) {
      if (!SWEEP[box.param]) { box.param = 'bankAt'; const d = SWEEP.bankAt.def; box.from = d[0]; box.to = d[1]; box.step = d[2]; }
    }
    if (!ids.has(selectedProfileId)) selectedProfileId = 'counter';
  }

  // ---------------- menu ----------------
  function setMenu(open) {
    $('#menu').hidden = !open;
    $('#menuBtn').setAttribute('aria-expanded', String(open));
  }
  function openSetup() {
    setMenu(false);
    showTab('play');
    const box = $('#setupBox');
    box.classList.add('forced');
    box.open = true;
    box.scrollIntoView({ behavior: 'smooth', block: 'start' });
  }
  function bindMenu() {
    $('#menuBtn').addEventListener('click', e => { e.stopPropagation(); setMenu($('#menu').hidden); });
    $('#menu').addEventListener('click', e => {
      const b = e.target.closest('button');
      if (!b) return;
      if (b.dataset.go) { showTab(b.dataset.go); setMenu(false); window.scrollTo(0, 0); }
      else if (b.dataset.menu === 'setup') openSetup();
      else if (b.dataset.menu === 'theme') { settings.theme = settings.theme === 'light' ? 'dark' : 'light'; applyTheme(); persist(); }
      else if (b.dataset.menu === 'backup') { setMenu(false); openBackup(); }
    });
    document.addEventListener('click', e => { if (!$('#menu').hidden && !e.target.closest('#menu')) setMenu(false); });
    document.addEventListener('keydown', e => {
      if (e.key !== 'Escape') return;
      if (!$('#backupModal').hidden) closeBackup();
      else if (!$('#menu').hidden) { setMenu(false); $('#menuBtn').focus(); }
    });
    // On phones the setup panel only shows when opened from the menu
    $('#setupBox').addEventListener('toggle', e => { if (!e.target.open) e.target.classList.remove('forced'); });
  }

  // ---------------- backup ----------------
  const BACKUP_APP = 'flip7-strategy-lab';
  let importPending = null;
  // Inside the Claude viewer, file saves go through the downloads capability (null = not available).
  // Standalone (no window.claude), a plain browser download works.
  let dlNS = null;
  try { if (window.claude && window.claude.use) window.claude.use('downloads').then(ns => { dlNS = ns || null; }, () => { dlNS = null; }); } catch (e) { dlNS = null; }
  const canDownload = () => !!dlNS || !window.claude;
  function exportObj() {
    return { app: BACKUP_APP, version: 1, exported: new Date().toISOString(), settings, simSettings,
      customProfiles: customProfiles.map(p => { const c = Object.assign({}, p); delete c.builtin; return c; }), hiddenProfiles, profileNames };
  }
  function openBackup() {
    const m = $('#backupModal');
    $('#bkOut').value = JSON.stringify(exportObj());
    $('#bkCopyMsg').textContent = `${customProfiles.length} custom profile${customProfiles.length === 1 ? '' : 's'}, ${settings.seats.length} seats.`;
    $('#bkDownload').hidden = !canDownload();
    $('#bkIn').value = ''; $('#bkPreview').innerHTML = ''; importPending = null;
    m.hidden = false;
    $('#bkClose').focus();
  }
  function closeBackup() { $('#backupModal').hidden = true; $('#menuBtn').focus(); }
  function previewImport(text) {
    importPending = null;
    const out = $('#bkPreview');
    let obj;
    try { obj = JSON.parse(String(text).trim()); } catch (e) { out.innerHTML = `<p class="warn">That isn't readable backup text. Copy the whole block from Export, from the first { to the last }.</p>`; return; }
    if (!isObj(obj) || (obj.app !== BACKUP_APP && !obj.settings && !obj.customProfiles)) { out.innerHTML = `<p class="warn">That text isn't a Flip 7 Strategy Lab backup.</p>`; return; }
    importPending = obj;
    const profs = Array.isArray(obj.customProfiles) ? obj.customProfiles.filter(isObj) : [];
    const seats = obj.settings && Array.isArray(obj.settings.seats) ? obj.settings.seats.length : 0;
    const when = obj.exported && !isNaN(Date.parse(obj.exported)) ? new Date(obj.exported).toLocaleString() : 'an unknown date';
    out.innerHTML = `<div class="bk-sum"><p>Backup from ${esc(when)}: ${profs.length} custom profile${profs.length === 1 ? '' : 's'}${profs.length ? ' (' + profs.slice(0, 6).map(p => esc(p.name || 'Untitled')).join(', ') + (profs.length > 6 ? ', …' : '') + ')' : ''}${seats ? `, ${seats} seats` : ''}.</p>
      <p class="warn">Importing replaces your current profiles and settings, and restarts the game in progress.</p>
      <button class="btn primary" id="bkApply" type="button">Replace my settings</button></div>`;
  }
  function applyImport() {
    if (!importPending) return;
    const keepTab = currentTab;
    settings = clone(DEFAULTS_SNAPSHOT.settings); simSettings = clone(DEFAULTS_SNAPSHOT.simSettings);
    customProfiles = []; hiddenProfiles = []; profileNames = {};
    loadState(importPending);
    importPending = null;
    persist(); applyTheme();
    renderSetup(); renderSimSetup();
    if (keepTab === 'profiles') renderProfiles();
    newGame();
    $('#bkPreview').innerHTML = `<p class="ok-msg">Imported ${customProfiles.length} custom profile${customProfiles.length === 1 ? '' : 's'} and your settings. A new game has started.</p>`;
    $('#bkOut').value = JSON.stringify(exportObj());
  }
  function bindBackup() {
    $('#bkClose').addEventListener('click', closeBackup);
    $('#backupModal').addEventListener('click', e => { if (e.target.id === 'backupModal') closeBackup(); });
    $('#bkCopy').addEventListener('click', () => {
      const ta = $('#bkOut'), msg = $('#bkCopyMsg');
      const fallback = () => { ta.focus(); ta.select(); msg.textContent = "Copy didn't go through here. The text is selected: press Ctrl+C (Cmd+C on a Mac), or long-press it and choose Copy."; };
      try {
        navigator.clipboard.writeText(ta.value).then(() => { msg.textContent = 'Copied. Paste it somewhere safe, or into Import on another copy of the Lab.'; }, fallback);
      } catch (e) { fallback(); }
    });
    $('#bkDownload').addEventListener('click', () => {
      const filename = 'flip7-lab-backup-' + new Date().toISOString().slice(0, 10) + '.json';
      const data = JSON.stringify(exportObj(), null, 1);
      const msg = $('#bkCopyMsg');
      if (dlNS) {
        dlNS.save({ filename, data }).then(r => {
          if (r && r.status === 'saved') msg.textContent = 'Saved ' + filename + '.';
        }, err => {
          const code = err && err.code;
          if (code === 'declined') msg.textContent = 'Download cancelled.';
          else if (code === 'rate_limited') msg.textContent = 'A save prompt is already open. Try again in a moment.';
          else { $('#bkDownload').hidden = true; msg.textContent = "Downloads aren't available here. Use Copy instead."; }
        });
        return;
      }
      try {
        const blob = new Blob([data], { type: 'application/json' });
        const a = document.createElement('a');
        a.href = URL.createObjectURL(blob);
        a.download = filename;
        document.body.appendChild(a); a.click(); a.remove();
        setTimeout(() => URL.revokeObjectURL(a.href), 2000);
      } catch (e) { $('#bkCopyMsg').textContent = "This browser wouldn't save the file. Use Copy instead."; }
    });
    $('#bkFileBtn').addEventListener('click', () => $('#bkFile').click());
    $('#bkFile').addEventListener('change', e => {
      const f = e.target.files && e.target.files[0];
      if (!f) return;
      const r = new FileReader();
      r.onload = () => { $('#bkIn').value = String(r.result); previewImport(r.result); };
      r.onerror = () => { $('#bkPreview').innerHTML = `<p class="warn">That file couldn't be read.</p>`; };
      r.readAsText(f);
      e.target.value = '';
    });
    $('#bkCheck').addEventListener('click', () => previewImport($('#bkIn').value));
    $('#bkPreview').addEventListener('click', e => { if (e.target.id === 'bkApply') applyImport(); });
  }

  function boot(data) {
    const saved = store.get(STORE_KEY, null);
    const src = (data && data.settings) ? data : saved;
    loadState(src);
    if (data && data.tab) currentTab = data.tab;
    if (data && data.selectedProfileId) selectedProfileId = data.selectedProfileId;
    repairState();
    applyTheme();

    $$('.tab-btn').forEach(b => b.addEventListener('click', () => showTab(b.dataset.tab)));
    bindSetup(); bindPlay(); bindSim(); bindProfiles(); bindMenu(); bindBackup();
    renderSetup(); renderSimSetup();
    showTab(currentTab);
    newGame();
  }

  try {
    if (window.claude && window.claude.hot && window.claude.hot.snapshot) {
      window.claude.hot.snapshot(() => ({ settings, simSettings, customProfiles, hiddenProfiles, profileNames, tab: currentTab, selectedProfileId }));
    }
  } catch (e) { /* no viewer */ }
  const hot = window.claude && window.claude.hot;
  if (hot && hot.ready) hot.ready(boot); else boot((hot && hot.data) || {});
})();

</script>
</body>
</html>
