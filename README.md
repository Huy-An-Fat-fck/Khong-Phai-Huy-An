<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Mô phỏng sóng biến điệu</title>
<style>
  body {
    font-family: Arial, sans-serif;
    text-align: center;
    background: #f2f2f2;
    margin: 0;
    padding: 0;
  }
  h1 {
    margin-top: 20px;
  }
  .controls {
    display: flex;
    justify-content: center;
    gap: 20px;
    margin: 20px 0;
    flex-wrap: wrap;
  }
  .control-box {
    background: #fff;
    padding: 15px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.2);
  }
  canvas {
    background: #fff;
    border: 1px solid #ccc;
    margin-bottom: 20px;
  }
</style>
</head>
<body>
<h1>Mô phỏng Sóng AM – FM cơ bản</h1>

<div class="controls">
  <div class="control-box">
    <h3>Sóng âm tần</h3>
    Biên độ: <input id="ampSignal" type="number" value="" step="0.1"><br>
    Tần số: <input id="freqSignal" type="number" value="" step="0.1">
  </div>
  <div class="control-box">
    <h3>Sóng mang</h3>
    Biên độ: <input id="ampCarrier" type="number" value="" step="0.1"><br>
    Tần số: <input id="freqCarrier" type="number" value="" step="0.1">
  </div>
</div>

<canvas id="canvas" width="800" height="300"></canvas>

<script>
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');

let ampSignal = document.getElementById('ampSignal').value;
let freqSignal = document.getElementById('freqSignal').value;
let ampCarrier = document.getElementById('ampCarrier').value;
let freqCarrier = document.getElementById('freqCarrier').value;

document.querySelectorAll('input').forEach(input=>{
  input.addEventListener('input',()=>{
    ampSignal = parseFloat(document.getElementById('ampSignal').value);
    freqSignal = parseFloat(document.getElementById('freqSignal').value);
    ampCarrier = parseFloat(document.getElementById('ampCarrier').value);
    freqCarrier = parseFloat(document.getElementById('freqCarrier').value);
  });
});

let t = 0;
function draw() {
  t += 0.02;
  ctx.clearRect(0,0,canvas.width,canvas.height);

  // Vẽ trục giữa
  ctx.strokeStyle = '#aaa';
  ctx.beginPath();
  ctx.moveTo(0,canvas.height/2);
  ctx.lineTo(canvas.width,canvas.height/2);
  ctx.stroke();

  // Sóng âm tần
  drawWave('#00b894', (x)=> ampSignal*Math.sin(freqSignal*x + t));

  // Sóng mang
  drawWave('#0984e3', (x)=> ampCarrier*Math.sin(freqCarrier*x + t));

  // Sóng biến điệu biên độ (AM)
  drawWave('#d63031', (x)=>
    (ampCarrier + ampSignal*Math.sin(freqSignal*x + t)) * Math.sin(freqCarrier*x + t)
  );

  requestAnimationFrame(draw);
}

function drawWave(color, func) {
  ctx.strokeStyle = color;
  ctx.beginPath();
  for (let x=0; x<canvas.width; x++) {
    let scaledX = x/50;
    let y = func(scaledX);
    let canvasY = canvas.height/2 - y*20; // scale lên
    if (x===0) ctx.moveTo(x, canvasY);
    else ctx.lineTo(x, canvasY);
  }
  ctx.stroke();
}

draw();
</script>
</body>
</html>
