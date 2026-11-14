# Beeb
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>صافرة كل دقيقة</title>
<style>
  * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
  body {
    margin: 0; min-height: 100vh; display: flex; align-items: center; justify-content: center;
    font-family: "Segoe UI", Tahoma, sans-serif;
    background: radial-gradient(circle at 50% 0%, #1e2a3a 0%, #0d1219 70%);
    color: #e8eef6; padding: 16px;
  }
  .card {
    width: 100%; max-width: 460px; background: #141c26;
    border: 1px solid #23303f; border-radius: 22px; padding: 22px;
    box-shadow: 0 20px 50px rgba(0,0,0,.55);
  }
  h1 { font-size: 18px; text-align: center; margin: 0 0 4px; font-weight: 600; }
  .sub { text-align: center; font-size: 12px; color: #7d8ea3; margin-bottom: 18px; }

  .display {
    position: relative; text-align: center; padding: 18px 0 12px;
    background: #0b1118; border: 1px solid #1e2a38; border-radius: 16px;
    margin-bottom: 16px; transition: .2s;
  }
  .display.flash { background: #12301f; border-color: #2fd07a; box-shadow: 0 0 30px rgba(47,208,122,.5); }
  #clock {
    font-size: 64px; font-weight: 700; letter-spacing: 2px; line-height: 1;
    font-variant-numeric: tabular-nums; color: #2fd07a; transition: color .2s;
  }
  #clock.warn { color: #ffb020; }
  #clock.hot  { color: #ff5252; }
  .label { font-size: 12px; color: #6d7f94; margin-top: 8px; }

  .row { display: flex; gap: 10px; margin-bottom: 12px; }
  button {
    flex: 1; padding: 14px 8px; border-radius: 12px; border: none; cursor: pointer;
    font-size: 15px; font-weight: 600; font-family: inherit;
    background: #1f2c3b; color: #cfe0f0; transition: .15s;
  }
  button:hover  { background: #2a3b4f; }
  button:active { transform: scale(.97); }
  .primary { background: linear-gradient(180deg,#2fd07a,#1ea85d); color: #04140a; }
  .primary:hover { background: linear-gradient(180deg,#3ae089,#25b968); }
  .danger  { background: #3a2126; color: #ff8a8a; }
  .danger:hover { background: #4d2a30; }

  .section { border-top: 1px solid #22303f; padding-top: 14px; margin-top: 6px; }
  .section-title { font-size: 13px; color: #7d8ea3; margin-bottom: 10px; font-weight: 600; }

  .stepper { display: flex; align-items: center; gap: 10px; margin-bottom: 12px; }
  .stepper button { flex: 0 0 48px; padding: 12px 0; font-size: 20px; }
  .stepper .val {
    flex: 1; text-align: center; font-size: 18px; font-weight: 700; color: #9ad9ff;
    font-variant-numeric: tabular-nums;
  }
  .presets { display: grid; grid-template-columns: repeat(4, 1fr); gap: 8px; margin-bottom: 12px; }
  .presets button { padding: 10px 0; font-size: 13px; }
  .presets button.active { background: #1e5f3f; color: #6cffb0; }

  .adds { display: grid; grid-template-columns: repeat(4, 1fr); gap: 8px; }

  .vol { display: flex; align-items: center; gap: 12px; }
  .vol input { flex: 1; accent-color: #2fd07a; }
  .vol span { font-size: 13px; color: #7d8ea3; width: 34px; text-align: left; }

  .footer { display: flex; justify-content: space-between; font-size: 12px; color: #6d7f94; margin-top: 16px; }
  .dot { display: inline-block; width: 8px; height: 8px; border-radius: 50%; background: #46586d; margin-left: 6px; vertical-align: middle; }
  .dot.on { background: #2fd07a; box-shadow: 0 0 8px #2fd07a; animation: pulse 1.4s infinite; }
  @keyframes pulse { 50% { opacity: .35; } }
</style>
</head>
<body>

<div class="card">
  <h1>🔔 صافرة كل دقيقة</h1>
  <div class="sub">عدّاد لا يتوقف — يصفر ثم يعيد نفسه تلقائياً</div>

  <div class="display" id="display">
    <div id="clock">01:00</div>
    <div class="label">الوقت المتبقي للصافرة القادمة</div>
  </div>

  <div class="row">
    <button class="primary" id="btnStart">▶ تشغيل</button>
    <button class="danger"  id="btnStop">⏸ إيقاف</button>
    <button id="btnReset">↺ تصفير</button>
  </div>

  <div class="section">
    <div class="section-title">⏱️ الفاصل الزمني بين كل صافرة</div>
    <div class="stepper">
      <button id="ivMinus">−</button>
      <div class="val" id="ivVal">60 ثانية (1:00)</div>
      <button id="ivPlus">+</button>
    </div>
    <div class="presets">
      <button data-iv="30">30 ث</button>
      <button data-iv="60">1 دقيقة</button>
      <button data-iv="120">2 دقيقة</button>
      <button data-iv="300">5 دقائق</button>
    </div>
  </div>

  <div class="section">
    <div class="section-title">➕ زيادة الوقت المتبقي</div>
    <div class="adds">
      <button data-add="10">+10 ث</button>
      <button data-add="30">+30 ث</button>
      <button data-add="60">+1 د</button>
      <button data-add="300">+5 د</button>
    </div>
  </div>

  <div class="section">
    <div class="section-title">🔊 مستوى الصوت</div>
    <div class="vol">
      <input type="range" id="vol" min="0" max="100" value="55">
      <span id="volTxt">55%</span>
      <button id="btnTest" style="flex:0 0 90px;padding:10px 0;font-size:13px;">تجربة</button>
    </div>
  </div>

  <div class="footer">
    <div><span class="dot" id="dot"></span><span id="statusTxt">متوقف</span></div>
    <div>عدد الصفارات: <b id="count">0</b></div>
  </div>
</div>

<script>
/* ================= الحالة ================= */
const state = {
  interval: 60,      // الفاصل بالثواني
  remaining: 60,     // المتبقي بالثواني
  running: false,
  lastTick: 0,
  count: 0,
  volume: 0.55
};

/* ================= الصوت ================= */
let audioCtx = null;

function ensureAudio() {
  if (!audioCtx) {
    audioCtx = new (window.AudioContext || window.webkitAudioContext)();
  }
  if (audioCtx.state === 'suspended') audioCtx.resume();
  return audioCtx;
}

/* نفخة صافرة واحدة (تردد عالٍ مع اهتزاز = صوت صفارة حقيقي) */
function whistleBurst(ctx, startTime, duration) {
  const vol = state.volume;
  const master = ctx.createGain();
  master.gain.setValueAtTime(0.0001, startTime);
  master.gain.linearRampToValueAtTime(vol, startTime + 0.02);
  master.gain.setValueAtTime(vol, startTime + duration - 0.09);
  master.gain.exponentialRampToValueAtTime(0.0001, startTime + duration);
  master.connect(ctx.destination);

  const osc = ctx.createOscillator();
  osc.type = 'sine';
  osc.frequency.setValueAtTime(2600, startTime);

  // اهتزاز التردد (Trill) الذي يعطي طابع الصافرة
  const lfo = ctx.createOscillator();
  lfo.type = 'sine';
  lfo.frequency.value = 38;
  const lfoGain = ctx.createGain();
  lfoGain.gain.value = 240;
  lfo.connect(lfoGain);
  lfoGain.connect(osc.frequency);

  osc.connect(master);
  osc.start(startTime);
  lfo.start(startTime);
  osc.stop(startTime + duration + 0.03);
  lfo.stop(startTime + duration + 0.03);
}

/* صافرتان متتاليتان: بيب بيب */
function playWhistle() {
  const ctx = ensureAudio();
  const t = ctx.currentTime + 0.02;
  whistleBurst(ctx, t, 0.42);
  whistleBurst(ctx, t + 0.60, 0.42);
}

/* ================= المؤقت ================= */
function tick() {
  if (!state.running) return;

  const now = performance.now();
  const dt = (now - state.lastTick) / 1000;
  state.lastTick = now;

  state.remaining -= dt;

  if (state.remaining <= 0) {
    playWhistle();
    state.count++;
    flashScreen();
    // إعادة الضبط مع الحفاظ على الإيقاع (يستمر بلا توقف)
    do { state.remaining += state.interval; } while (state.remaining <= 0);
  }

  render();
}

function start() {
  ensureAudio();               // يجب أن يكون بعد تفاعل المستخدم
  if (state.running) return;
  if (state.remaining <= 0) state.remaining = state.interval;
  state.running = true;
  state.lastTick = performance.now();
  render();
}

function stop() {
  state.running = false;
  render();
}

function reset() {
  state.running = false;
  state.remaining = state.interval;
  state.count = 0;
  render();
}

/* ================= الرسم ================= */
const $ = id => document.getElementById(id);

function fmt(sec) {
  sec = Math.max(0, Math.ceil(sec));
  const m = Math.floor(sec / 60);
  const s = sec % 60;
  return String(m).padStart(2, '0') + ':' + String(s).padStart(2, '0');
}

function render() {
  const clk = $('clock');
  clk.textContent = fmt(state.remaining);

  const total = state.interval;
  const ratio = total > 0 ? state.remaining / total : 0;
  clk.className = ratio <= 0.1 ? 'hot' : (ratio <= 0.33 ? 'warn' : '');

  $('ivVal').textContent = state.interval + ' ثانية (' + fmt(state.interval) + ')';

  document.querySelectorAll('.presets button').forEach(b => {
    b.classList.toggle('active', Number(b.dataset.iv) === state.interval);
  });

  $('dot').classList.toggle('on', state.running);
  $('statusTxt').textContent = state.running ? 'يعمل...' : 'متوقف';
  $('count').textContent = state.count;

  $('btnStart').disabled = state.running;
  $('btnStart').style.opacity = state.running ? .45 : 1;
}

function flashScreen() {
  const d = $('display');
  d.classList.add('flash');
  setTimeout(() => d.classList.remove('flash'), 450);
}

/* ================= الأحداث ================= */
$('btnStart').onclick = start;
$('btnStop').onclick  = stop;
$('btnReset').onclick = reset;
$('btnTest').onclick  = () => { ensureAudio(); playWhistle(); };

$('ivPlus').onclick = () => {
  state.interval = Math.min(3600, state.interval + 10);
  if (!state.running) state.remaining = state.interval;
  render();
};

$('ivMinus').onclick = () => {
  state.interval = Math.max(5, state.interval - 10);
  if (!state.running) state.remaining = state.interval;
  render();
};

document.querySelectorAll('.presets button').forEach(b => {
  b.onclick = () => {
    state.interval = Number(b.dataset.iv);
    if (!state.running) state.remaining = state.interval;
    render();
  };
});

document.querySelectorAll('.adds button').forEach(b => {
  b.onclick = () => {
    state.remaining += Number(b.dataset.add);
    if (!state.running) {
      // إذا كان متوقفاً، اجعل الزيادة ضمن الفاصل
      state.interval = Math.ceil(state.remaining);
    }
    render();
  };
});

$('vol').oninput = e => {
  state.volume = e.target.value / 100;
  $('volTxt').textContent = e.target.value + '%';
};

/* ================= التشغيل ================= */
setInterval(tick, 200);
render();
</script>
</body>
</html>
369472
