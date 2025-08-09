<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
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
    font-size: 0.85rem;
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
    font-size: 0.85rem;
  }
  .term-input button {
    background: black;
    color: #fff;
    border: 2px solid #ff00ff;
    padding: 0.6rem;
    cursor: pointer;
    font-size: 0.85rem;
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
      <input id="termInput" placeholder="Type card number first" autocomplete="off">
      <button onclick="sendCommand()">OK</button>
    </div>
  </main>

<script>
  const correctAnswers = {
    1: { question: "1/(x+1) = 2. What is x?", answers: ["-0.5", "-1/2"] },
    2: { question: "2x - 3 > 5. What is the solution?", answers: ["x > 4"] },
    3: { question: "f(x) = x² + 1. Find f(3)", answers: ["10"] }
  };

  let currentCard = null;
  let waitingForAnswer = false;

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
    appendLine("Step 1: Type the card number to see the question.");
    appendLine("Step 2: Type the answer.");
  }

  function handleCardNumber(input) {
    const num = parseInt(input);
    if (!correctAnswers[num]) {
      appendLine(`❌ Card ${num} not found.`);
      return;
    }
    currentCard = num;
    waitingForAnswer = true;
    appendLine(`📜 Card ${num}: ${correctAnswers[num].question}`);
    appendLine("Now type your answer:");
  }

  function handleAnswer(input) {
    const answer = input.toLowerCase();
    const validAnswers = correctAnswers[currentCard].answers.map(a => a.toLowerCase());
    if (validAnswers.includes(answer)) {
      appendLine("✅ Correct!");
    } else {
      appendLine("❌ Wrong.");
    }
    waitingForAnswer = false;
    currentCard = null;
    appendLine("Type another card number to continue.");
  }

  function sendCommand() {
    const value = termInput.value.trim();
    if (!value) return;
    appendLine("> " + value);

    if (!waitingForAnswer) {
      handleCardNumber(value);
    } else {
      handleAnswer(value);
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
