<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>FunStep: a Staircase-Themed Puzzle for Learning Rational Functions</title>

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
    padding: 0.5rem;
    font-size: 0.6rem;
    color: #ff00ff;
    text-shadow: 0 0 5px #ff00ff, 0 0 10px #ff00ff;
  }
  main {
    flex: 1;
    display: flex;
    flex-direction: column;
    padding: 0.3rem;
    gap: 0.5rem;
    overflow-y: auto;
  }
  .terminal {
    flex: 1;
    background: #000;
    border: 1px solid #00ffcc;
    padding: 0.5rem;
    overflow-y: auto;
    box-shadow: 0 0 8px #00ffcc;
    font-size: 0.6rem;
    line-height: 1.3;
  }
  .term-line {
    white-space: pre-wrap;
    word-wrap: break-word;
  }
  .term-input {
    display: flex;
    gap: 0.3rem;
  }
  .term-input input {
    flex: 1;
    background: black;
    color: #fff;
    border: 1px solid #ff00ff;
    padding: 0.5rem;
    font-family: 'Press Start 2P', cursive;
    font-size: 0.5rem;
  }
  .term-input button {
    background: black;
    color: #fff;
    border: 1px solid #ff00ff;
    padding: 0.5rem;
    cursor: pointer;
    text-shadow: 0 0 3px #ff00ff;
    font-family: 'Press Start 2P', cursive;
    font-size: 0.5rem;
  }
  .term-input button:hover {
    background: #ff00ff;
    color: black;
    box-shadow: 0 0 5px #ff00ff, 0 0 10px #ff00ff;
  }
</style>
</head>

<body>
  <header>🎮 FunStep: a Staircase-Themed Puzzle for Learning Rational Functions 🎮</header>
  <main>
    <div class="terminal" id="terminal"></div>

    <div class="term-input">
      <input id="termInput" placeholder="Type card number and answer..." autocomplete="off" />
      <button onclick="sendInput()">OK</button>
    </div>
  </main>

<script>
  const correctAnswers = {
    1: { question: "1/(x+1) = 2. What is x?", answers: ["-0.5", "-1/2"] },
    2: { question: "2x - 3 > 5. What is the solution?", answers: ["x > 4"] },
    3: { question: "f(x) = x² + 1. Find f(3)", answers: ["10"] }
  };

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
    appendToTerminal("Welcome to FunStep!");
    appendToTerminal("Enter the card number and your answer separated by a space.");
    appendToTerminal("Example: 1 -0.5");
  }

  function checkAnswer(input) {
    const parts = input.trim().split(/\s+/);
    if (parts.length < 2) {
      appendToTerminal("⚠ Please enter both card number and answer.");
      return;
    }

    const cardNum = parseInt(parts[0]);
    const answer = parts.slice(1).join(" ").toLowerCase();

    if (!correctAnswers[cardNum]) {
      appendToTerminal(`❌ Card ${cardNum} not found.`);
      return;
    }

    const validAnswers = correctAnswers[cardNum].answers.map(a => a.toLowerCase());
    appendToTerminal(`📜 Q: ${correctAnswers[cardNum].question}`);
    
    if (validAnswers.includes(answer)) {
      appendToTerminal("✅ Correct!");
    } else {
      appendToTerminal("❌ Wrong.");
    }
  }

  function sendInput() {
    const value = termInput.value.trim();
    if (value) {
      appendToTerminal("> " + value);
      checkAnswer(value);
      termInput.value = "";
    }
  }

  termInput.addEventListener("keydown", e => {
    if (e.key === "Enter") sendInput();
  });

  printWelcome();
</script>

</body>
</html>
