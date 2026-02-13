<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Valentine Game for Y.S.J</title>

<link href="https://fonts.googleapis.com/css2?family=DotGothic16&family=Jacquarda+Bastarda+9&family=Great+Vibes&family=Single+Day&display=swap" rel="stylesheet">

<style>
  :root {
    --bg-pink: #E89BB1;
    --dark-red: #7A0F14;
    --kisses-brown: #5C3A21;
  }

  body {
    margin: 0;
    background-color: var(--bg-pink);
    color: var(--dark-red);
    font-family: 'Single Day', cursive;
    display: flex;
    flex-direction: column;
    align-items: center;
    overflow-x: hidden;
  }

  /* --- 헤더 영역 (Great Vibes & DotGothic16) --- */
  header {
    width: 100%;
    background-color: var(--dark-red);
    padding: 25px 0;
    box-shadow: 0 5px 15px rgba(0,0,0,0.2);
  }

  .header-box {
    background-color: var(--bg-pink);
    margin: 0 8%;
    padding: 15px;
    border-radius: 20px;
    border: 3px solid rgba(255,255,255,0.5);
    position: relative;
    text-align: center;
  }

  .header-title {
    font-family: 'DotGothic16', sans-serif; /* 메인 도트 폰트 */
    font-size: 28px;
    margin: 0;
  }

  .sub-text {
    font-family: 'Great Vibes', cursive; /* 기타 영어 폰트 */
    font-size: 20px;
    display: flex;
    justify-content: space-between;
    padding: 0 20px;
    margin-top: 5px;
  }

  /* --- 메인 키세스 로고 디테일 --- */
  .hero { margin-top: 60px; text-align: center; }

  .kisses-wrap {
    position: relative;
    width: 300px;
    height: 250px;
    margin: 0 auto;
  }

  /* 키세스 몸통 */
  .kisses-main {
    width: 100%;
    height: 100%;
    background-color: var(--dark-red);
    clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
    border-radius: 0 0 50px 50px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    box-shadow: inset 0 -10px 20px rgba(0,0,0,0.3);
  }

  /* 키세스 안 글씨 (Jacquarda Bastarda 9) */
  .kisses-text {
    font-family: 'Jacquarda Bastarda 9', serif;
    color: white;
    font-size: 32px;
    line-height: 1.2;
    text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
    z-index: 2;
  }

  /* 키세스 띠지 (Tag) 디테일 */
  .kisses-tag {
    position: absolute;
    top: 5px;
    right: 20px;
    background: white;
    padding: 4px 15px;
    font-family: 'Great Vibes', cursive;
    font-size: 14px;
    color: var(--dark-red);
    transform: rotate(25deg);
    border-left: 2px solid #ddd;
    box-shadow: 3px 3px 8px rgba(0,0,0,0.2);
    z-index: 5;
    white-space: nowrap;
  }

  .kisses-tag::before {
    content: "";
    position: absolute;
    left: -10px;
    top: 50%;
    width: 10px;
    height: 2px;
    background: #ccc;
  }

  /* --- 버튼 & 게임 --- */
  .start-btn {
    margin-top: 40px;
    padding: 15px 50px;
    font-family: 'DotGothic16', sans-serif;
    font-size: 22px;
    background-color: var(--dark-red);
    color: white;
    border: 3px solid white;
    border-radius: 50px;
    cursor: pointer;
    transition: 0.3s;
  }

  .start-btn:hover { background: #5a0b0f; transform: scale(1.05); }

  #game-area {
    display: none;
    width: 90vw;
    max-width: 600px;
    height: 450px;
    background: rgba(255,255,255,0.2);
    border: 4px dashed var(--dark-red);
    margin: 20px;
    position: relative;
    border-radius: 30px;
    overflow: hidden;
  }

  .choco-item {
    position: absolute;
    width: 45px;
    height: 45px;
    background-color: var(--kisses-brown);
    clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
    cursor: pointer;
  }
</style>
</head>
<body>

<header>
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
    <div class="kisses-main">
      <div class="kisses-text">Chocolate<br>Catch Game</div>
    </div>
  </div>
  <p style="font-size: 18px; margin-top: 30px;">초콜릿을 10개 모으면 특별한 보상을 받을 수 있어요.</p>
  <button class="start-btn" onclick="startGame()">Start Game</button>
</div>

<div id="game-area"></div>

<div id="reward-modal" style="display:none; position:fixed; inset:0; background:rgba(0,0,0,0.8); z-index:1000; justify-content:center; align-items:center;">
  <div style="background:white; padding:50px; border-radius:30px; text-align:center; border: 5px solid var(--dark-red);">
    <h2 style="font-family: 'Great Vibes', cursive; font-size: 40px;">Mission Complete!</h2>
    <p style="font-size: 20px;">성준님 전용 <b>'달콤한 데이트권'</b> 획득 💌</p>
    <button class="start-btn" onclick="location.reload()">닫기</button>
  </div>
</div>

<script>
  let score = 0;
  function startGame() {
    document.getElementById('intro').style.display = 'none';
    const area = document.getElementById('game-area');
    area.style.display = 'block';
    
    const gameTimer = setInterval(() => {
      const choco = document.createElement('div');
      choco.className = 'choco-item';
      choco.style.left = Math.random() * (area.clientWidth - 50) + 'px';
      choco.style.top = '-50px';
      area.appendChild(choco);

      let pos = -50;
      const fall = setInterval(() => {
        pos += 4;
        choco.style.top = pos + 'px';
        if (pos > area.clientHeight) {
          clearInterval(fall);
          choco.remove();
        }
      }, 20);

      choco.onclick = () => {
        score++;
        choco.remove();
        if (score >= 10) {
          clearInterval(gameTimer);
          document.getElementById('reward-modal').style.display = 'flex';
        }
      };
    }, 900);
  }
</script>

</body>
</html>
