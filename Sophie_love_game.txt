<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Sophie 💖</title>
  <style>
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background-color: #fff0f5;
      overflow: hidden;
    }
    #header {
      text-align: center;
      font-size: 2rem;
      font-weight: bold;
      color: #d63384;
      padding: 1rem;
      background-color: #ffe3ec;
    }
    canvas {
      display: block;
      margin: 0 auto;
      background-color: #fffafc;
      border: 4px solid #ff69b4;
    }
    #message {
      display: none;
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: #fff5f8;
      border: 2px solid #ff69b4;
      padding: 2rem;
      border-radius: 15px;
      text-align: center;
      font-size: 1.5rem;
      color: #d63384;
      max-width: 90%;
      z-index: 10;
    }
  </style>
</head>
<body>
  <div id="header">Sophie 💖</div>
  <canvas id="gameCanvas" width="400" height="600"></canvas>
  <div id="message">
    <p>🎉 <strong>Happy birthday my love 💖</strong></p>
    <p>I love you mostestest.</p>
    <p>You are the most precious person in my life ❤️</p>
  </div>

  <script>
    const canvas = document.getElementById("gameCanvas");
    const ctx = canvas.getContext("2d");

    const basket = { x: 170, y: 550, width: 60, height: 30 };
    const hearts = [];
    let score = 0;
    const goal = 20;

    function drawBasket() {
      ctx.fillStyle = "#ff69b4";
      ctx.fillRect(basket.x, basket.y, basket.width, basket.height);
      ctx.fillStyle = "white";
      ctx.font = "20px Arial";
      ctx.fillText("🤍", basket.x + 20, basket.y + 22);
    }

    function drawHeart(heart) {
      ctx.font = "24px Arial";
      ctx.fillText("❤️", heart.x, heart.y);
    }

    function spawnHeart() {
      const x = Math.random() * (canvas.width - 24);
      hearts.push({ x: x, y: 0 });
    }

    function updateHearts() {
      for (let i = 0; i < hearts.length; i++) {
        hearts[i].y += 3;

        // Collision with basket
        if (
          hearts[i].y + 24 > basket.y &&
          hearts[i].x > basket.x &&
          hearts[i].x < basket.x + basket.width
        ) {
          hearts.splice(i, 1);
          score++;
          if (score >= goal) {
            document.getElementById("message").style.display = "block";
          }
        }
      }
    }

    function drawScore() {
      ctx.fillStyle = "#d63384";
      ctx.font = "18px Arial";
      ctx.fillText(`Caught: ${score}/${goal}`, 10, 20);
    }

    function draw() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      drawBasket();
      hearts.forEach(drawHeart);
      drawScore();
    }

    function gameLoop() {
      updateHearts();
      draw();
      requestAnimationFrame(gameLoop);
    }

    setInterval(spawnHeart, 1000);
    gameLoop();

    // Touch controls for phones
    canvas.addEventListener("touchstart", function (e) {
      const touchX = e.touches[0].clientX - canvas.getBoundingClientRect().left;
      basket.x = touchX - basket.width / 2;
    });

    // Mouse drag support for desktop
    canvas.addEventListener("mousemove", function (e) {
      const mouseX = e.clientX - canvas.getBoundingClientRect().left;
      basket.x = mouseX - basket.width / 2;
    });
  </script>
</body>
</html>
