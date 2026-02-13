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
    --dot-color: #f0c3cb; /* 도트 배경용 밝은 갈색 */
    --container-bg: rgba(255,255,255,0.7); /* 컨테이너 배경색 */
  }

  body {
    margin: 0;
    /* 이미지와 동일한 도트 패턴 배경 */
    background-color: var(--bg-pink);
    background-image: radial-gradient(var(--dot-color) 1.5px, transparent 1.5px);
    background-size: 25px 25px;
    color: var(--dark-red);
    font-family: 'Single Day', cursive;
    display: flex;
    flex-direction: column;
    align-items: center;
    min-height: 100vh;
    padding-bottom: 50px; /* 하단 여백 */
    box-sizing: border-box;
  }

  /* --- 상단 헤더 영역 (이미지 하트 디테일 추가) --- */
  header {
    width: 100%;
    background-color: transparent; /* 이미지처럼 투명 */
    padding: 20px 0 0; /* 위쪽 여백 */
    position: relative;
    z-index: 10;
  }

  .header-box {
    background-color: var(--bg-pink);
    margin: 0 15%;
    padding: 15px;
    border-radius: 15px;
    border: 2px solid white;
    text-align: center;
    box-shadow: 0 5px 15px rgba(0,0,0,0.1);
    position: relative;
    z-index: 2; /* 하트 위로 오게 */
  }

  .header-title {
    font-family: 'DotGothic16', sans-serif;
    font-size: clamp(18px, 5vw, 28px);
    margin: 0;
  }

  .sub-text {
    font-family: 'Great Vibes', cursive;
    font-size: 18px;
    display: flex;
    justify-content: space-between;
    padding: 0 15px;
    margin-top: 5px;
  }

  /* 블렌더 처리된 하트 */
  .blender-heart {
    position: absolute;
    width: 80px;
    height: 70px;
    background-color: var(--dark-red);
    transform: rotate(-45deg);
    border-radius: 50% 50% 0 50%;
    box-shadow: 0 5px 15px rgba(0,0,0,0.3); /* 입체감 */
    z-index: 1; /* 헤더 박스 아래로 */
  }

  .blender-heart::before,
  .blender-heart::after {
    content: "";
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: var(--dark-red);
    border-radius: 50%;
  }

  .blender-heart::before { top: -50%; left: 0; }
  .blender-heart::after { left: 50%; top: 0; }

  .header-heart-left { left: 10%; top: -10px; } /* 위치 조정 */
  .header-heart-right { right: 10%; top: -10px; } /* 위치 조정 */

  /* --- 메인 키세스 & 스타트 버튼 영역 --- */
  .hero { 
    margin: 50px 0 30px; 
    text-align: center; 
    background: var(--container-bg); /* 컨테이너 배경색 */
    padding: 30px 40px;
    border-radius: 30px; /* 둥근 모서리 */
    box-shadow: 0 10px 20px rgba(0,0,0,0.15); /* 그림자 */
    max-width: 400px;
    width: 90%;
  }

  .kisses-wrap {
    position: relative;
    width: 250px;
    height: 200px;
    margin: 0 auto;
  }

  .kisses-main {
    width: 100%;
    height: 100%;
    background-color: var(--dark-red);
    clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
    box-shadow: 0 10px 20px rgba(0,0,0,0.2), inset 0 -10px 20px rgba(0,0,0,0.3); /* 입체 그림자 */
  }

  .kisses-text {
    font-family: 'Jacquarda Bastarda 9', serif;
    color: white;
    font-size: 28px;
    line-height: 1.1;
    text-align: center;
    margin-top: 30px; /* 띠지 피해서 */
    text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
  }

  .kisses-tag {
    position: absolute;
    top: 0; /* 이미지처럼 위로 바싹 붙임 */
    right: 20px;
    background: white;
    padding: 5px 15px;
    font-family: 'Great Vibes', cursive;
    font-size: 14px;
    color: var(--dark-red);
    transform: rotate(20deg);
    border: 1px solid #ddd;
    box-shadow: 3px 3px 8px rgba(0,0,0,0.2);
    z-index: 5;
    white-space: nowrap;
    border-radius: 3px;
  }

  .start-btn {
    padding: 15px 40px;
    font-family: 'DotGothic16', sans-serif;
    font-size: 20px;
    background: var(--dark-red);
    color: white;
    border: 2px solid white;
    border-radius: 50px;
    cursor: pointer;
    margin-top: 20px;
    box-shadow: 0 5px 10px rgba(0,0,0,0.2);
    transition: all 0.2s;
  }

  .start-btn:hover { background: #5a0b0f; transform: translateY(-2px); box-shadow: 0 7px 15px rgba(0,0,0,0.3); }
  .start-btn:active { transform: translateY(0); box-shadow: 0 3px 8px rgba(0,0,0,0.2); }

  /* --- 게임 컨테이너 & 스코어보드 --- */
  #game-container-wrap {
    display: none;
    background: var(--container-bg);
    padding: 30px 40px;
    border-radius: 30px;
    box-shadow: 0 10px 20px rgba(0,0,0,0.15);
    max-width: 400px;
    width: 90%;
    margin-bottom: 30px;
  }

  #score-board {
    font-family: 'DotGothic16', sans-serif;
    font-size: 20px;
    background: white;
    padding: 10px 25px;
    border-radius: 15px;
    margin-bottom: 20px;
    box-shadow: inset 0 2px 5px rgba(0,0,0,0.1);
    display: flex;
    justify-content: space-between; /* 이미지처럼 정렬 */
    align-items: center;
    border: 2px solid var(--dark-red);
  }

  #game-area {
    width: 100%;
    height: 350px;
    background: white;
    border: 3px dashed var(--dark-red);
    position: relative;
    border-radius: 20px;
    overflow: hidden;
    box-shadow: inset 0 0 10px rgba(0,0,0,0.1);
  }

  /* --- 떨어지는 초콜릿 --- */
  .choco-item {
    position: absolute;
    width: 45px;
    height: 45px;
    background-color: var(--kisses-brown);
    clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
    cursor: pointer;
    box-shadow: 0 5px 10px rgba(0,0,0,0.2); /* 초콜릿에도 그림자 */
    transition: transform 0.1s ease-out;
  }

  .choco-item:hover { transform: scale(1.1); }

  /* --- 미션 완료 모달 --- */
  #reward-modal {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.8);
    z-index: 2000;
    justify-content: center;
    align-items: center;
  }

  #reward-modal .modal-content {
    background: var(--container-bg);
    padding: 40px 50px;
    border-radius: 30px;
    text-align: center;
    border: 5px solid var(--dark-red);
    max-width: 80%;
    box-shadow: 0 15px 30px rgba(0,0,0,0.3);
  }

  #reward-modal h2 {
    font-family: 'Great Vibes', cursive;
    font-size: 35px;
    margin-bottom: 15px;
    color: var(--dark-red);
  }

  #reward-modal p {
    font-size: 18px;
    margin-bottom: 30px;
  }

  .replay-btn {
    padding: 12px 30px;
    font-family: 'DotGothic16', sans-serif;
    font-size: 18px;
    background: var(--dark-red);
    color: white;
    border: 2px solid white;
    border-radius: 50px;
    cursor: pointer;
    box-shadow: 0 3px 8px rgba(0,0,0,0.2);
    transition: all 0.2s;
  }

  .replay-btn:hover { background: #5a0b0f; transform: translateY(-1px); box-shadow: 0 5px 10px rgba(0,0,0,0.3); }
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
  <p style="font-size: 18px; margin-top: 20px;">초콜릿 10개를 모아 데이트권을 획득하세요!</p>
  <button class="start-btn" onclick="startGame()">START GAME</button>
</div>

<div id="game-container-wrap">
  <div id="score-board">Score: <span id="score">0</span> / 10</div>
  <div id="game-area"></div>
</div>

<div id="reward-modal">
  <div class="modal-content">
    <h2>Reward Complete!</h2>
    <p>성준님 전용 <b>'달콤한 데이트권'</b> 획득! 💌</p>
    <button class="replay-btn" onclick="location.reload()">REPLAY</button>
  </div>
</div>

<script>
  let score = 0;
  let gameInterval; // 게임 종료 시 인터벌을 클리어하기 위해 변수 선언

  function startGame() {
    document.getElementById('intro').style.display = 'none';
    document.getElementById('game-container-wrap').style.display = 'block';
    const area = document.getElementById('game-area');
    
    // 초콜릿 생성 간격 (1.2초에 하나)
    gameInterval = setInterval(() => {
      const choco = document.createElement('div');
      choco.className = 'choco-item';
      choco.style.left = Math.random() * (area.clientWidth - 50) + 'px';
      choco.style.top = '-50px';
      area.appendChild(choco);

      // 낙하 속도 (2.5픽셀/20ms -> 1초에 125픽셀)
      let pos = -50;
      const fallInterval = setInterval(() => {
        pos += 2.5; 
        choco.style.top = pos + 'px';
        
        if (pos > area.clientHeight) {
          clearInterval(fallInterval);
          choco.remove();
        }
      }, 20); // 20ms마다 위치 업데이트

      choco.onclick = () => {
        score++;
        document.getElementById('score').innerText = score;
        clearInterval(fallInterval); // 클릭하면 떨어지는 것도 멈춰야 함
        choco.remove();
        if (score >= 10) {
          clearInterval(gameInterval); // 게임 종료
          document.getElementById('reward-modal').style.display = 'flex';
        }
      };
    }, 1200);
  }
</script>

</body>
</html>
