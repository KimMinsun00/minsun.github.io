# minsun.github.io
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>코드1과 코드2 결과</title>
  <script src="https://cdn.jsdelivr.net/npm/p5@1.6.0/lib/p5.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.6.0/addons/p5.sound.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/p5.gif.js@0.2.2/dist/p5.gif.min.js"></script>

  <style>
    body { background:white; font-family:Arial; padding:20px; }
    h2 { margin-top:40px; }
    .sketch-box { margin-bottom:60px; }
  </style>
</head>
<body>

<h1>코드1 → 코드2 결과 페이지</h1>

<!-- 코드1 -->
<div class="sketch-box">
  <h2>코드1 결과</h2>
  <div id="sketch1"></div>
</div>

<!-- 코드2 -->
<div class="sketch-box">
  <h2>코드2 결과</h2>
  <div id="sketch2"></div>
</div>


<!-- ---------------------------- -->
<!-- 코드1 스케치 -->
<!-- ---------------------------- -->
<script>
let blink = false;
let lastBlink = 0;

let icecream = { x: 120, y: 360, targetX: 200, targetY: 310, visible: true, moving: false, eaten: false };
let mouthOutlineVisible = true;
let eatStart = 0;

let s1 = function(p){

p.setup = function() {
  p.createCanvas(400, 500).parent("sketch1");
  p.frameRate(30);
}

p.draw = function() {
  p.background(255, 182, 193);

  drawHeadBase();
  drawFace();
  drawHair();
  drawBody();
  drawEars();
  drawPiercings();

  if (p.millis() - lastBlink > 3000) {
    blink = !blink;
    lastBlink = p.millis();
  }

  drawEyes(blink);
  if (!blink) drawLashes();
  drawNose();

  if (mouthOutlineVisible) {
    p.fill(200, 30, 90);
    p.ellipse(200, 310, 75, 35);
  }
  p.fill(255, 160, 180);
  p.ellipse(200, 310, 55, 18);

  drawBlush();

  if (icecream.moving && !icecream.eaten) {
    let speed = 2;
    let dx = icecream.targetX - icecream.x;
    let dy = icecream.targetY - icecream.y;
    let distToTarget = Math.sqrt(dx * dx + dy * dy);
    if (distToTarget > 1) {
      icecream.x += (dx / distToTarget) * speed;
      icecream.y += (dy / distToTarget) * speed;
    } else {
      icecream.eaten = true;
      eatStart = p.millis();
    }
  }

  if (icecream.visible) drawIcecream(icecream.x, icecream.y);

  if (icecream.eaten && p.millis() - eatStart > 400 && icecream.visible) {
    icecream.visible = false;
    mouthOutlineVisible = false;
  }
};

p.mousePressed = function() {
  icecream.moving = true;
};

p.keyPressed = function() {
  if (p.key === 'r' || p.key === 'R') {
    icecream = { x: 120, y: 360, targetX: 200, targetY: 310, visible: true, moving: false, eaten: false };
    mouthOutlineVisible = true;
  }
  if (p.key === 's') {
    p.saveGif('mySketch', 10);
  }
};

// -----------------------------------------------------

function drawHeadBase() {
  p.fill(110, 70, 40);
  p.rect(70, 160, 250, 270, 20);
}
function drawFace() {
  p.fill(255, 220, 190);
  p.noStroke();
  p.ellipse(200, 250, 190, 190);
}
function drawHair() {
  p.fill(110, 70, 40);
  p.rect(63, 160, 55, 270, 30);
  p.rect(276, 160, 55, 270, 30);
  p.arc(197, 200, 268, 240, p.PI, 0);
  for (let x = 120; x <= 280; x += 12) p.ellipse(x, 150, 22, 45);
}
function drawBody() {
  p.fill(0);
  p.rect(70, 365, 250, 120, 20);
  p.fill(255);
  p.ellipse(200, 410, 15, 15);
  p.ellipse(200, 440, 15, 15);
  p.fill(255, 220, 190);
  p.rect(170, 300, 60, 90, 20);
}
function drawEars() {
  p.fill(255, 220, 190);
  p.ellipse(105, 250, 32, 48);
  p.ellipse(295, 250, 32, 48);
}
function drawPiercings() {
  p.fill(200);
  p.ellipse(95, 240, 6, 6);
  p.noFill();
  p.stroke(180);
  p.strokeWeight(2);
  p.arc(308, 250, 20, 15, (3 * p.PI) / 2, p.PI / 2);
  p.noStroke();
}
function drawEyes(isBlinking) {
  if (!isBlinking) {
    p.fill(255);
    p.ellipse(160, 245, 58, 40);
    p.ellipse(240, 245, 58, 40);
    p.fill(100, 60, 40);
    p.ellipse(160, 245, 30, 30);
    p.ellipse(240, 245, 30, 30);
    p.fill(0);
    p.ellipse(160, 245, 12, 12);
    p.ellipse(240, 245, 12, 12);
  } else {
    p.fill(0);
    p.rect(130, 240, 60, 6, 3);
    p.rect(210, 240, 60, 6, 3);
  }
}
function drawLashes() {
  p.stroke(0);
  p.strokeWeight(2);
  p.line(140, 230, 130, 220);
  p.line(160, 225, 160, 210);
  p.line(180, 230, 190, 220);
  p.line(240, 225, 240, 210);
  p.line(220, 230, 210, 220);
  p.line(260, 230, 270, 220);
  p.noStroke();
}
function drawNose() {
  p.stroke(80);
  p.strokeWeight(2);
  p.line(200, 255, 200, 275);
  p.noStroke();
}
function drawBlush() {
  p.fill(255, 150, 170, 120);
  p.ellipse(150, 280, 35, 25);
  p.ellipse(250, 280, 35, 25);
}
function drawIcecream(x, y) {
  p.fill(245, 200, 130);
  p.triangle(x - 10, y + 45, x + 10, y + 45, x, y + 100);
  p.fill(255);
  p.ellipse(x, y + 40, 40, 25);
  p.fill(255, 245, 250);
  p.ellipse(x, y + 25, 35, 22);
  p.fill(255, 250, 255);
  p.ellipse(x, y + 10, 30, 20);
}

};
new p5(s1);
</script>


