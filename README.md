<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Arcade Math Terminal</title>

<!-- Retro Pixel Arcade Font (fallback to monospace if blocked) -->
<link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap" rel="stylesheet">

<style>
  :root{
    --bg:#0d0d0d; --neon:#00ffcc; --accent:#ff00ff; --panel:#000;
  }
  html,body{height:100%;}
  body{
    margin:0;
    font-family: 'Press Start 2P', monospace, monospace;
    background:var(--bg);
    color:var(--neon);
    display:flex;
    flex-direction:column;
    min-height:100vh;
    background-image: repeating-linear-gradient(
      to bottom,
      rgba(255,255,255,0.02) 0px,
      rgba(255,255,255,0.02) 2px,
      transparent 2px,
      transparent 4px
    );
  }
  header{
    text-align:center;
    padding:12px 8px;
    font-size:14px;
    color:var(--accent);
    text-shadow:0 0 8px var(--accent), 0 0 16px var(--accent);
  }
  main{
    flex:1;
    display:flex;
    flex-direction:column;
    gap:10px;
    padding:8px;
    box-sizing:border-box;
  }
  .terminal{
    flex:1;
    background:var(--panel);
    border:3px solid var(--neon);
    padding:12px;
    box-shadow:0 0 16px var(--neon);
    overflow:auto;
    font-size:12px;
    line-height:1.4;
  }
  .term-line{ white-space:pre-wrap; word-wrap:break-word; margin:4px 0; }
  .term-input{ display:flex; gap:8px; align-items:center; }
  input[type="text"]{
    flex:1;
    padding:10px 8px;
    border:2px solid var(--accent);
    background:#06060a;
    color:var(--neon);
    font-family:inherit;
    font-size:12px;
    outline:none;
  }
  button{
    padding:10px 12px;
    background:#06060a;
    border:2px solid var(--accent);
    color:var(--neon);
    cursor:pointer;
    font-family:inherit;
    font-size:12px;
  }
  button:hover{ background:var(--accent); color:#000; box-shadow:0 0 8px var(--accent); }
  /* portrait friendly */
  @media (min-width:700px){
    main{max-width:420px;margin:0 auto;}
  }
</style>
</head>
<body>
  <header>🎮 RETRO MATH TERMINAL 🎮</header>
  <main>
    <div class="terminal" id="terminal" aria-live="polite"></div>

    <div class="term-input">
      <input id="termInput" type="text" placeholder="Type command (e.g. load 1, check -0.5)..." autocomplete="off" />
      <button id="runBtn" type="button">RUN</button>
    </div>
  </main>

<script>
// Wrap everything to be safe for all pages and ensure DOM is ready
window.addEventListener('DOMContentLoaded', function () {
  // Use ASCII-friendly strings to avoid encoding issues (x^2 instead of superscript)
  const correctAnswers = {
    1: { type: "Rational Equation", question: "1/(x+1) = 2. What is x?", answers: ["-0.5", "-1/2"] },
    2: { type: "Inequality", question: "2x - 3 > 5. What is the solution?", answers: ["x > 4"] },
    3: { type: "Function", question: "f(x) = x^2 + 1. Find f(3)", answers: ["10"] }
  };

  let currentCard = null;
  const terminal = document.getElementById('terminal');
  const termInput = document.getElementById('termInput');
  const runBtn = document.getElementById('runBtn');

  function appendToTerminal(text, cssClass) {
    const el = document.createElement('div');
    el.className = 'term-line' + (cssClass ? ' ' + cssClass : '');
    el.textContent = text;
    terminal.appendChild(el);
    terminal.scrollTop = terminal.scrollHeight;
  }

  function printWelcome() {
    appendToTerminal('WELCOME TO THE MATH ARCADE!');
    appendToTerminal(\"Type 'help' for commands.\");
  }

  function safeLower(s){ return String(s||'').trim().toLowerCase(); }

  function runCommand(cmd) {
    try {
      const raw = String(cmd||'').trim();
      if (!raw) return;
      const parts = raw.split(/\\s+/);
      const command = safeLower(parts[0]);
      const arg = parts.slice(1).join(' ');

      if (command === 'help') {
        appendToTerminal('COMMANDS:');
        appendToTerminal(' load <n>        - Load card number');
        appendToTerminal(' check <answer>  - Check answer for loaded card');
        appendToTerminal(' show            - Show current question');
        appendToTerminal(' list            - List available cards');
        appendToTerminal(' clear           - Clear terminal');
        return;
      }

      if (command === 'load') {
        const n = parseInt(arg, 10);
        if (!Number.isInteger(n) || !correctAnswers[n]) {
          appendToTerminal('❌ Card not found. Use: list');
        } else {
          currentCard = n;
          appendToTerminal('📜 Loaded Card ' + n + ': ' + correctAnswers[n].type + ' — ' + correctAnswers[n].question);
        }
        return;
      }

      if (command === 'check') {
        if (!currentCard) { appendToTerminal(\"⚠ Load a card first with 'load <n>'.\"); return; }
        const answer = safeLower(arg);
        const valid = correctAnswers[currentCard].answers.map(a => safeLower(a));
        appendToTerminal(valid.includes(answer) ? '✅ Correct!' : '❌ Wrong.');
        return;
      }

      if (command === 'show') {
        if (!currentCard) { appendToTerminal('⚠ No card loaded.'); return; }
        appendToTerminal('📜 Card ' + currentCard + ': ' + correctAnswers[currentCard].type + ' — ' + correctAnswers[currentCard].question);
        return;
      }

      if (command === 'list') {
        Object.keys(correctAnswers).forEach(k => {
          const d = correctAnswers[k];
          appendToTerminal(k + ': ' + d.type);
        });
        return;
      }

      if (command === 'clear') {
        terminal.innerHTML = '';
        printWelcome();
        return;
      }

      appendToTerminal('❓ Unknown command: ' + command);
    } catch (err) {
      // Show error in terminal and log to console (helpful for debugging on GitHub Pages)
      appendToTerminal('ERROR: ' + (err && err.message ? err.message : String(err)));
      console.error(err);
    }
  }

  function sendCommand() {
    const v = termInput.value.trim();
    if (!v) return;
    appendToTerminal('> ' + v);
    runCommand(v);
    termInput.value = '';
    termInput.focus();
  }

  runBtn.addEventListener('click', sendCommand);
  termInput.addEventListener('keydown', function (e) { if (e.key === 'Enter') sendCommand(); });

  // Auto-focus input for quick testing on devices
  termInput.focus();
  printWelcome();
});
</script>

</body>
</html>
