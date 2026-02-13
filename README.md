<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Valentine Catch Game for YSJ</title>
<link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&family=Single+Day&display=swap" rel="stylesheet">

<style>
  :root {
    --bg-pink: #E89BB1;
    --dark-red: #7A0F14;
    --kisses-brown: #5C3A21;
  }

  body {
    margin: 0;
    padding: 0;
    background-color: var(--bg-pink);
    font-family: 'Single Day', cursive; /* 부드러운 한글 폰트 */
    display: flex;
    flex-direction: column;
    align-items: center;
    color: var(--dark-red);
  }

  /* 상단 헤더: 피그마 시안 재현 */
  header {
    width: 100%;
    background-color: var(--dark-red);
    padding: 20px 0;
    position: relative;
    box-shadow: 0 4px 0 rgba(0,0,0,0.1);
  }

  .header-content {
    background-color: var(--bg-pink);
    margin: 0 10%;
    padding: 15px;
    border-radius: 20px;
    border: 3px solid white;
    position: relative;
  }

  .pixel-title {
    font-family: 'Press Start 2P', cursive; /* 도트 폰트 */
    font-size: 16px;
    margin: 0;
    color: var(--dark-red);
  }

  .to-ysj {
    position: absolute;
    bottom: 5px;
    right: 20px;
    font-size: 14px;
    font-style: italic;
  }

  /* 메인 키세스 로고 */
  .hero {
    margin-top: 50px;
    text-align: center;
  }

  .kisses-container {
    position: relative;
    width: 250px;
    height: 220px;
    margin: 0 auto;
  }

  .kisses-shape {
    width: 100%;
    height: 100%;
    background-color: var(--dark-red);
    clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
    border-radius: 0 0 40px 40px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    color: white;
  }

  /* 키세스 종이 띠 */
  .kisses-tag {
    position: absolute;
    top: 0;
    right: 30px;
    background: white;
    padding: 5px 15px;
    font-size: 10px;
    transform: rotate(20deg);
    border: 1px solid #ccc;
    box-shadow: 2px 2px 5px rgba(0,0,0,0.1);
    z-index: 10;
  }

  .main-text {
    font-family: 'Press Start 2P', cursive;
    font-size: 12px;
    line-height: 2;
  }

  /* 시작 버튼 */
  .start-btn {
    margin-top: 30px;
    padding: 15px 40px;
    font-size: 20px;
    background-color: var(--dark-red);
    color: white;
    border: none;
    border-radius: 50px;
    cursor: pointer;
    box-shadow: 0 5px 0 #5a0b0f;
  }

  .start-btn:active {
    transform: translateY(4px);
    box-shadow: none;
  }

  /* 게임 영역 */
  #game-area {
    display: none;
    width: 90vw;
    max-width: 500px;
    height: 400px;
    background: rgba(255,255,255,0.3);
    border: 4px dashed var(--dark-red);
    margin: 20px;
    position: relative;
    border-radius: 20px;
    overflow: hidden;
  }

  /* 떨어지는 키세스 초콜릿 */
  .drop-chocolate {
    position: absolute;
    width: 40px;
    height: 40px;
    background-color: var(--kisses-brown);
    clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
    cursor: pointer;
  }
</style>
</head>
<body>

<header>
  <div class="header-content">
    <p class="pixel-title">Happy Valentine's Day</p>
    <span class="to-ysj">to Y.S.J</span>
  </div>
</header>

<div class="hero" id="intro">
  <div class="kisses-container">
    <div class="kisses-tag">To. Y.S.J</div>
    <div class="kisses-shape">
      <div class="main-text">Chocolate<br>Catch Game</div>
    </div>
  </div>
  <p style="font-weight: bold;">초콜릿을 10개 모으면 특별한 보상을 받을 수 있어요.</p>
  <button class="start-btn" onclick="startGame()">Start Game</button>
</div>

<div id="game-area"></div>

<div id="reward-modal" style="display:none; position:fixed; inset:0; background:rgba(0,0,0,0.7); z-index:100; justify-content:center; align-items:center;">
  <div style="background:white; padding:40px; border-radius:20px; text-align:center;">
    <h2 style="color:var(--dark-red)">🎉 MISSION COMPLETE!</h2>
    <p>성준님을 위한 <b>'데이트권 1회'</b>가 발급되었습니다!</p>
    <button class="start-btn" onclick="location.reload()">닫기</button>
  </div>
</div>

<script>
  let score = 0;
  function startGame() {
    document.getElementById('intro').style.display = 'none';
    const area = document.getElementById('game-area');
    area.style.display = 'block';
    
    const interval = setInterval(() => {
      const choco = document.createElement('div');
      choco.className = 'drop-chocolate';
      choco.style.left = Math.random() * (area.clientWidth - 40) + 'px';
      choco.style.top = '-50px';
      area.appendChild(choco);

      // 떨어지는 애니메이션
      let top = -50;
      const drop = setInterval(() => {
        top += 5;
        choco.style.top = top + 'px';
        if (top > area.clientHeight) {
          clearInterval(drop);
          choco.remove();
        }
      }, 20);

      choco.onclick = () => {
        score++;
        choco.remove();
        if (score >= 10) {
          clearInterval(interval);
          document.getElementById('reward-modal').style.display = 'flex';
        }
      };
    }, 800);
  }
</script>

</body>
</html>
