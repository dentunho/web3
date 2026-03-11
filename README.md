<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<title>Flappy Bird Clone</title>
<style>
  body {
    margin: 0;
    overflow: hidden;
    background: #70c5ce;
    font-family: Arial;
    text-align: center;
  }
  canvas {
    display: block;
    margin: auto;
    background: #70c5ce;
  }
</style>
</head>
<body>

<canvas id="game" width="400" height="600"></canvas>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

let bird = {
  x: 80,
  y: 200,
  width: 30,
  height: 30,
  gravity: 0.5,
  lift: -8,
  velocity: 0
};

let pipes = [];
let frame = 0;
let score = 0;

document.addEventListener("keydown", () => {
  bird.velocity = bird.lift;
});

function drawBird() {
  ctx.fillStyle = "yellow";
  ctx.fillRect(bird.x, bird.y, bird.width, bird.height);
}

function drawPipes() {
  ctx.fillStyle = "green";
  pipes.forEach(pipe => {
    ctx.fillRect(pipe.x, 0, pipe.width, pipe.top);
    ctx.fillRect(pipe.x, pipe.bottom, pipe.width, canvas.height);
  });
}

function update() {
  frame++;

  bird.velocity += bird.gravity;
  bird.y += bird.velocity;

  if (frame % 90 === 0) {
    let gap = 150;
    let top = Math.random() * 250 + 50;

    pipes.push({
      x: canvas.width,
      width: 50,
      top: top,
      bottom: top + gap
    });
  }

  pipes.forEach(pipe => {
    pipe.x -= 2;

    if (
      bird.x < pipe.x + pipe.width &&
      bird.x + bird.width > pipe.x &&
      (bird.y < pipe.top || bird.y + bird.height > pipe.bottom)
    ) {
      alert("Game Over! Score: " + score);
      document.location.reload();
    }

    if (pipe.x + pipe.width === bird.x) {
      score++;
    }
  });

  if (bird.y + bird.height >= canvas.height || bird.y <= 0) {
    alert("Game Over! Score: " + score);
    document.location.reload();
  }
}

function drawScore() {
  ctx.fillStyle = "#000";
  ctx.font = "24px Arial";
  ctx.fillText("Score: " + score, 10, 30);
}

function gameLoop() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  update();
  drawBird();
  drawPipes();
  drawScore();

  requestAnimationFrame(gameLoop);
}

gameLoop();
</script>

</body>
</html>
