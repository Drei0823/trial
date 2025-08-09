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
    padding: 1rem;
    font-size: 0.9rem;
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
    font-size: 0.65rem;
  }
  .term-input button {
    background: black;
    color: #fff;
    border: 2px solid #ff00ff;
    padding: 0.8rem;
    cursor: pointer;
    text-shadow: 0 0 5px #ff00ff;
    font-family: 'Press Start 2P', cursive;
    font-size: 0.65rem;
  }
  .term-input button:hover {
    background: #ff00ff;
    color: black;
    box-shadow: 0 0 10px #ff00ff, 0 0 20px
