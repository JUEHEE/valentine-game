<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>To. Y.S.J - Chocolate Catch Game</title>

<style>
  /* 1. 디자인 이미지의 색감 반영 */
  body {
    margin: 0;
    font-family: 'Courier New', Courier, monospace; /* 픽셀 느낌을 위한 고정폭 글꼴 */
    background-color: #f3a6b3; /* 메인 핑크 */
    text-align: center;
    overflow-x: hidden;
    color: #7a0f14;
  }

  header {
    background: #7a0f14;
    padding: 15px;
    color: #f3a6b3;
    font-size: 24px;
    font-weight: bold;
    border-bottom: 4px dashed #f3a6b3;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 40px;
  }

  .hero {
    padding: 40px 20px;
  }

  /* 2. 키세스 초콜릿을 형상화한 타이틀 버튼 */
  .main-logo {
    width: 200px;
    height: 180px;
    background-color: #7a0f14;
    clip-path: polygon(50% 0%, 0% 100%, 100% 100%); /* 삼각형(키세스) 모양 */
    margin: 0 auto 20px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    color: #f3a6b3;
    position: relative;
    border-radius: 0 0 20px 20px;
    box-shadow: 0 10px 20px rgba(0,0,0,0.1);
  }

  /* 키세스 종이 띠 재현 */
  .main-logo::after {
    content: "To. Y.S.J";
    position: absolute;
    top: -10px;
    right: 30px;
    background: white;
    color: #7a0f14;
    padding: 2px 10px;
    font-size: 12px;
    transform: rotate(15deg);
    box-shadow: 2px 2px 5px rgba(0,0,0,0.1);
  }

  h1 { font-size: 1.5rem; margin-top: 50px; }

  button {
    padding: 15px 40px;
    font-size: 18px;
    font-weight: bold;
    border-radius: 50px;
    border: 3px solid #7a0f14;
    background: white;
    color: #7a0f14;
    cursor: pointer;
    transition: 0.3s;
  }

  button:hover {
    background: #7a0f14;
    color: #f3a6b3;
  }

  /* 3. 게임 영역 디자인 */
  #game-area {
    width: 90%;
    max-width: 600px;
    height: 400px;
    background: rgba(255, 255, 255, 0.5);
    margin: 20px auto;
    position: relative;
    border: 4px solid #7a0f14;
    border-radius: 20px;
    overflow: hidden;
    cursor: crosshair;
  }

  /* 진짜 키세스 모양 초콜릿 */
  .chocolate {
    width: 50px;
    height: 50px;
    background: #5C3A21;
    position: absolute;
    cursor: pointer;
    clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
    border-radius: 0 0 10px 10px;
    transition: transform 0.1s;
  }

  .chocolate:hover { transform: scale(1.2); }

  /* 4. 보상 모달창 */
  .modal-content {
    background: #fff;
    border: 10px double #7a0f14;
    padding: 40px;
    border-radius: 0; /* 레트로 느낌 */
  }

  .heart { position: fixed; animation: fall 3s linear forwards; z-index: 100; }
  @keyframes fall {
    0% { transform: translateY(-10px) rotate(0deg); opacity: 1; }
    100% { transform: translateY(100vh) rotate(360deg); opacity: 0; }
  }
</style>
</head>

<body>

<header>
  <span>Valentine Event 2026</span>
  <span>to Y.S.J 💗</span>
</header>

<div class="hero" id="intro">
  <div class="main-logo">
    <div style="font-size: 14px;">Chocolate</div>
    <div style="font-size: 14px;">Catch Game</div>
  </div>
  <p>성준님을 위한 초콜릿 10개를 모아주세요!</p>
  <button onclick="startGame()">START</button>
</div>

<div id="game-section" style="display:none;">
  <div id="scoreboard" style="font-size: 24px; margin: 20px;">
    Collected: <span id="score">0</span> / 10
  </div>
  <div id="game-area"></div>
</div>

<div id="reward-modal" style="display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(122,15,20,0.8); justify-content:center; align-items:center; z-index:1000;">
  <div class="modal-content">
    <h2 style="color:#7a0f14;">🎉 MISSION COMPLETE!</h2>
    <p style="font-size: 20px;">양성준 전용 <b>'달콤한 데이트권'</b> 획득! 💌</p>
    <p>지금 바로 스크린샷을 찍어 전송하세요!</p>
    <button onclick="location.reload()">다시 하기</button>
  </div>
</div>

<script>
let score = 0;
let gameInterval;

function startGame() {
  document.getElementById("intro").style.display = "none";
  document.getElementById("game-section").style.display = "block";
  spawnChocolate();
  gameInterval = setInterval(spawnChocolate, 900); // 0.9초마다 생성
}

function spawnChocolate() {
  const gameArea = document.getElementById("game-area");
  const chocolate = document.createElement("div");
  chocolate.classList.add("chocolate");

  // 게임 영역 안에서 랜덤 위치
  const x = Math.random() * (gameArea.clientWidth - 50);
  const y = Math.random() * (gameArea.clientHeight - 50);

  chocolate.style.left = x + "px";
  chocolate.style.top = y + "px";

  chocolate.onclick = function() {
    score++;
    document.getElementById("score").innerText = score;
    this.remove();

    if (score >= 10) {
      clearInterval(gameInterval);
      document.getElementById("reward-modal").style.display = "flex";
      launchHearts();
    }
  };

  gameArea.appendChild(chocolate);

  // 1.2초 뒤에 안 눌러도 사라짐
  setTimeout(() => { if(chocolate) chocolate.remove(); }, 1200);
}

function launchHearts() {
  const colors = ['💖', '💝', '🍫', '✨'];
  for (let i = 0; i < 50; i++) {
    const heart = document.createElement("div");
    heart.className = "heart";
    heart.innerHTML = colors[Math.floor(Math.random() * colors.length)];
    heart.style.left = Math.random() * window.innerWidth + "px";
    heart.style.fontSize = (Math.random() * 30 + 20) + "px";
    heart.style.animationDuration = (Math.random() * 2 + 2) + "s";
    document.body.appendChild(heart);
  }
}
</script>
</body>
</html>
