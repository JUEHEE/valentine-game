<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>To. Y.S.J - Valentine Mission</title>

<link href="https://fonts.googleapis.com/css2?family=DotGothic16&family=Jacquarda+Bastarda+9&family=Great+Vibes&family=Single+Day&display=swap" rel="stylesheet">

<style>
  :root {
    --bg-pink: #E89BB1;
    --dark-red: #7A0F14;
    --kisses-brown: #5C3A21;
    --dot-color: #f0c3cb;
    --container-bg: rgba(255, 255, 255, 0.9);
  }

  body {
    margin: 0;
    background-color: var(--bg-pink);
    background-image: radial-gradient(var(--dot-color) 1.5px, transparent 1.5px);
    background-size: 25px 25px;
    color: var(--dark-red);
    font-family: 'Single Day', cursive;
    display: flex;
    flex-direction: column;
    align-items: center;
    min-height: 100vh;
    padding: 20px;
    box-sizing: border-box;
    overflow: hidden; /* 게임 중 스크롤 방지 */
  }

  /* --- 헤더 및 대칭 하트 --- */
  header {
    width: 100%;
    max-width: 500px;
    position: relative;
    margin-bottom: 60px;
    margin-top: 20px;
  }

  .header-box {
    background-color: white;
    padding: 20px;
    border-radius: 20px;
    border: 3px solid white;
    text-align: center;
    box-shadow: 0 8px 20px rgba(0,0,0,0.15);
    position: relative;
    z-index: 10;
  }

  .header-title {
    font-family: 'DotGothic16', sans-serif;
    font-size: 26px;
    margin: 0 0 10px 0;
    border-bottom: 2px solid var(--dark-red);
    padding-bottom: 5px;
  }

  /* 하트 쉐이프 (대칭 수정) */
  .blender-heart {
    position: absolute;
    width: 50px;
    height: 50px;
    background-color: var(--dark-red);
    z-index: 1;
    filter: drop-shadow(0 10px 15px rgba(0,0,0,0.4));
  }
  .blender-heart::before, .blender-heart::after {
    content: "";
    position: absolute;
    width: 50px;
    height: 50px;
    background-color: var(--dark-red);
    border-radius: 50%;
  }
  .blender-heart::before { top: -25px; left: 0; }
  .blender-heart::after { left: 25px; top: 0; }

  .header-heart-left { 
    left: -20px; top: -30px; 
    transform: rotate(-15deg) scale(0.7); 
  }
  .header-heart-right { 
    right: -20px; top: -30px; 
    transform: rotate(15deg) scale(0.7) scaleX(1); /* 대칭 유지 */
  }

  /* --- 메인 키세스 영역 --- */
  .hero { 
    background: var(--container-bg);
    padding: 40px;
    border-radius: 40px;
    box-shadow: 0 15px 35px rgba(0,0,0,0.1);
    max-width: 450px;
    width: 100%;
    text-align: center;
  }

  .kisses-wrap {
    position: relative;
    width: 280px;
    height: 220px;
    margin: 0 auto 30px;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .kisses-main {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: var(--dark-red);
    clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
    z-index: 1;
  }

  .kisses-text-overlay {
    position: relative;
    z-index: 10;
    margin-top: 90px;
    color: white;
    font-family: 'DotGothic16', sans-serif;
    font-size: 24px;
    line-height: 1.4;
  }

  .kisses-tag {
    position: absolute;
    top: 15%;
    right: -10px;
    background: white;
    padding: 8px 15px;
    font-family: 'Great Vibes', cursive;
    font-size: 18px;
    color: var(--dark-red);
    transform: rotate(15deg);
    box-shadow: 5px 5px 15px rgba(0,0,0,0.2);
    z-index: 15;
  }

  /* --- 버튼 스타일 --- */
  .btn {
    padding: 15px 40px;
    font-family: 'DotGothic16', sans-serif;
    font-size: 20px;
    color: white;
    border: none;
    border-radius: 50px;
    cursor: pointer;
    text-decoration: none;
    display: inline-block;
    transition: 0.1s;
    margin: 10px 5px;
  }
  
  .start-btn { background: var(--dark-red); box-shadow: 0 6px 0 #4a090c; }
  .mail-btn { background: #4285F4; box-shadow: 0 6px 0 #2a56a0; }
  .btn:active { transform: translateY(4px); box-shadow: none; }

  /* --- 게임 영역 --- */
  #game-container-wrap { display: none; width: 100%; max-width: 500px; }
  #game-area {
    width: 100%; height: 400px; background: #fff;
    border: 4px dashed var(--dark-red); position: relative;
    border-radius: 25px; overflow: hidden;
  }
  .choco-item {
    position: absolute; width: 50px; height: 50px;
    background-color: var(--kisses-brown); clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
  }

  /* --- 모달 --- */
  #reward-modal {
    display: none; position: fixed; inset: 0;
    background: rgba(0,0,0,0.9); z-index: 9999;
    flex-direction: column; justify-content: center; align-items: center;
  }
  .modal-content {
    background: white; padding: 40px; border-radius: 40px;
    text-align: center; border: 6px solid var(--dark-red); width: 85%; max-width: 400px;
  }

  #confetti-canvas {
    position: fixed; top: 0; left: 0; width: 100%; height: 100%;
    pointer-events: none; z-index: 10000;
  }
</style>
</head>
<body>

<canvas id="confetti-canvas"></canvas>

