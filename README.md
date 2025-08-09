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
  }
  .term-line {
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
  <header>🎮 FunStep 🎮</header>
  <main>
    <div class="terminal" id="terminal"></div>

    <div class="term-input">
      <input id="termInput" placeholder="Type card number or answer..." autocomplete="off" />
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
    const line = document.createElement("div");
    line.className = "term-line";
    line.textContent = text;
    terminal.appendChild(line);
    terminal.scrollTop = terminal.scrollHeight;
  }

  function printWelcome() {
    appendToTerminal("WELCOME TO THE MATH ARCADE!");
    appendToTerminal("Type a card number to start (1, 2, or 3).");
  }

  function sendCommand() {
    const value = termInput.value.trim();
    if (!value) return;

    appendToTerminal("> " + value);

    // If input is a number → load that card
    if (!isNaN(value)) {
      const n = parseInt(value);
      if (!correctAnswers[n]) {
        appendToTerminal(`❌ Card ${n} not found.`);
      } else {
        currentCard = n;
        appendToTerminal(`📜 Card ${n}: ${correctAnswers[n].type} — ${correctAnswers[n].question}`);
      }
    }
    // Otherwise → check answer
    else {
      if (!currentCard) {
        appendToTerminal("⚠ Load a card first by typing its number.");
      } else {
        const ans = value.toLowerCase();
        const valid = correctAnswers[currentCard].answers.map(a => a.toLowerCase());
        if (valid.includes(ans)) {
          appendToTerminal("✅ Correct!");
        } else {
          appendToTerminal("❌ Wrong.");
        }
      }
    }

    termInput.value = "";
  }

  termInput.addEventListener("keydown", e => {
    if (e.key === "Enter") sendCommand();
  });

  printWelcome();
</script>

</body>
</html>
