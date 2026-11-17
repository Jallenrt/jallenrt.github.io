
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Card Table</title>
<style>
  :root { --bg: #0f1218; --surface: #181d27; --surface-2: #202737; --line: #2b3347; --ink: #eef0f5; --muted: #98a1b5; --accent: #f2b441; }
  @media (prefers-color-scheme: light) {
    :root { --bg: #f4f1ea; --surface: #fffdf8; --surface-2: #f0ebe0; --line: #d9d2c2; --ink: #1d2230; --muted: #5d6578; --accent: #b87a00; }
  }
  * { box-sizing: border-box; }
  body { margin: 0; min-height: 100vh; background: var(--bg); color: var(--ink); font: 16px/1.5 system-ui, -apple-system, "Segoe UI", Roboto, sans-serif; display: grid; place-items: center; padding: 28px 16px; }
  main { width: 100%; max-width: 760px; }
  h1 { font: 400 clamp(34px, 8vw, 52px)/1.1 Georgia, "Times New Roman", serif; margin: 0 0 6px; }
  h1 span { color: var(--accent); }
  .sub { color: var(--muted); margin: 0 0 28px; }
  .games { display: grid; gap: 14px; }
  a.game { display: grid; grid-template-columns: auto 1fr auto; gap: 16px; align-items: center; text-decoration: none; color: inherit; background: var(--surface); border: 1px solid var(--line); border-radius: 14px; padding: 18px 20px; transition: transform .15s, border-color .15s, background .15s; }
  a.game:hover, a.game:focus-visible { border-color: var(--accent); background: var(--surface-2); transform: translateY(-2px); outline: none; }
  .icon { width: 52px; height: 72px; border-radius: 8px; border: 2px solid var(--accent); display: grid; place-items: center; font: 400 30px Georgia, serif; color: var(--accent); background: var(--bg); transform: rotate(-4deg); }
  .game h2 { font: 400 22px/1.2 Georgia, serif; margin: 0 0 3px; }
  .game p { margin: 0; color: var(--muted); font-size: 15px; }
  .tag { display: inline-block; margin-top: 8px; font-size: 12px; letter-spacing: .08em; text-transform: uppercase; font-weight: 700; color: var(--accent); }
  .go { font-size: 26px; color: var(--accent); }
  @media (max-width: 480px) { a.game { grid-template-columns: auto 1fr; } .go { display: none; } }
  footer { margin-top: 26px; color: var(--muted); font-size: 13px; }
</style>
</head>
<body>
<main>
  <h1>Card <span>Table</span></h1>
  <p class="sub">Pick a game.</p>
  <nav class="games" aria-label="Games">
    <a class="game" href="https://jallenrt.github.io/flip7/">
      <div class="icon" aria-hidden="true">7</div>
      <div>
        <h2>Flip 7 Strategy Lab</h2>
        <p>Play against AI opponents, or run thousands of simulated games to see which strategy wins.</p>
        <span class="tag">3 to 8 seats · play vs AI</span>
      </div>
      <div class="go" aria-hidden="true">›</div>
    </a>
<!--   <a class="game" href="three-dragon-ante/">
      <div class="icon" aria-hidden="true">3</div>
      <div>
        <h2>Three Dragon Ante</h2>
        <p>The dragon-themed betting and card game.</p>
        <span class="tag">Card game</span>
      </div>
      <div class="go" aria-hidden="true">›</div>
    </a>
    <a class="game" href="giants-war/">
      <div class="icon" aria-hidden="true">G</div>
      <div>
        <h2>Three Dragon Ante: Giants War</h2>
        <p>The Giants War variant.</p>
        <span class="tag">Card game</span>
      </div>
      <div class="go" aria-hidden="true">›</div>
    </a>
-->
  </nav>
  <footer>Games run in your browser. Saved settings stay on your device.</footer>
</main>
</body>
</html>
