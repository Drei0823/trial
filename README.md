<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Arcade Math Terminal</title>

<!-- Retro Pixel Arcade Font -->
<link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap" rel="stylesheet">

<style>
  body {
    margin: 0;
    font-family: 'Press Start 2P', cursive;
    background-color: #0d0d0d;
    color: #00ffcc;
    display: flex;
    flex-direction: column;
    height: 100vh;
    background-image: repeating-linear-gradient(
      to bottom,
      rgba(255, 255, 255, 0.02) 0px,
      rgba(255, 255, 255, 0.02) 2px,
      transparent 2px,
      transparent 4px
    );
  }
  header {
    text-align: center;
    padding: 1rem;
    font-size: 1rem;
    color: #ff00ff;
    text-shadow: 0 0 10px #ff00ff, 0 0 20px #ff00ff;
  }
  main {
    flex: 1;
    display: flex;
    flex-direction: column;
    padding: 0.5rem;
    gap: 1rem;
    overflow-y: auto;
  }
  .terminal {
    flex: 1;
    background: #000;
    border: 2px solid #00ffcc;
    padding: 1rem;
    overflow-y: auto;
    box-shadow: 0 0 15px #00ffcc;
    font-size: 0.8rem;
    line-height: 1.5;
    white-space: pre-wrap;
    word-wrap: break-word;
  }
  .term-input {
    display: flex;
    gap: 0.5rem;
  }
  .term-input input {
    flex: 1;
    background: black;
    color: #fff;
    border: 2px solid #ff00ff;
    padding: 0.8rem;
    font-family: 'Press Start 2P', cursive;
    font-size: 0.7rem;
  }
  .term-input button {
    background: black;
    color: #fff;
    border: 2px solid #ff00ff;
    padding: 0.8rem;
    cursor: pointer;
    text-shadow: 0 0 5px #ff00ff;
    font-family: 'Press Start 2P', cursive;
    font-size: 0.7rem;
  }
  .term-input button:hover {
    background: #ff00ff;
    color: black;
    box-shadow: 0 0 10px #ff00ff, 0 0 20px #ff00ff;
  }
</style>
</head>

<body>
  <header>🎮 RETRO MATH TERMINAL 🎮</header>
  <main>
    <div class="terminal" id="terminal">
WELCOME TO THE MATH ARCADE!
Type <span style="color:#ff00ff">help</span> for commands.
    </div>

    <div class="term-input">
      <input id="termInput" placeholder="Type command here..." autocomplete="off" />
      <button onclick="sendCommand()">RUN</button>
    </div>
  </main>

<script>
  const correctAnswers = {
    1: { type: "Rational Equation", question: "1/(x+1) = 2. What is x?", answers: ["-0.5", "-1/2"] },
    2: { type: "Inequality", question: "2x - 3 > 5. What is the solution?", answers: ["x > 4"] },
    3: { type: "Function", question: "f(x) = x² + 1. Find f(3)", answers: ["10"] }
  };

  let currentCard = null;
  const terminal = document.getElementById("terminal");
  const termInput = document.getElementById("termInput");

  function appendToTerminal(text) {
    terminal.innerHTML += "\n" + text;
    terminal.scrollTop = terminal.scrollHeight;
  }

  function runCommand(cmd) {
    const parts = cmd.trim().split(/\s+/);
    const command = parts[0]?.toLowerCase();
    const arg = parts.slice(1).join(" ");

    if (!command) return;

    if (command === "help") {
      appendToTerminal("COMMANDS:\n load <n> - Load card number\n check <answer> - Check answer\n show - Show current question\n list - List all cards\n clear - Clear terminal");
    }
    else if (command === "load") {
      const n = parseInt(arg);
      if (!correctAnswers[n]) {
        appendToTerminal(`❌ Card ${n} not found.`);
      } else {
        currentCard = n;
        appendToTerminal(`📜 Loaded Card ${n}: ${correctAnswers[n].type} — ${correctAnswers[n].question}`);
      }
    }
    else if (command === "check") {
      if (!currentCard) {
        appendToTerminal("⚠ Load a card first with 'load <n>'.");
      } else {
        const ans = arg.trim().toLowerCase();
        const valid = correctAnswers[currentCard].answers.map(a => a.toLowerCase());
        if (valid.includes(ans)) {
          appendToTerminal("✅ Correct!");
        } else {
          appendToTerminal("❌ Wrong.");
        }
      }
    }
    else if (command === "show") {
      if (!currentCard) {
        appendToTerminal("⚠ No card loaded.");
      } else {
        appendToTerminal(`📜 Card ${currentCard}: ${correctAnswers[currentCard].type} — ${correctAnswers[currentCard].question}`);
      }
    }
    else if (command === "list") {
      Object.keys(correctAnswers).forEach(k => {
        appendToTerminal(`${k}: ${correctAnswers[k].type}`);
      });
    }
    else if (command === "clear") {
      terminal.innerHTML = "Terminal cleared.";
    }
    else {
      appendToTerminal(`❓ Unknown command: ${command}`);
    }
  }

  function sendCommand() {
    const value = termInput.value.trim();
    if (value) {
      appendToTerminal("> " + value);
      runCommand(value);
      termInput.value = "";
    }
  }

  termInput.addEventListener("keydown", e => {
    if (e.key === "Enter") sendCommand();
  });
</script>

</body>
</html>
