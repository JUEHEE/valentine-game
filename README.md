<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>To. Y.S.J - Sweet Catch Game</title>

<link href="https://fonts.googleapis.com/css2?family=DotGothic16&family=Jacquarda+Bastarda+9&family=Great+Vibes&family=Single+Day&display=swap" rel="stylesheet">

<style>
  :root {
    --bg-pink: #E89BB1;
    --dark-red: #7A0F14;
    --kisses-brown: #5C3A21;
    --dot-color: #f0c3cb;
    --container-bg: rgba(255, 255, 255, 0.85); /* 시안처럼 약간 더 불투명하게 */
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
  }

  /* --- 상단 헤더 영역 --- */
  header {
    width: 100%;
    max-width: 500px;
    position: relative;
    margin-bottom: 40px;
  }

  .header-box {
    background-color: white; /* 시안처럼 깔끔한 흰색 배경 */
    padding: 20px;
    border-radius: 20px;
    border: 3px solid white;
    text-align: center;
    box-shadow: 0 8px 20px rgba(0,0,0,0.15);
    position: relative;
    z-index: 2;
  }

  .header-title {
    font-family: 'DotGothic16', sans-serif;
    font-size: 26px;
    margin: 0 0 10px 0;
    border-bottom: 2px solid var(--dark-red); /* 시안의 구분선 추가 */
    padding-bottom: 5px;
  }

  .sub-text {
    font-family: 'Great Vibes', cursive;
    font-size: 22px;
    display: flex;
    justify-content: space-between;
    margin: 0;
  }

  /* 블렌더 처리된 하트 (깨짐 방지 수정) */
  .blender-heart {
    position: absolute;
    width: 80px;
    height: 70px;
    background-color: var(--dark-red);
    transform: rotate(-45deg);
    border-radius: 50% 50% 0 50%;
    box-shadow: 0 10px 20px rgba(0,0,0,0.4);
    z-index: 1;
    opacity: 0.9;
  }
  .blender-heart::after {
    content: "";
    position: absolute;
    width: 80px;
    height: 80px;
    background-color: var(--dark-red);
    border-radius: 50%;
    top: 0;
    left: 40px;
  }

  .header-heart-left { left: -30px; top: -20px; }
  .header-heart-right { right: -30px; top: -20px; transform: rotate(-45deg) scaleX(-1); }

  /* --- 메인 키세스 영역 --- */
  .hero { 
    background: var(--container-bg);
    padding: 40px;
    border-radius: 40px;
    box-shadow: 0 15px 35px rgba(0,0,0,0.1);
    max-width: 450px;
    width: 100%;
    text-align: center;
    position: relative;
  }

  .kisses-wrap {
    position: relative;
    width: 280px;
    height: 220px;
    margin: 0 auto 20px;
  }

  .kisses-main {
    width: 100%;
    height: 100%;
    background-color: var(--dark-red);
    clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
    display: flex;
    justify-content: center;
    align-items: flex-end; /* 텍스트 위치 하단 고정 */
    padding-bottom: 30px;
    box-sizing: border-box;
  }

  .kisses-text {
    font-family: 'DotGothic16', sans-serif; /* 시안처럼 도트체로 변경 */
    color: white;
    font-size: 24px;
    line-height: 1.4;
    z-index: 3;
  }

  .kisses-tag {
    position: absolute;
    top: 20%;
    right: -10px;
    background: white;
    padding: 8px 15px;
    font-family: 'Great Vibes', cursive;
    font-size: 18px;
    color: var(--dark-red);
    transform: rotate(15deg);
    box-shadow: 5px 5px 15px rgba(0,0,0,0.2);
    z-index: 5;
    border-radius: 2px;
  }

  /* --- 게임 실행 화면 --- */
  #game-container-wrap {
    display: none;
    background: var(--container-bg);
    padding: 30px;
    border-radius: 40px;
    max-width: 500px;
    width: 100%;
    box-shadow: 0 15px 35px rgba(0,0,0,0.1);
  }

  #score-board {
    font-family: 'DotGothic16', sans-serif;
    font-size: 22px;
    background: white;
    padding: 15px;
    border-radius: 20px;
    margin-bottom: 20px;
    border: 3px solid var(--dark-red);
    display: flex;
    justify-content: space-around;
  }

  #game-area {
    width: 100%;
    height: 400px;
    background: #fff;
    border: 4px dashed var(--dark-red);
    position: relative;
    border-radius: 25px;
    overflow: hidden;
  }

  .choco-item {
    position: absolute;
    width: 50px;
    height: 50px;
    background-color: var(--kisses-brown);
    clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
    cursor: pointer;
    filter: drop-shadow(0 4px 4px rgba(0,0,0,0.3));
  }

  .start-btn {
    padding: 15px 50px;
    font-family: 'DotGothic16', sans-serif;
    font-size: 22px;
    background: var(--dark-red);
    color: white;
    border: none;
    border-radius: 50px;
    cursor: pointer;
    box-shadow: 0 6px 0 #4a090c;
    transition: 0.1s;
  }
  .start-btn:active { transform: translateY(4px); box-shadow: none; }

</style>
</head>
<body>

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
    <div class="kisses-main">
      <div class="kisses-text">Chocolate<br>Catch Game</div>
    </div>
  </div>
  <p style="font-size: 20px; margin: 20px 0;">초콜릿 10개를 모으면<br>특별한 보상을 받을 수 있어요.</p>
  <button class="start-btn" onclick="startGame()">START GAME</button>
</div>

<div id="game-container-wrap">
  <div id="score-board">
    <span>Score: <span id="score">0</span></span>
    <span>Goal: 10</span>
  </div>
  <div id="game-area"></div>
</div>

<div id="reward-modal" style="display:none; position:fixed; inset:0; background:rgba(0,0,0,0.85); z-index:9999; justify-content:center; align-items:center;">
  <div style="background:white; padding:50px; border-radius:40px; text-align:center; border: 6px solid var(--dark-red); max-width: 85%;">
    <h2 style="font-family: 'Great Vibes', cursive; font-size: 45px; margin:0 0 20px 0;">Mission Complete!</h2>
    <p style="font-size: 22px; margin-bottom: 30px;">성준님 전용 <b>'달콤한 데이트권'</b> 획득! 💌</p>
    <button class="start-btn" onclick="location.reload()">REPLAY</button>
  </div>
</div>

<script>
  let score = 0;
  let gameInterval;

  function startGame() {
    document.getElementById('intro').style.display = 'none';
    document.getElementById('game-container-wrap').style.display = 'block';
    const area = document.getElementById('game-area');
    
    gameInterval = setInterval(() => {
      const choco = document.createElement('div');
      choco.className = 'choco-item';
      choco.style.left = Math.random() * (area.clientWidth - 50) + 'px';
      choco.style.top = '-60px';
      area.appendChild(choco);

      let pos = -60;
      const fallInterval = setInterval(() => {
        pos += 2.0; // 시안에 맞춰 속도 더 하향
        choco.style.top = pos + 'px';
        
        if (pos > area.clientHeight) {
          clearInterval(fallInterval);
          if(choco.parentNode) choco.remove();
        }
      }, 20);

      choco.onclick = () => {
        score++;
        document.getElementById('score').innerText = score;
        clearInterval(fallInterval);
        choco.remove();
        if (score >= 10) {
          clearInterval(gameInterval);
          setTimeout(() => {
            document.getElementById('reward-modal').style.display = 'flex';
          }, 300);
        }
      };
    }, 1300);
  }
</script>

</body>
</html>
