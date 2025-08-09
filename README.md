<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>FunStep: a Staircase-Themed Puzzle for Learning Rational Functions</title>
<style>
  :root{--bg:#0b0b0d; --neon:#00ffcc; --accent:#ff00ff; --panel:#000;}
  *{box-sizing:border-box}
  html,body{height:100%}
  body{
    margin:0; font-family:monospace,monospace; background:var(--bg); color:var(--neon);
    display:flex; flex-direction:column; align-items:stretch; min-height:100vh;
    background-image:repeating-linear-gradient(to bottom, rgba(255,255,255,0.02) 0px, rgba(255,255,255,0.02) 2px, transparent 2px, transparent 4px);
  }
  header{
    padding:12px; text-align:center; color:var(--accent); font-weight:bold;
    font-size:15px; letter-spacing:0.3px;
    text-shadow:0 0 8px var(--accent), 0 0 16px var(--accent);
    background:linear-gradient(90deg, rgba(255,0,255,0.04), rgba(0,255,204,0.02));
  }
  main{flex:1; padding:10px; max-width:520px; margin:0 auto; width:100%}
  .terminal{
    background:var(--panel); border:3px solid var(--neon); padding:12px;
    height:62vh; overflow:auto; font-size:13px; line-height:1.4; border-radius:8px;
    box-shadow:0 6px 20px rgba(0,0,0,0.6), 0 0 18px rgba(0,255,204,0.06) inset;
  }
  .term-line{margin:6px 0; white-space:pre-wrap; word-wrap:break-word}
  .controls{display:flex; gap:8px; margin-top:8px}
  input[type="text"]{
    flex:1; padding:10px; border:2px solid var(--accent); background:#07070a; color:var(--neon);
    border-radius:6px;
    outline:none;
  }
  button{
    padding:10px 12px; border:2px solid var(--accent); background:#07070a; color:var(--neon);
    cursor:pointer; border-radius:6px;
  }
  button:hover{background:var(--accent); color:#000}
  .status{font-size:12px; color:#9fb0c8; margin-top:8px}
  .hint{font-size:12px; color:#9fb0c8; margin-top:6px}
  @media (min-width:700px){ main{max-width:420px} }
</style>
</head>
<body>
  <header>FunStep: a Staircase-Themed Puzzle for Learning Rational Functions</header>
  <main>
    <div id="terminal" class="terminal" aria-live="polite"></div>

    <div class="controls">
      <input id="termInput" type="text" placeholder="Type here the card number or answer..." autocomplete="off" />
      <button id="runBtn" type="button">RUN</button>
    </div>

    <div class="status" id="status">Status: waiting</div>
    <div class="hint">How to use: Type a card number (e.g. <strong>1</strong>) to load the question; then type the answer (e.g. <strong>-0.5</strong>).</div>
  </main>

<script>
window.addEventListener('DOMContentLoaded', function() {
  // Questions use plain ASCII to avoid encoding issues on some hosts
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

  function setStatus(s) {
    status.textContent = 'Status: ' + s;
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
        setStatus('unknown card');
        return;
      }
      currentCard = n;
      logLine('📜 Card ' + n + ' loaded: ' + correctAnswers[n].type + ' — ' + correctAnswers[n].question);
      setStatus('card ' + n + ' loaded');
      return;
    }
    // Else treat as answer
    if (!currentCard) {
      logLine('⚠ No card loaded. Type the card number first (e.g. 1).');
      setStatus('no card');
      return;
    }
    const ans = String(raw).trim().toLowerCase();
    const valid = correctAnswers[currentCard].answers.map(a => String(a).trim().toLowerCase());
    if (valid.includes(ans)) {
      logLine('✅ Correct! 🎉');
      setStatus('correct');
      // optionally auto-unload after correct (comment out if unwanted)
      // currentCard = null;
    } else {
      logLine('❌ Wrong. Try again or type another card number.');
      setStatus('wrong');
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
      setStatus('error');
    }
    input.value = '';
    input.focus();
  }

  btn.addEventListener('click', send);
  input.addEventListener('keydown', function(e){ if (e.key === 'Enter') send(); });

  // focus and welcome
  input.focus();
  logLine('WELCOME — FunStep Math Arcade');
  logLine('Type a card number to load the question (1, 2, 3...). Then type the answer to check.');
  setStatus('ready — ' + window.location.pathname);
});
</script>
</body>
</html>