<!-- ---------------------------- -->
<!-- 코드2 스케치 -->
<!-- ---------------------------- -->
<script>
let s2 = function(p){
let startTime;
let cheekColorA, cheekColorB;

p.setup = function() {
  p.createCanvas(570, 380).parent("sketch2");
  p.colorMode(p.RGB);
  startTime = p.millis();

  cheekColorA = p.color(255, 100, 100);
  cheekColorB = p.color(p.random(255), p.random(255), p.random(255));
}

p.draw = function() {
  let t = (p.millis() - startTime) / 1000;
  let loopT = t % 10;

  let bgC1 = p.color(0, 150, 150);
  let bgC2 = p.color(150, 0, 200);
  let pct = (Math.sin(t) + 1) / 2;

  p.colorMode(p.HSB);
  let hueValue = (p.millis() * 0.2) % 360; 
  p.background(hueValue, 80, 100);
  p.colorMode(p.RGB);

  p.stroke(0);
  p.strokeWeight(2);

  let faceY = Math.sin(t * 2) * 5;
  let earScale = 1 + Math.sin(t * 2) * 0.1;
  let handScale = 1 + Math.sin(t * 3) * 0.1;

  let cheekPct = (Math.sin(t * 1.5) + 1) / 2;
  let cheekColor = p.lerpColor(cheekColorA, cheekColorB, cheekPct);

  let eyeMove = Math.sin(t * 5) * 5;

  p.fill(0);
  p.ellipse(190, 80 + faceY, 100 * earScale, 100 * earScale);
  p.ellipse(410, 80 + faceY, 100 * earScale, 100 * earScale);

  p.fill(255);
  p.ellipse(300, 200 + faceY, 315, 285);

  p.fill(cheekColor);
  p.noStroke();
  p.ellipse(200, 210 + faceY, 60, 45);
  p.ellipse(400, 210 + faceY, 60, 45);

  p.fill(50);
  p.ellipse(240, 170 + faceY, 100, 60);
  p.ellipse(360, 170 + faceY, 100, 60);

  p.fill(255);
  p.ellipse(250 + eyeMove, 170 + faceY, 30, 30);
  p.ellipse(350 + eyeMove, 170 + faceY, 30, 30);

  p.fill(0);
  p.ellipse(250 + eyeMove * 1.5, 170 + faceY, 15, 15);
  p.ellipse(350 + eyeMove * 1.5, 170 + faceY, 15, 15);

  p.fill(0);
  p.ellipse(300, 225 + faceY, 20, 15);

  p.fill(255, 150, 150);
  p.stroke(0);
  p.strokeWeight(2);
  p.arc(300, 255 + faceY, 100, 100, 0, p.PI, p.CHORD);

  p.fill(0);
  p.ellipse(220, 330 + faceY, 100 * handScale, 60 * handScale);
  p.ellipse(380, 330 + faceY, 100 * handScale, 60 * handScale);

  p.ellipse(190, 345 + faceY, 25 * handScale, 25 * handScale);
  p.ellipse(220, 355 + faceY, 25 * handScale, 25 * handScale);
  p.ellipse(250, 345 + faceY, 25 * handScale, 25 * handScale);

  p.ellipse(350, 345 + faceY, 25 * handScale, 25 * handScale);
  p.ellipse(380, 355 + faceY, 25 * handScale, 25 * handScale);
  p.ellipse(410, 345 + faceY, 25 * handScale, 25 * handScale);
};

p.keyPressed = function() {
  if (p.key === 's') {
    p.saveGif('mySketch2', 5, { duration: 10 });
  }
};

};
new p5(s2);
</script>

</body>
</html>