<header>
  <div class="blender-heart header-heart-left"></div>
  <div class="blender-heart header-heart-right"></div>
  <div class="header-box">
    <h1 class="header-title">Happy Valentine's Day</h1>
    <div class="sub-text">
      <span>Valentine Event 2026</span>
      <span>to Y.S.J</span>
    </div>
  </div>
</header>

<div class="hero" id="intro">
  <div class="kisses-wrap">
    <div class="kisses-tag">Happy Valentine to YSJ</div>
    <div class="kisses-main"></div>
    <div class="kisses-text-overlay">Chocolate<br>Catch Game</div>
  </div>
  <p style="font-size: 19px;">성준님! 초콜릿 10개를 모아<br>특별한 <b>데이트권</b>을 획득하세요!</p>
  <button class="btn start-btn" onclick="startGame()">START GAME</button>
</div>

<div id="game-container-wrap">
  <div style="background:white; padding:15px; border-radius:20px; border:3px solid var(--dark-red); margin-bottom:20px; font-family:'DotGothic16'; font-size:22px; text-align:center;">
    Score: <span id="score">0</span> / 10
  </div>
  <div id="game-area"></div>
</div>

<div id="reward-modal">
  <div class="modal-content">
    <h2 style="font-family: 'Great Vibes', cursive; font-size: 40px; margin:0 0 10px 0; color: var(--dark-red);">Mission Complete!</h2>
    <p style="font-size: 18px; line-height: 1.6; margin-bottom: 25px;">
      축하합니다! 데이트권을 획득하셨어요.<br>
      <span style="color: #d63031; font-weight: bold;">지금 이 화면을 캡쳐해서</span><br>
      아래 버튼을 눌러 인증샷을 보내주세요! ❤️
    </p>
    
    <a href="mailto:juehee0110@gmail.com?subject=[인증] 발렌타인 데이트권 미션 성공!&body=성준님! 미션 성공을 축하드립니다. 캡쳐한 이미지를 첨부해서 답장해주세요!" class="btn mail-btn">
      인증샷 보내기
    </a>
    <br>
    <button class="btn" style="background:#888; box-shadow: 0 6px 0 #555; font-size: 16px;" onclick="location.reload()">다시 하기</button>
  </div>
</div>

<script>
  let score = 0;
  const canvas = document.getElementById('confetti-canvas');
  const ctx = canvas.getContext('2d');
  let particles = [];
  let animationId;

  function resizeCanvas() {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
  }
  window.addEventListener('resize', resizeCanvas);
  resizeCanvas();

  class Heart {
    constructor() {
      this.x = canvas.width / 2;
      this.y = canvas.height / 2;
      this.size = Math.random() * 15 + 10;
      this.speedX = (Math.random() - 0.5) * 20;
      this.speedY = (Math.random() - 0.5) * 20 - 5;
      this.gravity = 0.25;
      this.opacity = 1;
      this.color = `hsl(${Math.random() * 30 + 330}, 80%, 60%)`;
    }
    draw() {
      ctx.save();
      ctx.globalAlpha = this.opacity;
      ctx.fillStyle = this.color;
      ctx.beginPath();
      let topCurveHeight = this.size * 0.3;
      ctx.moveTo(this.x, this.y + topCurveHeight);
      ctx.bezierCurveTo(this.x, this.y, this.x - this.size / 2, this.y, this.x - this.size / 2, this.y + topCurveHeight);
      ctx.bezierCurveTo(this.x - this.size / 2, this.y + (this.size + topCurveHeight) / 2, this.x, this.y + (this.size + topCurveHeight) / 2, this.x, this.y + this.size);
      ctx.bezierCurveTo(this.x, this.y + (this.size + topCurveHeight) / 2, this.x + this.size / 2, this.y + (this.size + topCurveHeight) / 2, this.x + this.size / 2, this.y + topCurveHeight);
      ctx.bezierCurveTo(this.x + this.size / 2, this.y, this.x, this.y, this.x, this.y + topCurveHeight);
      ctx.fill();
      ctx.restore();
    }
    update() {
      this.speedY += this.gravity;
      this.x += this.speedX;
      this.y += this.speedY;
      this.opacity -= 0.01;
    }
  }

  function animate() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    particles.forEach((p, i) => {
      p.update();
      p.draw();
      if (p.opacity <= 0) particles.splice(i, 1);
    });
    animationId = requestAnimationFrame(animate);
  }

  function burst() {
    for (let i = 0; i < 100; i++) particles.push(new Heart());
    if (!animationId) animate();
  }

  function startGame() {
    document.getElementById('intro').style.display = 'none';
    document.getElementById('game-container-wrap').style.display = 'block';
    const area = document.getElementById('game-area');
    
    const gameTimer = setInterval(() => {
      const choco = document.createElement('div');
      choco.className = 'choco-item';
      choco.style.left = Math.random() * (area.clientWidth - 50) + 'px';
      choco.style.top = '-60px';
      area.appendChild(choco);

      let pos = -60;
      const fall = setInterval(() => {
        pos += 2.2;
        choco.style.top = pos + 'px';
        if (pos > area.clientHeight) {
          clearInterval(fall);
          if(choco.parentNode) choco.remove();
        }
      }, 20);

      choco.onclick = () => {
        score++;
        document.getElementById('score').innerText = score;
        choco.remove();
        if (score >= 10) {
          clearInterval(gameTimer);
          document.getElementById('reward-modal').style.display = 'flex';
          burst();
          setTimeout(burst, 400);
          setTimeout(burst, 800);
        }
      };
    }, 1100);
  }
</script>
</body>
</html>
