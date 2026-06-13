<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<title>Тест на отношения</title>

<style>
body {
  margin: 0;
  font-family: Arial;
  background: black;
  overflow: hidden;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

/* фон */
body::before {
  content: "";
  position: absolute;
  width: 200%;
  height: 200%;
  background: radial-gradient(circle, #222 10%, transparent 60%);
  animation: move 10s linear infinite;
}

@keyframes move {
  0% { transform: translate(0,0); }
  50% { transform: translate(-10%, -10%); }
  100% { transform: translate(0,0); }
}

/* контейнер */
.container {
  background: white;
  width: 420px;
  max-height: 90vh;        /* 🔥 важно */
  overflow: hidden;
  border: 3px solid black;
  border-radius: 10px;
  z-index: 2;
  position: relative;
  display: flex;
  flex-direction: column;
}

/* внутренняя прокрутка */
.scroll {
  overflow-y: auto;
  padding: 15px;
  flex: 1;
}

/* старт */
#startScreen {
  padding: 20px;
  text-align: center;
}

.startBtn {
  background: black;
  color: white;
  padding: 12px;
  border: none;
  cursor: pointer;
  font-size: 16px;
}

/* вопросы */
.question {
  margin-bottom: 15px;
}

.option {
  display: block;
  padding: 10px;
  margin: 5px 0;
  border: 1px solid black;
  cursor: pointer;
  transition: 0.2s;
}

.option:hover {
  background: #eee;
}

.option.selected {
  background: black;
  color: white;
}

button {
  width: 100%;
  padding: 10px;
  background: black;
  color: white;
  border: none;
  cursor: pointer;
  margin-top: 10px;
}

/* результат */
#result {
  margin-top: 15px;
  font-weight: bold;
  text-align: center;
}

/* сердечки */
.heart {
  position: absolute;
  color: pink;
  font-size: 20px;
  animation: floatUp 6s linear infinite;
  opacity: 0.8;
}

@keyframes floatUp {
  0% { transform: translateY(100vh); opacity: 0; }
  20% { opacity: 1; }
  100% { transform: translateY(-10vh); opacity: 0; }
}
</style>
</head>

<body>

<audio id="music" loop>
  <source src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_c8b1b6c3b0.mp3">
</audio>

<div class="container">

<!-- старт -->
<div id="startScreen">
  <h2>💖 Тест на отношения</h2>
  <button class="startBtn" onclick="start()">Начать</button>
</div>

<!-- тест -->
<div id="quizScreen" class="scroll" style="display:none;">

  <div class="question">
    <p>1. Дата знакомства</p>
    <div class="option" onclick="select(this,'q1','a')">13 октября</div>
    <div class="option" onclick="select(this,'q1','c')">19 сентября</div>
    <div class="option" onclick="select(this,'q1','b')">23 июня</div>
    <div class="option" onclick="select(this,'q1','d')">8 ноября</div>
  </div>

  <div class="question">
    <p>2. Сколько вместе?</p>
    <div class="option" onclick="select(this,'q2','a')">632</div>
    <div class="option" onclick="select(this,'q2','b')">700</div>
    <div class="option" onclick="select(this,'q2','c')">596</div>
    <div class="option" onclick="select(this,'q2','d')">954</div>
  </div>

  <div class="question">
    <p>3. Первое видео?</p>
    <div class="option" onclick="select(this,'q3','a')">про чувства</div>
    <div class="option" onclick="select(this,'q3','c')">про глаза</div>
    <div class="option" onclick="select(this,'q3','b')">про переписку</div>
    <div class="option" onclick="select(this,'q3','d')">про музыку</div>
  </div>

  <div class="question">
    <p>4. Какие глаза?</p>
    <div class="option" onclick="select(this,'q4','a')">зелёные и карие</div>
    <div class="option" onclick="select(this,'q4','c')">голубые и зелёные</div>
    <div class="option" onclick="select(this,'q4','b')">карие и голубые</div>
  </div>

  <div class="question">
    <p>5. Какая осень?</p>
    <div class="option" onclick="select(this,'q5','b')">3</div>
    <div class="option" onclick="select(this,'q5','a')">2</div>
    <div class="option" onclick="select(this,'q5','c')">4</div>
  </div>

  <div class="question">
    <p>6. Какой год?</p>
    <div class="option" onclick="select(this,'q6','a')">2010</div>
    <div class="option" onclick="select(this,'q6','b')">2026</div>
    <div class="option" onclick="select(this,'q6','c')">2024</div>
    <div class="option" onclick="select(this,'q6','d')">2020</div>
  </div>

  <button onclick="check()">Проверить</button>
  <div id="result"></div>

</div>
</div>

<script>

let answers = {};

function start() {
  document.getElementById("startScreen").style.display = "none";
  document.getElementById("quizScreen").style.display = "block";
  document.getElementById("music").play();
}

/* выбор */
function select(el,q,val) {
  answers[q] = val;
  let opts = el.parentNode.querySelectorAll(".option");
  opts.forEach(o => o.classList.remove("selected"));
  el.classList.add("selected");
}

/* проверка */
function check() {
  const correct = {
    q1:"c",
    q2:"a",
    q3:"b",
    q4:"b",
    q5:"b",
    q6:"a"
  };

  let score = 0;

  for (let q in correct) {
    if (answers[q] === correct[q]) score++;
  }

  let percent = Math.round(score/6*100);

  let level =
    percent <= 30 ? "💔 слабая связь" :
    percent <= 60 ? "💞 нормальная" :
    percent <= 90 ? "💖 сильная" :
    "💘 идеальная любовь";

  document.getElementById("result").innerHTML =
    `Результат: ${score}/6<br>${percent}%<br>${level}`;

  hearts();
}

/* сердечки */
function hearts() {
  for (let i=0;i<25;i++) createHeart();
  setInterval(createHeart,300);
}

function createHeart() {
  const h = document.createElement("div");
  h.className = "heart";
  h.innerHTML = "💖";
  h.style.left = Math.random()*100+"vw";
  h.style.fontSize = (10+Math.random()*20)+"px";
  h.style.animationDuration = (3+Math.random()*3)+"s";
  document.body.appendChild(h);
  setTimeout(()=>h.remove(),6000);
}

</script>

</body>
</html>
