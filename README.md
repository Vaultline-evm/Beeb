<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>صافرتان — تحذيرية + تنفيذية</title>
<style>
  * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
  body {
    margin: 0; min-height: 100vh; display: flex;
    font-family: "Segoe UI", Tahoma, sans-serif;
    background: radial-gradient(circle at 50% 0%, #1e2a3a 0%, #0d1219 70%);
    color: #e8eef6; padding: 16px;
  }
  .card {
    margin: auto; width: 100%; max-width: 470px; background: #141c26;
    border: 1px solid #23303f; border-radius: 22px; padding: 22px;
    box-shadow: 0 20px 50px rgba(0,0,0,.55);
  }
  h1 { font-size: 18px; text-align: center; margin: 0 0 4px; font-weight: 600; }
  .sub { text-align: center; font-size: 12px; color: #7d8ea3; margin-bottom: 18px; }

  .display {
    position: relative; text-align: center; padding: 18px 0 14px;
    background: #0b1118; border: 1px solid #1e2a38; border-radius: 16px;
    margin-bottom: 16px; transition: background .18s, border-color .18s, box-shadow .18s;
  }
  .display.flash-short { background: #2e2612; border-color: #ffb020; box-shadow: 0 0 30px rgba(255,176,32,.45); }
  .display.flash-long  { background: #12301f; border-color: #2fd07a; box-shadow: 0 0 36px rgba(47,208,122,.55); }
  #clock {
    font-size: 62px; font-weight: 700; letter-spacing: 2px; line-height: 1;
    font-variant-numeric: tabular-nums; color: #2fd07a; transition: color .2s;
  }
  #clock.warn { color: #ffb020; }
  #clock.hot  { color: #ff5252; }
  .label { font-size: 11px; color: #6d7f94; margin-top: 8px; }
  .pre-info { font-size: 12.5px; margin-top: 10px; color: #8fb3d6; font-weight: 600; }
  .pre-info.done { color: #2fd07a; }
  .pre-info.off  { color: #55677d; font-weight: 400; }

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
  .section-title { font-size: 12.5px; color: #7d8ea3; margin-bottom: 10px; font-weight: 600; }

  .stepper { display: flex; align-items: center; gap: 10px; margin-bottom: 12px; }
  .stepper button { flex: 0 0 48px; padding: 12px 0; font-size: 20px; }
  .stepper .val {
    flex: 1; text-align: center; font-size: 17px; font-weight: 700; color: #9ad9ff;
    font-variant-numeric: tabular-nums;
  }
  .presets { display: grid; grid-template-columns: repeat(4, 1fr); gap: 8px; margin-bottom: 4px; }
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
  <h1>🔔 صافرتان لكل دورة</h1>
  <div class="sub">قصيرة قبل الموعد + طويلة عند الموعد تماماً</div>

  <div class="display" id="display">
    <div id="clock">01:00</div>
    <div class="label">الوقت المتبقي للصافرة الطويلة (التنفيذية)</div>
    <div class="pre-info" id="preInfo">🔉 الصافرة القصيرة بعد 00:50</div>
  </div>

  <div class="row">
    <button class="primary" id="btnStart">▶ تشغيل</button>
    <button class="danger"  id="btnStop">⏸ إيقاف</button>
    <button id="btnReset">↺ تصفير</button>
  </div>

  <div class="section">
    <div class="section-title">⏱️ الفاصل بين الدورات</div>
    <div class="stepper">
      <button id="ivMinus">−</button>
      <div class="val" id="ivVal">60 ث (01:00)</div>
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
    <div class="section-title">🔉 الصافرة القصيرة (التحذيرية) — تُطلق قبل الموعد بـ</div>
    <div class="stepper">
      <button id="preMinus">−</button>
      <div class="val" id="preVal">10 ثانية</div>
      <button id="prePlus">+</button>
    </div>
    <div class="presets">
      <button data-pre="0">معطّلة</button>
      <button data-pre="5">5 ث</button>
      <button data-pre="10">10 ث</button>
      <button data-pre="30">30 ث</button>
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
    <div class="section-title">🔊 مستوى الصوت والتجربة</div>
    <div class="vol">
      <input type="range" id="vol" min="0" max="100" value="55">
      <span id="volTxt">55%</span>
    </div>
    <div class="row" style="margin-top:10px; margin-bottom:0">
      <button id="btnTestShort">🔉 تجربة القصيرة</button>
      <button id="btnTestLong">🔊 تجربة الطويلة</button>
    </div>
  </div>

  <div class="footer">
    <div><span class="dot" id="dot"></span><span id="statusTxt">متوقف</span></div>
    <div>صفارات تنفيذية: <b id="count">0</b></div>
  </div>
</div>

<script>
/* ================= الحالة ================= */
const state = {
  interval: 60,        // الفاصل بين الصافرات الطويلة
  preAlert: 10,        // ثواني الصافرة القصيرة قبل الموعد
  remaining: 60,       // المعروض
  running: false,
  count: 0,
  volume: 0.55,
  targetTime: 0,       // توقيت الصافرة الطويلة القادمة (بساعة الصوت)
  preDone: false,      // هل انتهت صافرة هذا الدورة؟
  preHandle: null,
  mainHandle: null
};

const SHORT = { dur: 0.20, freq: 2350, trill: 48, depth: 180 };
const LONG  = { dur: 1.60, freq: 2650, trill: 36, depth: 250 };
const LOOKAHEAD = 1.0;   // نجدول الصوت قبل موعده بثانية

const $ = id => document.getElementById(id);

/* ================= الصوت ================= */
let audioCtx = null;

function ensureAudio() {
  if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
  if (audioCtx.state === 'suspended') audioCtx.resume();
  return audioCtx;
}

/* يبني صافرة مجدولة عند atTime بالضبط */
function makeWhistle(atTime, spec, type) {
  const ctx = ensureAudio();
  const peak = state.volume;
  const h = { timer: null };

  if (peak > 0.001) {
    const master = ctx.createGain();
    master.gain.setValueAtTime(0.0001, atTime);
    master.gain.linearRampToValueAtTime(peak, atTime + 0.015);
    master.gain.setValueAtTime(peak, atTime + Math.max(0.02, spec.dur - 0.08));
    master.gain.linearRampToValueAtTime(0.0001, atTime + spec.dur);
    master.connect(ctx.destination);

    const osc = ctx.createOscillator();
    osc.type = 'sine';
    osc.frequency.setValueAtTime(spec.freq, atTime);

    const lfo = ctx.createOscillator();
    lfo.type = 'sine';
    lfo.frequency.setValueAtTime(spec.trill, atTime);
    const lg = ctx.createGain();
    lg.gain.setValueAtTime(spec.depth, atTime);
    lfo.connect(lg);
    lg.connect(osc.frequency);

    osc.connect(master);
    osc.start(atTime);
    lfo.start(atTime);
    osc.stop(atTime + spec.dur + 0.05);
    lfo.stop(atTime + spec.dur + 0.05);

    h.master = master; h.osc = osc; h.lfo = lfo;
  }

  // وميض بصري متزامن مع لحظة الصوت
  const delay = Math.max(0, (atTime - ctx.currentTime) * 1000);
  h.timer = setTimeout(() => flash(type), delay);
  return h;
}

function killWhistle(h) {
  if (!h) return;
  if (h.timer) { clearTimeout(h.timer); h.timer = null; }
  if (!audioCtx) return;
  const t = audioCtx.currentTime;
  if (h.master) {
    try {
      h.master.gain.cancelScheduledValues(t);
      h.master.gain.setValueAtTime(0, t);
    } catch (e) {}
  }
  try { if (h.osc) h.osc.stop(t + 0.02); } catch (e) {}
  try { if (h.lfo) h.lfo.stop(t + 0.02); } catch (e) {}
  const m = h.master;
  if (m) setTimeout(() => { try { m.disconnect(); } catch (e) {} }, 150);
}

function flash(type) {
  const d = $('display');
  d.classList.remove('flash-short', 'flash-long');
  void d.offsetWidth;
  d.classList.add(type === 'long' ? 'flash-long' : 'flash-short');
  setTimeout(() => d.classList.remove('flash-short', 'flash-long'), 550);
}

/* ================= المنطق ================= */
function effPre() {
  if (state.preAlert <= 0) return 0;
  return Math.min(state.preAlert, Math.max(1, state.interval - 1));
}

function tick() {
  if (!state.running) return;
  const ctx = ensureAudio();
  const now = ctx.currentTime;
  let remaining = state.targetTime - now;

  /* --- لحظة الصافرة الطويلة (التنفيذية) --- */
  if (remaining <= 0) {
    if (!state.mainHandle) {
      // احتياط: إن لم تُجدول (تبويب كان مجمّداً) نشغّلها الآن
      makeWhistle(now + 0.02, LONG, 'long');
    }
    state.mainHandle = null;
    state.preHandle = null;
    state.preDone = false;
    state.count++;
    do { state.targetTime += state.interval; } while (state.targetTime - now <= 0.05);
    remaining = state.targetTime - now;
  }

  const pre = effPre();

  /* --- جدولة الصافرة القصيرة (قبل الموعد) --- */
  if (pre > 0 && !state.preDone) {
    const dt = (state.targetTime - pre) - now;
    if (dt <= 0) {
      state.preDone = true;              // فات موعدها لهذه الدورة
    } else if (dt <= LOOKAHEAD) {
      state.preHandle = makeWhistle(now + dt, SHORT, 'short');
      state.preDone = true;
    }
  }

  /* --- جدولة الصافرة الطويلة عند الموعد تماماً --- */
  if (!state.mainHandle) {
    const dt = state.targetTime - now;
    if (dt > 0 && dt <= LOOKAHEAD) {
      state.mainHandle = makeWhistle(state.targetTime, LONG, 'long');
    }
  }

  state.remaining = Math.max(0, state.targetTime - now);
  render();
}

/* ================= التحكم ================= */
function start() {
  if (state.running) return;
  ensureAudio();
  if (state.remaining <= 0.1) state.remaining = state.interval;
  state.running = true;
  state.targetTime = audioCtx.currentTime + state.remaining;
  state.preDone = false;
  state.preHandle = null;
  state.mainHandle = null;
  render();
}

function pause() {
  if (!state.running) return;
  state.remaining = Math.max(0, state.targetTime - audioCtx.currentTime);
  state.running = false;
  killWhistle(state.preHandle); state.preHandle = null;
  killWhistle(state.mainHandle); state.mainHandle = null;
  state.preDone = false;
  render();
}

function reset() {
  state.running = false;
  killWhistle(state.preHandle); state.preHandle = null;
  killWhistle(state.mainHandle); state.mainHandle = null;
  state.preDone = false;
  state.remaining = state.interval;
  state.count = 0;
  render();
}

function addTime(sec) {
  if (state.running) {
    state.targetTime += sec;
    killWhistle(state.preHandle); state.preHandle = null;
    killWhistle(state.mainHandle); state.mainHandle = null;
    state.preDone = false;
    state.remaining = Math.max(0, state.targetTime - audioCtx.currentTime);
  } else {
    state.remaining += sec;
  }
  render();
}

function setIntervalSec(sec) {
  state.interval = Math.max(5, Math.min(3600, sec));
  if (!state.running) {
    state.remaining = state.interval;
  } else {
    killWhistle(state.preHandle); state.preHandle = null;
    state.preDone = false;
  }
  render();
}

function setPreAlert(sec) {
  state.preAlert = Math.max(0, Math.min(3600, sec));
  if (state.running) {
    killWhistle(state.preHandle); state.preHandle = null;
    state.preDone = false;
  }
  render();
}

/* ================= الرسم ================= */
function fmt(sec) {
  sec = Math.max(0, Math.ceil(sec - 1e-6));
  const m = Math.floor(sec / 60), s = sec % 60;
  return String(m).padStart(2, '0') + ':' + String(s).padStart(2, '0');
}

function render() {
  const rem = Math.max(0, state.remaining);
  const pre = effPre();

  const clock = $('clock');
  clock.textContent = fmt(rem);
  let cls = '';
  if (state.running) {
    if (rem <= 1) cls = 'hot';
    else if (pre > 0 && rem <= pre) cls = 'warn';
  }
  clock.className = cls;

  const pi = $('preInfo');
  if (pre <= 0) {
    pi.textContent = 'الصافرة التحذيرية معطّلة';
    pi.className = 'pre-info off';
  } else if (rem > pre) {
    pi.textContent = '🔉 الصافرة القصيرة بعد ' + fmt(rem - pre);
    pi.className = 'pre-info';
  } else {
    pi.textContent = '🔉 تمت الصافرة القصيرة — الطويلة الآن';
    pi.className = 'pre-info done';
  }

  $('ivVal').textContent = state.interval + ' ث (' + fmt(state.interval) + ')';
  $('preVal').textContent = pre > 0 ? pre + ' ثانية' : 'معطّلة';

  document.querySelectorAll('[data-iv]').forEach(b =>
    b.classList.toggle('active', Number(b.dataset.iv) === state.interval));
  document.querySelectorAll('[data-pre]').forEach(b =>
    b.classList.toggle('active', Number(b.dataset.pre) === state.preAlert));

  $('dot').classList.toggle('on', state.running);
  $('statusTxt').textContent = state.running ? 'يعمل...' : 'متوقف';
  $('count').textContent = state.count;
  $('btnStart').disabled = state.running;
  $('btnStart').style.opacity = state.running ? .45 : 1;
}

/* ================= الأحداث ================= */
$('btnStart').onclick = start;
$('btnStop').onclick  = pause;
$('btnReset').onclick = reset;

$('ivPlus').onclick  = () => setIntervalSec(state.interval + 10);
$('ivMinus').onclick = () => setIntervalSec(state.interval - 10);
$('prePlus').onclick  = () => setPreAlert(state.preAlert + 1);
$('preMinus').onclick = () => setPreAlert(state.preAlert - 1);

document.querySelectorAll('[data-iv]').forEach(b =>
  b.onclick = () => setIntervalSec(Number(b.dataset.iv)));
document.querySelectorAll('[data-pre]').forEach(b =>
  b.onclick = () => setPreAlert(Number(b.dataset.pre)));
document.querySelectorAll('[data-add]').forEach(b =>
  b.onclick = () => addTime(Number(b.dataset.add)));

$('vol').oninput = e => {
  state.volume = e.target.value / 100;
  $('volTxt').textContent = e.target.value + '%';
};

$('btnTestShort').onclick = () => {
  ensureAudio();
  makeWhistle(audioCtx.currentTime + 0.03, SHORT, 'short');
};
$('btnTestLong').onclick = () => {
  ensureAudio();
  makeWhistle(audioCtx.currentTime + 0.03, LONG, 'long');
};

/* ================= التشغيل ================= */
setInterval(tick, 100);
render();
</script>
</body>
</html>
