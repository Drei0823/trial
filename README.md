<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Arcade Math Terminal — Fixed</title>
<style>
  /* Simple, no-external-font version for reliability on GitHub Pages */
  :root{--bg:#0b0b0d; --neon:#00ffcc; --accent:#ff00ff; --panel:#000;}
  *{box-sizing:border-box}
  html,body{height:100%}
  body{
    margin:0; font-family:monospace,monospace; background:var(--bg); color:var(--neon);
    display:flex; flex-direction:column; align-items:stretch; min-height:100vh;
    background-image:repeating-linear-gradient(to bottom, rgba(255,255,255,0.02) 0px, rgba(255,255,255,0.02) 2px, transparent 2px, transparent 4px);
  }
  header{padding:12px; text-align:center; color:var(--accent); font-weight:bold; font-size:14px}
  main{flex:1; padding:10px; max-width:420px; margin:0 auto; width:100%}
  .terminal{background:var(--panel); border:3px solid var(--neon); padding:12px; height:60vh; overflow:auto; font-size:13px; line-height:1.4}
  .term-line{margin:6px 0; white-space:pre-wrap; word-wrap:break-word}
  .controls{display:flex; gap:8px; margin-top:8px}
  input[type="text"]{flex:1; padding:10px; border:2px solid var(--accent); background:#07070a; color:var(--neon);}
  button{padding:10px 12px; border:2px solid var(--accent); background:#07070a; color:var(--neon); cursor:pointer}
  button:hover{background:var(--accent); color:#000}
  .status{font-size:12px; color:#9fb0c8; margin-top:6px}
  .note{font-size:12px; color:#9fb0c8; margin-top:8px;}
</style>
</head>
<body>
  <header>RETRO MATH TERMINAL — SIMPLE FIX</header>
  <main>
    <div id="terminal" class="terminal" aria-live="polite"></div>

    <div class="controls">
      <input id="termInput" type="text" placeholder="Type card number and after put your answer..." autocomplete="off" />
      <button id="runBtn" type="button">RUN</button>
    </div>

    <div class="status" id="status">Status: waiting</div>
    <div class="note">Save this file as <strong>index.html</strong> (UTF-8) and upload to your repo root. Open the Pages URL, not the raw file.</div>
  </main>

<script>
// Self-contained script with robust checks
window.addEventListener('DOMContentLoaded', function() {
  // Use simple ASCII in questions to avoid charset issues
  const correctAnswers = {
    1: { type: "Rational Equation", question: "1/(x+1) = 2. What is x?", answers: ["-0.5", "-1/2"] },
    2: { type: "Inequality", question: "2x - 3 > 5. What is the solution?", answers: ["x > 4"] },
    3: { type: "Function", question: "f(x) = x^2 + 1. Find f(3)", answers: ["10"] }
  };

  let currentCard = null;
  const term = document.getElementById('terminal');
  const input = document.getElementById('termInput');
  const btn = document.getElementById('runBtn');
  const status = document.getElementById('status');

  function logLine(text) {
    const d = document.createElement('div');
    d.className = 'term-line';
    d.textContent = text;
    term.appendChild(d);
    term.scrollTop = term.scrollHeight;
  }

  function printWelcome() {
    logLine('WELCOME — Math Arcade (simple mode)');
    logLine('Type a card number (e.g. 1) to load question.');
    logLine('Then type an answer to check it. No commands needed.');
    status.textContent = 'Status: ready — Page URL: ' + window.location.href;
  }

  function isIntegerString(s) {
    return /^-?\d+$/.test(String(s).trim());
  }

  function handleInput(raw) {
    if (!raw) return;
    logLine('> ' + raw);
    // If integer -> load card
    if (isIntegerString(raw)) {
      const n = parseInt(raw, 10);
      if (!correctAnswers[n]) {
        logLine('❌ Card ' + n + ' not found. Available: ' + Object.keys(correctAnswers).join(', '));
        return;
      }
      currentCard = n;
      logLine('📜 Card ' + n + ' loaded: ' + correctAnswers[n].type + ' — ' + correctAnswers[n].question);
      return;
    }
    // Else treat as answer
    if (!currentCard) {
      logLine('⚠ No card loaded. Type the card number first (e.g. 1).');
      return;
    }
    const ans = String(raw).trim().toLowerCase();
    const valid = correctAnswers[currentCard].answers.map(a => String(a).trim().toLowerCase());
    if (valid.includes(ans)) {
      logLine('✅ Correct!');
    } else {
      logLine('❌ Wrong. Try again or type another card number.');
    }
  }

  function send() {
    const v = input.value.trim();
    if (!v) return;
    try {
      handleInput(v);
    } catch (err) {
      logLine('ERROR: ' + String(err));
      console.error(err);
    }
    input.value = '';
    input.focus();
  }

  btn.addEventListener('click', send);
  input.addEventListener('keydown', function(e){ if (e.key === 'Enter') send(); });

  // Provide a basic visual test so user can confirm JS runs
  printWelcome();
});
</script>
</body>
</html>
