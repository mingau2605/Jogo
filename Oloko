<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<title>Plataforma – 3 Mundos</title>
<style>
  body { margin: 0; background: #111; }
  canvas { display: block; margin: auto; }
</style>
</head>
<body>

<canvas id="game" width="800" height="400"></canvas>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

const gravity = 0.6;
let keys = {};

document.addEventListener("keydown", e => keys[e.key] = true);
document.addEventListener("keyup", e => keys[e.key] = false);

const player = {
  x: 50, y: 300, w: 30, h: 40,
  vx: 0, vy: 0,
  speed: 4, jump: -12,
  power: false
};

let world = 1;
let level = 1;
let lives = 3;

let platforms = [];
let enemies = [];
let powerups = [];
let boss = null;

function collide(a, b) {
  return a.x < b.x + b.w &&
         a.x + a.w > b.x &&
         a.y < b.y + b.h &&
         a.y + a.h > b.y;
}

function worldStyle() {
  if (world === 1) return {bg:"#7ec850", platform:"#3b7a2a"};
  if (world === 2) return {bg:"#aee6ff", platform:"#4aa3c4"};
  return {bg:"#ff9b4a", platform:"#b22222"};
}

function loadLevel() {
  platforms = [
    {x:0,y:350,w:800,h:50},
    {x:200,y:280,w:120,h:20},
    {x:420,y:230,w:120,h:20}
  ];

  enemies = [
    {x:500,y:320,w:30,h:30,dir:1}
  ];

  powerups = [
    {x:430,y:190,w:20,h:20}
  ];

  boss = null;

  if (level === 4) {
    boss = {
      x: 600, y: 290,
      w: 60, h: 60,
      life: 5 + world * 2,
      dir: -1
    };
    enemies = [];
    powerups = [];
  }

  player.x = 50;
  player.y = 300;
}

function loseLife() {
  lives--;
  if (lives <= 0) {
    alert("Game Over!");
    world = 1;
    level = 1;
    lives = 3;
  }
  loadLevel();
}

function update() {
  player.vx = 0;
  if (keys["ArrowRight"]) player.vx = player.speed;
  if (keys["ArrowLeft"]) player.vx = -player.speed;
  if (keys[" "] && player.vy === 0) player.vy = player.jump;

  player.vy += gravity;
  player.x += player.vx;
  player.y += player.vy;

  platforms.forEach(p => {
    if (collide(player, p) && player.vy > 0) {
      player.y = p.y - player.h;
      player.vy = 0;
    }
  });

  enemies.forEach(e => {
    e.x += e.dir * 2;
    if (e.x < 0 || e.x > 770) e.dir *= -1;
    if (collide(player, e)) loseLife();
  });

  powerups = powerups.filter(p => {
    if (collide(player, p)) {
      player.power = true;
      player.speed = 6;
      return false;
    }
    return true;
  });

  if (boss) {
    boss.x += boss.dir * 2;
    if (boss.x < 400 || boss.x > 740) boss.dir *= -1;

    if (collide(player, boss)) {
      if (player.vy > 0) {
        boss.life--;
        player.vy = player.jump / 2;
      } else {
        loseLife();
      }
    }

    if (boss.life <= 0) {
      level++;
      if (level > 4) {
        level = 1;
        world++;
        if (world > 3) {
          alert("Parabéns! Você zerou o jogo!");
          world = 1;
        }
      }
      loadLevel();
    }
  }

  if (player.x > 780 && !boss) {
    level++;
    loadLevel();
  }

  if (player.y > 400) loseLife();
}

function draw() {
  const style = worldStyle();
  ctx.fillStyle = style.bg;
  ctx.fillRect(0,0,800,400);

  ctx.fillStyle = style.platform;
  platforms.forEach(p => ctx.fillRect(p.x,p.y,p.w,p.h));

  ctx.fillStyle = player.power ? "gold" : "red";
  ctx.fillRect(player.x,player.y,player.w,player.h);

  ctx.fillStyle = "purple";
  enemies.forEach(e => ctx.fillRect(e.x,e.y,e.w,e.h));

  ctx.fillStyle = "yellow";
  powerups.forEach(p => ctx.fillRect(p.x,p.y,p.w,p.h));

  if (boss) {
    ctx.fillStyle = "black";
    ctx.fillRect(boss.x,boss.y,boss.w,boss.h);
    ctx.fillStyle = "white";
    ctx.fillText("Boss HP: " + boss.life, boss.x, boss.y - 5);
  }

  ctx.fillStyle = "black";
  ctx.fillText(`Mundo ${world}  Fase ${level}  Vidas: ${lives}`, 10, 20);
}

function loop() {
  update();
  draw();
  requestAnimationFrame(loop);
}

loadLevel();
loop();
</script>

</body>
</html>
