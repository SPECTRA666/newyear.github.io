<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<title>До Нового года</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
:root {
  --bg: #0f172a;
  --text: #ffffff;
  --card: #1e293b;
}

.light {
  --bg: #f8fafc;
  --text: #0f172a;
  --card: #e2e8f0;
}

body {
  margin: 0;
  height: 100vh;
  background: var(--bg);
  color: var(--text);
  font-family: Arial, sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
}

.card {
  background: var(--card);
  padding: 30px;
  border-radius: 20px;
  text-align: center;
  box-shadow: 0 0 30px rgba(0,0,0,.3);
}

h1 {
  margin-bottom: 20px;
}

#timer {
  font-size: 2rem;
  margin-bottom: 20px;
}

button, select, input {
  padding: 8px 12px;
  margin: 5px;
  border-radius: 10px;
  border: none;
  cursor: pointer;
}

.snow {
  position: fixed;
  top: -10px;
  color: white;
  font-size: 1em;
  user-select: none;
  animation: fall linear infinite;
}

@keyframes fall {
  to {
    transform: translateY(110vh);
  }
}
</style>
</head>

<body>
<div class="card">
  <h1 id="title">До Нового года осталось</h1>
  <div id="timer">Загрузка...</div>

  <div>
    <input type="date" id="customDate">
    <button onclick="setCustomDate()">Установить дату</button>
  </div>

  <div>
    <button onclick="toggleTheme()">🌗 Тема</button>
    <button onclick="toggleSound()">🔊 Звук</button>
  </div>

  <div>
    <select onchange="setLang(this.value)">
      <option value="ru">Русский</option>
      <option value="am">Հայերեն</option>
    </select>
  </div>
</div>

<audio id="sound" src="https://www.soundjay.com/misc/sounds/bell-ringing-05.mp3"></audio>

<script>
let targetDate = getNewYear();
let soundEnabled = false;
let lang = "ru";

const texts = {
  ru: {
    title: "До события осталось",
    finished: "🎉 С НОВЫМ ГОДОМ!"
  },
  am: {
    title: "Մինչ Նոր տարին մնացել է",
    finished: "🎉 ՇՆՈՐՀԱՎՈՐ ՆՈՐ ՏԱՐԻ"
  }
};

function getNewYear() {
  const now = new Date();
  return new Date(`January 1, ${now.getFullYear()+1} 00:00:00`);
}

function updateTimer() {
  const now = new Date();
  const diff = targetDate - now;

  if (diff <= 0) {
    document.getElementById("timer").innerText = texts[lang].finished;
    if (soundEnabled) document.getElementById("sound").play();
    return;
  }

  const d = Math.floor(diff / 86400000);
  const h = Math.floor(diff / 3600000) % 24;
  const m = Math.floor(diff / 60000) % 60;
  const s = Math.floor(diff / 1000) % 60;

  document.getElementById("timer").innerText =
    `${d} д ${h} ч ${m} мин ${s} сек`;
}

setInterval(updateTimer, 1000);
updateTimer();

function toggleTheme() {
  document.body.classList.toggle("light");
}

function toggleSound() {
  soundEnabled = !soundEnabled;
}

function setLang(l) {
  lang = l;
  document.getElementById("title").innerText = texts[l].title;
}

function setCustomDate() {
  const input = document.getElementById("customDate").value;
  if (input) targetDate = new Date(input + "T00:00:00");
}

// ❄️ СНЕГ
function createSnow() {
  const snow = document.createElement("div");
  snow.className = "snow";
  snow.innerText = "❄";
  snow.style.left = Math.random() * window.innerWidth + "px";
  snow.style.animationDuration = 3 + Math.random() * 5 + "s";
  snow.style.opacity = Math.random();
  snow.style.fontSize = 10 + Math.random() * 20 + "px";
  document.body.appendChild(snow);

  setTimeout(() => snow.remove(), 8000);
}
setInterval(createSnow, 200);
</script>

</body>
</html>
