<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>FunStep: a Staircase-Themed Puzzle for Learning Rational Functions</title>
<style>
  body {
    margin: 0;
    font-family: Arial, sans-serif;
    background-color: #0d0d0d;
    color: #00ffcc;
    display: flex;
    flex-direction: column;
    height: 100vh;
    font-size: 14px;
  }
  header {
    text-align: center;
    padding: 0.8rem;
    font-size: 0.9rem;
    background-color: #111;
    color: #ff00ff;
    text-shadow: 0 0 6px #ff00ff;
  }
  main {
    flex: 1;
    display: flex;
    flex-direction: column;
    padding: 0.5rem;
    gap: 0.5rem;
  }
  .terminal {
    flex: 1;
    background: #000;
    border: 2px solid #00ffcc;
    padding: 0.6rem;
    overflow-y: auto;
    box-shadow: 0 0 8px #00ffcc;
    font-size: 0.8rem;
    line-height: 1.4;
  }
  .term-input {
    display: flex;
    gap: 0.4rem;
  }
  .term-input input {
    flex: 1;
    background: black;
    color: #fff;
    border: 2px solid #ff00ff;
    padding: 0.6rem;
    font-size: 0.8rem;
  }
  .term-input button {
    background: black;
    color: #fff;
    border: 2px solid #ff00ff;
    padding: 0.6rem;
    cursor: pointer;
    font-size: 0.8rem;
  }
  .term-input button:hover {
    background: #ff00ff;
    color: black;
  }
</style>
</head>
<body>
  <header>🎮 FunStep: a Staircase-Themed Puzzle for Learning Rational Functions 🎮</header>
  <main>
    <div class="terminal" id="terminal"></div>
    <div class="term-input">
      <input id="termInput" placeholder="Type card number and answer (e.g. 1 -0.5)" autocomplete="off" />
      <button onclick="sendCommand()">OK</button>
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

  function appendLine(text) {
    const div = document.createElement("div");
    div.textContent = text;
    terminal.appendChild(div);
    terminal.scrollTop = terminal.scrollHeight;
  }

  function printWelcome() {
    appendLine("Welcome to FunStep!");
    appendLine("Enter the card number and your answer separated by a space.");
    appendLine("Example: 1 -0.5");
  }

  function checkCardAndAnswer(input) {
    const parts = input.trim().split(/\s+/);
    if (parts.length < 2) {
      appendLine("⚠ Please type card number and answer, e.g. '1 -0.5'");
      return;
    }
    const cardNum = parseInt(parts[0]);
    const answer = parts.slice(1).join(" ").toLowerCase();

    if (!correctAnswers[cardNum]) {
      appendLine(`❌ Card ${cardNum} not found.`);
      return;
    }
    const validAnswers = correctAnswers[cardNum].answers.map(a => a.toLowerCase());
    appendLine(`📜 Card ${cardNum}: ${correctAnswers[cardNum].question}`);
    appendLine(validAnswers.includes(answer) ? "✅ Correct!" : "❌ Wrong.");
  }

  function sendCommand() {
    const value = termInput.value.trim();
    if (value) {
      appendLine("> " + value);
      checkCardAndAnswer(value);
      termInput.value = "";
    }
  }

  termInput.addEventListener("keydown", e => {
    if (e.key === "Enter") sendCommand();
  });

  printWelcome();
</script>
</body>
</html>
