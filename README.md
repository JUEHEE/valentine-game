<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Chocolate Catch Game</title>

<style>
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background-color: #f3a6b3;
  text-align: center;
  overflow-x: hidden;
}

header {
  background: #7a0f14;
  padding: 20px;
  color: white;
  font-size: 28px;
  font-weight: bold;
}

.hero {
  padding: 60px 20px;
}

button {
  padding: 12px 25px;
  font-size: 16px;
  border-radius: 30px;
  border: none;
  background: #7a0f14;
  color: white;
  cursor: pointer;
}

#game-section {
  display: none;
  margin-top: 40px;
}

#game-area {
  width: 600px;
  height: 400px;
  background: white;
  margin: 0 auto;
  position: relative;
  border-radius: 20px;
  box-shadow: 0 10px 20px rgba(0,0,0,0.2);
  overflow: hidden;
}

.chocolate {
  width: 40px;
  height: 40px;
  background: #5C3A21;
  border-radius: 50%;
  position: absolute;
  cursor: pointer;
}

#scoreboard {
  margin: 20px;
  font-size: 18px;
  font-weight: bold;
}

#reward-modal {
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.6);
  justify-content: center;
  align-items: center;
}

.modal-content {
  background: white;
  padding: 40px;
  border-radius: 20px;
}

.heart {
  position: fixed;
  font-size: 20px;
  animation: fall 3s linear forwards;
}

@keyframes fall {
  0% { transform: translateY(-10px); opacity: 1; }
  100% { transform: translateY(100vh); opacity: 0; }
}
</style>
</head>

<body>

<header>Happy Valentine's Day 💗</header>

<div class="hero">
  <h1>🍫 Chocolate Catch Game 🍫</h1>
  <p>초콜릿을 10개 모으면 특별한 보상을 받을 수 있어요.</p>
  <button onclick="startGame()">Start Game</button>
</div>

<div id="game-section">
  <div id="scoreboard">
    Score: <span id="score">0</span> / 10
  </div>
  <div id="game-area"></div>
</div>

<div id="reward-modal">
  <div class="modal-content">
    <h2>🎉 Mission Complete!</h2>
    <p>데이트권 1회 획득 💌</p>
    <button onclick="closeModal()">닫기</button>
  </div>
</div>

<script>
let score = 0;
let gameInterval;

function startGame() {
  document.getElementById("game-section").style.display = "block";
  spawnChocolate();
  gameInterval = setInterval(spawnChocolate, 1000);
}

function spawnChocolate() {
  const gameArea = document.getElementById("game-area");
  const chocolate = document.createElement("div");
  chocolate.classList.add("chocolate");

  chocolate.style.top = Math.random() * 360 + "px";
  chocolate.style.left = Math.random() * 560 + "px";

  chocolate.onclick = function() {
    score++;
    document.getElementById("score").innerText = score;
    chocolate.remove();

    if (score >= 10) {
      clearInterval(gameInterval);
      document.getElementById("reward-modal").style.display = "flex";
      launchHearts();
    }
  };

  gameArea.appendChild(chocolate);

  setTimeout(() => {
    chocolate.remove();
  }, 1500);
}

function closeModal() {
  document.getElementById("reward-modal").style.display = "none";
}

function launchHearts() {
  for (let i = 0; i < 40; i++) {
    const heart = document.createElement("div");
    heart.className = "heart";
    heart.innerHTML = "💖";
    heart.style.left = Math.random() * window.innerWidth + "px";
    heart.style.top = "-20px";
    heart.style.fontSize = (Math.random() * 30 + 20) + "px";
    document.body.appendChild(heart);

    setTimeout(() => {
      heart.remove();
    }, 3000);
  }
}
