<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no, viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<title>Plinky Companion</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Boldonse&family=Red+Hat+Mono:wght@400;600;700&display=swap" rel="stylesheet">
<style>
/* ===== Design tokens (Plinky//12 series: ink / paper / signal red) ===== */
:root {
  --ink: #141414;
  --ink-2: #1e1e1e;
  --ink-3: #2a2a2a;
  --paper: #E6E6E3;
  --paper-dim: #9a9a96;
  --red: #FF3A1F;
  --hair: rgba(230,230,227,.18);
  --mono: "Red Hat Mono", ui-monospace, Menlo, monospace;
  --display: "Boldonse", "Arial Black", sans-serif;
}
* { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
html, body { margin: 0; height: 100%; background: var(--ink); color: var(--paper);
  font-family: var(--mono); overflow: hidden; touch-action: none;
  -webkit-user-select: none; user-select: none; -webkit-touch-callout: none; }
button, select { font-family: var(--mono); }

/* ===== Header ===== */
header { height: 52px; display: flex; align-items: center; gap: 10px;
  padding: 0 max(16px, env(safe-area-inset-left)); border-bottom: 1px solid var(--hair); }
.tag { background: var(--paper); color: var(--ink); font-weight: 700; font-size: 12px;
  padding: 4px 8px; letter-spacing: .08em; white-space: nowrap; }
.tabs { display: flex; gap: 0; margin-left: 6px; }
.tab { background: transparent; color: var(--paper-dim); border: 1px solid var(--hair);
  padding: 8px 16px; font-size: 13px; font-weight: 700; letter-spacing: .1em; }
.tab.on { background: var(--paper); color: var(--ink); border-color: var(--paper); }
.spacer { flex: 1; }
.midi { display: flex; align-items: center; gap: 8px; font-size: 11px; color: var(--paper-dim); }
.midi select { background: var(--ink-2); color: var(--paper); border: 1px solid var(--hair);
  font-size: 11px; padding: 5px 4px; max-width: 150px; border-radius: 0; }
.dot { width: 9px; height: 9px; border-radius: 50%; background: var(--ink-3); border: 1px solid var(--paper-dim); }
.dot.ok { background: var(--paper); border-color: var(--paper); }
.dot.hit { background: var(--red); border-color: var(--red); }

main { position: absolute; top: 52px; left: 0; right: 0; bottom: 0; }
.page { position: absolute; inset: 0; display: none; }
.page.on { display: block; }

/* ===== Sound page ===== */
#sound { display: none; padding: 14px max(16px, env(safe-area-inset-right)) 14px max(16px, env(safe-area-inset-left)); }
#sound.on { display: grid; grid-template-columns: 1fr 320px; gap: 16px; }
.faders { display: grid; grid-template-columns: repeat(11, minmax(0,1fr)); grid-template-rows: 1fr 1fr; gap: 14px 6px; min-height: 0; }
.fader { display: flex; flex-direction: column; min-width: 0; min-height: 0; }
.grp { font-size: 9px; height: 14px; color: var(--red); letter-spacing: .12em; font-weight: 700;
  border-top: 1px solid var(--hair); padding-top: 2px; white-space: nowrap; overflow: visible; }
.grp.first { border-top-color: var(--paper); }
.track { position: relative; flex: 1; margin: 4px 0; background: var(--ink-2);
  border: 1px solid var(--hair); overflow: hidden; touch-action: none; }
.fill { position: absolute; left: 0; right: 0; background: var(--paper); }
.fader.touch .fill { background: var(--red); }
.mid { position: absolute; left: 0; right: 0; top: 50%; height: 1px; background: var(--paper-dim); opacity: .5; }
.val { font-size: 12px; font-weight: 700; text-align: center; }
.lbl { font-size: 9px; text-align: center; color: var(--paper-dim); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; letter-spacing: .04em; }

.side { display: flex; flex-direction: column; gap: 12px; min-height: 0; }
.sect { font-size: 10px; letter-spacing: .14em; color: var(--paper-dim); display: flex; gap: 8px; align-items: center; }
.sect b { background: var(--paper); color: var(--ink); padding: 2px 6px; }
#xy { position: relative; width: 100%; aspect-ratio: 1; background: var(--ink-2); border: 1px solid var(--hair); touch-action: none; overflow: hidden; }
#xy .h, #xy .v { position: absolute; background: var(--paper); opacity: .7; }
#xy .h { left: 0; right: 0; height: 1px; }
#xy .v { top: 0; bottom: 0; width: 1px; }
#xy .knob { position: absolute; width: 26px; height: 26px; margin: -13px 0 0 -13px; border: 2px solid var(--paper); }
#xy.touch .knob { border-color: var(--red); background: rgba(255,58,31,.25); }
#xy .read { position: absolute; left: 6px; bottom: 4px; font-size: 10px; color: var(--paper-dim); }
.presets { display: grid; grid-template-columns: repeat(4, 1fr); gap: 6px; }
.btn { background: var(--ink-2); color: var(--paper); border: 1px solid var(--hair);
  font-size: 12px; font-weight: 700; padding: 12px 0; letter-spacing: .08em; border-radius: 0; }
.btn.has { border-color: var(--paper-dim); }
.btn.cur { background: var(--paper); color: var(--ink); }
.btn.arm { background: var(--red); border-color: var(--red); color: var(--ink); }
.row { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 6px; }
.hint { font-size: 10px; color: var(--paper-dim); line-height: 1.5; }

/* ===== Visual page ===== */
#stage { position: absolute; inset: 0; width: 100%; height: 100%; touch-action: none; }
.ov { position: absolute; top: 12px; display: flex; gap: 6px; z-index: 2; }
.ov.l { left: max(12px, env(safe-area-inset-left)); }
.ov.r { right: max(12px, env(safe-area-inset-right)); }
.ov .btn { padding: 8px 12px; font-size: 11px; background: rgba(20,20,20,.75); }
.ov .btn.cur { background: var(--red); border-color: var(--red); color: var(--ink); }
body.hide header { display: none; }
body.hide main { top: 0; }
body.hide .ov { opacity: 0; }
body.hide .ov.r { opacity: .25; }
body.hide .ov.r .btn:not(#bHide) { display: none; }

@media (max-width: 900px) {
  #sound.on { grid-template-columns: 1fr; overflow-y: auto; }
  .faders { grid-template-columns: repeat(6, minmax(0,1fr)); grid-template-rows: none; grid-auto-rows: 210px; }
  .midi select { max-width: 90px; }
}
</style>
</head>
<body>
<header>
  <span class="tag">PLINKY//12</span>
  <div class="tabs">
    <button class="tab on" data-p="sound">SOUND</button>
    <button class="tab" data-p="visual">VISUAL</button>
  </div>
  <div class="spacer"></div>
  <div class="midi">
    <span class="dot" id="dot"></span>
    <span id="status">NO MIDI</span>
    <select id="selIn" title="MIDI in"></select>
    <select id="selOut" title="MIDI out"></select>
    <select id="selCh" title="Channel"></select>
  </div>
</header>

<main>
  <section class="page on" id="sound">
    <div class="faders" id="faders"></div>
    <div class="side">
      <div class="sect"><b>XY</b> CC 1 / CC 2</div>
      <div id="xy"><div class="h"></div><div class="v"></div><div class="knob"></div><div class="read">X 064 · Y 064</div></div>
      <div class="sect"><b>PRESETS</b> TAP = LOAD</div>
      <div class="presets" id="presets"></div>
      <div class="row">
        <button class="btn" id="bSave">SAVE</button>
        <button class="btn" id="bInit">INIT</button>
        <button class="btn" id="bSend">SEND ALL</button>
      </div>
      <div class="hint">Drag faders up/down · double-tap resets · Plinky sends CC feedback only while transport runs.</div>
    </div>
  </section>

  <section class="page" id="visual">
    <canvas id="stage"></canvas>
    <div class="ov l" id="scenes"></div>
    <div class="ov r">
      <button class="btn" id="bDemo">DEMO</button>
      <button class="btn" id="bHide">HIDE UI</button>
    </div>
  </section>
</main>

<script>
"use strict";
// =====================================================================
// CONFIG
// =====================================================================
// Voice params: index = CC offset. Low value CC = 16 + index.
// init: value sent by INIT (adjust to taste / to Plinky defaults).
// bi: bipolar (center = 64).
const PARAMS = [
  // row 1
  { i: 0,  n: "HP CUT",   g: "FILTER", init: 0 },
  { i: 1,  n: "LP CUT",   g: "FILTER", init: 100 },
  { i: 2,  n: "RESO",     g: "FILTER", init: 24 },
  { i: 3,  n: "ATTACK",   g: "ENV",    init: 0 },
  { i: 4,  n: "DECAY",    g: "ENV",    init: 50 },
  { i: 5,  n: "SUSTAIN",  g: "ENV",    init: 110 },
  { i: 6,  n: "RELEASE",  g: "ENV",    init: 30 },
  { i: 7,  n: "GLIDE",    g: "PITCH",  init: 0 },
  { i: 8,  n: "PITCH",    g: "PITCH",  init: 0 },   // neutral at bottom (mid = +1 oct)
  { i: 9,  n: "OCTAVE",   g: "PITCH",  init: 0 },
  { i: 15, n: "TOAD",     g: "TOAD",   init: 0 },
  // row 2
  { i: 10, n: "NOISE|SUB", g: "TONE",  init: 64, bi: true },
  { i: 11, n: "SAT|FOLD",  g: "TONE",  init: 64, bi: true },
  { i: 12, n: "STEREO",   g: "SPACE",  init: 64 },
  { i: 13, n: "DELAY",    g: "SPACE",  init: 0 },
  { i: 14, n: "REVERB",   g: "SPACE",  init: 30 },
  { i: 20, n: "CHORUS",   g: "SPACE",  init: 64, bi: true },
  { i: 17, n: "STRETCH",  g: "SAMPLE", init: 0 },
  { i: 18, n: "START",    g: "SAMPLE", init: 0 },
  { i: 19, n: "LENGTH",   g: "SAMPLE", init: 127 },
  { i: 21, n: "SIDECHAIN", g: "OUT",   init: 64, bi: true },
  { i: 16, n: "VOLUME",   g: "OUT",    init: 100 },
];
const CC_BASE = 16, CC_X = 1, CC_Y = 2;
const STORE_KEY = "plinkyCompanion.v1";

// =====================================================================
// STATE
// =====================================================================
const S = {
  vals: new Array(22).fill(0),
  x: 64, y: 64,
  touching: new Set(),      // param indices currently under a finger
  presets: new Array(8).fill(null),
  curPreset: -1,
  saveArmed: false,
  chan: 1,
  out: null,
  inSel: "all",
  pending: new Map(),       // cc -> value, flushed once per frame
};
PARAMS.forEach(p => S.vals[p.i] = p.init);

function load() {
  try {
    const d = JSON.parse(localStorage.getItem(STORE_KEY) || "null");
    if (d && Array.isArray(d.presets)) S.presets = d.presets.slice(0, 8);
    if (d && d.chan) S.chan = d.chan;
  } catch (e) {}
}
function persist() {
  try { localStorage.setItem(STORE_KEY, JSON.stringify({ presets: S.presets, chan: S.chan })); } catch (e) {}
}
load();

// =====================================================================
// MIDI
// =====================================================================
let midi = null;
const dot = document.getElementById("dot");
const statusEl = document.getElementById("status");
const selIn = document.getElementById("selIn");
const selOut = document.getElementById("selOut");
const selCh = document.getElementById("selCh");

for (let c = 1; c <= 16; c++) selCh.add(new Option("CH " + c, c));
selCh.value = S.chan;
selCh.onchange = () => { S.chan = +selCh.value; persist(); };

function sendCC(cc, v) {
  if (!S.out) return;
  try { S.out.send([0xB0 | (S.chan - 1), cc & 127, v & 127]); } catch (e) {}
}
function queueCC(cc, v) { S.pending.set(cc, v); }
function flushCC() {
  if (!S.pending.size) return;
  S.pending.forEach((v, cc) => sendCC(cc, v));
  S.pending.clear();
}

function refreshPorts() {
  if (!midi) return;
  const prevIn = selIn.value || "all", prevOut = selOut.value;
  selIn.innerHTML = ""; selOut.innerHTML = "";
  selIn.add(new Option("IN: ALL", "all"));
  let auto = null;
  midi.inputs.forEach(p => {
    selIn.add(new Option("IN: " + p.name, p.id));
    p.onmidimessage = onMidi;
  });
  selOut.add(new Option("OUT: —", ""));
  midi.outputs.forEach(p => {
    selOut.add(new Option("OUT: " + p.name, p.id));
    if (!auto && /plinky/i.test(p.name)) auto = p.id;
  });
  selIn.value = [...selIn.options].some(o => o.value === prevIn) ? prevIn : "all";
  const outId = prevOut && midi.outputs.get(prevOut) ? prevOut : (auto || (midi.outputs.size ? midi.outputs.values().next().value.id : ""));
  selOut.value = outId;
  S.out = outId ? midi.outputs.get(outId) : null;
  S.inSel = selIn.value;
  const n = midi.inputs.size;
  dot.className = "dot" + (n ? " ok" : "");
  statusEl.textContent = n ? (S.out ? "LINKED" : "IN ONLY") : "NO DEVICE";
}
selIn.onchange = () => { S.inSel = selIn.value; };
selOut.onchange = () => { S.out = selOut.value ? midi.outputs.get(selOut.value) : null; refreshPorts(); };

if (navigator.requestMIDIAccess) {
  navigator.requestMIDIAccess({ sysex: false }).then(m => {
    midi = m; m.onstatechange = refreshPorts; refreshPorts();
  }).catch(() => { statusEl.textContent = "MIDI DENIED"; setTimeout(() => setDemo(true)); });
} else {
  statusEl.textContent = "NO WEB MIDI";
  setTimeout(() => setDemo(true));   // deferred: demo UI is defined further down
}

let hitT = 0;
function onMidi(e) {
  if (S.inSel !== "all" && e.target.id !== S.inSel) return;
  const d = e.data, st = d[0];
  hitT = performance.now();
  if (st === 0xF8) { V.clockTick(); return; }
  if (st === 0xFA) { V.clockStart(); return; }
  if (st === 0xFC) { V.clockStop(); return; }
  const type = st & 0xF0, ch = (st & 0x0F) + 1;
  if (type === 0x90 && d[2] > 0) V.noteOn(d[1], d[2], ch);
  else if (type === 0x80 || (type === 0x90 && d[2] === 0)) V.noteOff(d[1], ch);
  else if (type === 0xB0) {
    if (ch !== S.chan) return;
    const cc = d[1], v = d[2];
    if (cc >= CC_BASE && cc < CC_BASE + 22) {
      const i = cc - CC_BASE;
      if (!S.touching.has(i)) { S.vals[i] = v; renderFader(i); }
    } else if (cc === CC_X && !xyTouch) { S.x = v; renderXY(); }
    else if (cc === CC_Y && !xyTouch) { S.y = v; renderXY(); }
  } else if (type === 0xC0) {
    setScene(d[1] % SCENES.length);
  }
}

// =====================================================================
// SOUND PAGE
// =====================================================================
const fadersEl = document.getElementById("faders");
const faderEls = {};
let lastGroup = null, col = 0;
PARAMS.forEach((p, k) => {
  const rowStart = k % 11 === 0;
  const showGrp = rowStart || p.g !== lastGroup;
  lastGroup = p.g;
  const el = document.createElement("div");
  el.className = "fader";
  el.innerHTML =
    `<div class="grp${showGrp ? " first" : ""}">${showGrp ? p.g : ""}</div>` +
    `<div class="track"><div class="fill"></div>${p.bi ? '<div class="mid"></div>' : ""}</div>` +
    `<div class="val">000</div><div class="lbl">${p.n}</div>`;
  fadersEl.appendChild(el);
  faderEls[p.i] = { el, fill: el.querySelector(".fill"), val: el.querySelector(".val"), track: el.querySelector(".track"), p };
  attachFader(p, el.querySelector(".track"));
});

function renderFader(i) {
  const f = faderEls[i]; if (!f) return;
  const v = S.vals[i], t = v / 127;
  if (f.p.bi) {
    const c = 0.5;
    const top = Math.min(t, c), bot = Math.max(t, c);
    f.fill.style.bottom = (top * 100) + "%";
    f.fill.style.height = Math.max((bot - top) * 100, 0.8) + "%";
    const b = v - 64;
    f.val.textContent = (b > 0 ? "+" : b < 0 ? "−" : "±") + String(Math.abs(b)).padStart(2, "0");
  } else {
    f.fill.style.bottom = "0";
    f.fill.style.height = (t * 100) + "%";
    f.val.textContent = String(v).padStart(3, "0");
  }
}

function attachFader(p, track) {
  let startY = 0, startV = 0, id = null, lastDown = 0;
  track.addEventListener("pointerdown", e => {
    e.preventDefault();
    const now = performance.now();
    if (now - lastDown < 300) { setParam(p.i, p.init); lastDown = 0; return; }
    lastDown = now;
    id = e.pointerId; track.setPointerCapture(id);
    startY = e.clientY; startV = S.vals[p.i];
    S.touching.add(p.i); faderEls[p.i].el.classList.add("touch");
  });
  track.addEventListener("pointermove", e => {
    if (e.pointerId !== id) return;
    const h = track.clientHeight || 1;
    const v = Math.round(startV + (startY - e.clientY) / h * 127);
    setParam(p.i, v);
  });
  const end = e => {
    if (e.pointerId !== id) return;
    id = null; S.touching.delete(p.i); faderEls[p.i].el.classList.remove("touch");
  };
  track.addEventListener("pointerup", end);
  track.addEventListener("pointercancel", end);
}

function setParam(i, v) {
  v = Math.max(0, Math.min(127, v | 0));
  if (S.vals[i] === v) return;
  S.vals[i] = v; renderFader(i); queueCC(CC_BASE + i, v);
  markDirty();
}

// XY pad
const xyEl = document.getElementById("xy");
const xyH = xyEl.querySelector(".h"), xyV = xyEl.querySelector(".v"), xyK = xyEl.querySelector(".knob"), xyR = xyEl.querySelector(".read");
let xyTouch = false;
function renderXY() {
  const px = S.x / 127 * 100, py = (1 - S.y / 127) * 100;
  xyV.style.left = px + "%"; xyH.style.top = py + "%";
  xyK.style.left = px + "%"; xyK.style.top = py + "%";
  xyR.textContent = `X ${String(S.x).padStart(3, "0")} · Y ${String(S.y).padStart(3, "0")}`;
}
function xyFrom(e) {
  const r = xyEl.getBoundingClientRect();
  const x = Math.round(Math.max(0, Math.min(1, (e.clientX - r.left) / r.width)) * 127);
  const y = Math.round(Math.max(0, Math.min(1, 1 - (e.clientY - r.top) / r.height)) * 127);
  if (x !== S.x) { S.x = x; queueCC(CC_X, x); }
  if (y !== S.y) { S.y = y; queueCC(CC_Y, y); }
  renderXY(); markDirty();
}
xyEl.addEventListener("pointerdown", e => { e.preventDefault(); xyTouch = true; xyEl.classList.add("touch"); xyEl.setPointerCapture(e.pointerId); xyFrom(e); });
xyEl.addEventListener("pointermove", e => { if (xyTouch) xyFrom(e); });
const xyEnd = () => { xyTouch = false; xyEl.classList.remove("touch"); };
xyEl.addEventListener("pointerup", xyEnd);
xyEl.addEventListener("pointercancel", xyEnd);

// Presets
const presetsEl = document.getElementById("presets");
const bSave = document.getElementById("bSave");
const presetBtns = [];
for (let k = 0; k < 8; k++) {
  const b = document.createElement("button");
  b.className = "btn"; b.textContent = String(k + 1).padStart(2, "0");
  b.onclick = () => {
    if (S.saveArmed) {
      S.presets[k] = { vals: S.vals.slice(), x: S.x, y: S.y };
      S.curPreset = k; S.saveArmed = false; persist();
    } else if (S.presets[k]) {
      const pr = S.presets[k];
      pr.vals.forEach((v, i) => { S.vals[i] = v; renderFader(i); });
      S.x = pr.x; S.y = pr.y; renderXY();
      sendAll(); S.curPreset = k;
    }
    renderPresets();
  };
  presetsEl.appendChild(b); presetBtns.push(b);
}
function renderPresets() {
  presetBtns.forEach((b, k) => {
    b.className = "btn" + (S.presets[k] ? " has" : "") + (k === S.curPreset ? " cur" : "") + (S.saveArmed ? " arm" : "");
  });
  bSave.className = "btn" + (S.saveArmed ? " arm" : "");
}
function markDirty() { if (S.curPreset >= 0) { S.curPreset = -1; renderPresets(); } }
function sendAll() {
  PARAMS.forEach(p => queueCC(CC_BASE + p.i, S.vals[p.i]));
  queueCC(CC_X, S.x); queueCC(CC_Y, S.y);
}
bSave.onclick = () => { S.saveArmed = !S.saveArmed; renderPresets(); };
document.getElementById("bInit").onclick = () => {
  PARAMS.forEach(p => { S.vals[p.i] = p.init; renderFader(p.i); });
  S.x = 64; S.y = 64; renderXY(); sendAll(); markDirty();
};
document.getElementById("bSend").onclick = sendAll;

PARAMS.forEach(p => renderFader(p.i));
renderXY(); renderPresets();

// Tabs
document.querySelectorAll(".tab").forEach(t => t.onclick = () => {
  document.querySelectorAll(".tab").forEach(x => x.classList.toggle("on", x === t));
  document.querySelectorAll(".page").forEach(pg => pg.classList.toggle("on", pg.id === t.dataset.p));
  resize();
});

// =====================================================================
// VISUAL ENGINE
// =====================================================================
const cv = document.getElementById("stage");
const ctx = cv.getContext("2d");
let W = 0, H = 0, DPR = 1;
function resize() {
  DPR = Math.min(window.devicePixelRatio || 1, 2);
  const r = cv.getBoundingClientRect();
  W = Math.max(1, r.width); H = Math.max(1, r.height);
  cv.width = Math.round(W * DPR); cv.height = Math.round(H * DPR);
  ctx.setTransform(DPR, 0, 0, DPR, 0, 0);
  SCENES.forEach(s => s.reset && s.reset());
}
window.addEventListener("resize", resize);

const INK = "#141414", PAPER = "#E6E6E3", RED = "#FF3A1F";
const NOTE_NAMES = ["C", "C#", "D", "D#", "E", "F", "F#", "G", "G#", "A", "A#", "B"];
const noteName = n => NOTE_NAMES[n % 12] + (Math.floor(n / 12) - 1);
const pv = i => S.vals[i] / 127;                 // unipolar 0..1
const bp = i => (S.vals[i] - 64) / 64;           // bipolar -1..1
const clamp = (v, a, b) => Math.max(a, Math.min(b, v));
const rnd = (a, b) => a + Math.random() * (b - a);

// Envelope times follow the Sound page (attack / release faders)
const attackTime = () => 0.004 + pv(3) * pv(3) * 1.5;
const releaseTime = () => 0.06 + pv(6) * pv(6) * 3.0;

const V = {
  notes: new Map(),     // key "ch:n" -> {n, v, ch, t0, rel}
  log: [],              // recent events for readouts
  echoes: [],           // scheduled delay echoes
  beat: 0, beatT: -10, ticks: 0, bpm: 0, lastBeatMs: 0, running: false,
  now: 0,
  noteOn(n, vel, ch, echo) {
    const v = vel / 127, t = this.now;
    if (!echo) {
      this.notes.set(ch + ":" + n, { n, v, ch, t0: t, rel: null });
      this.log.unshift({ n, v, ch, t }); if (this.log.length > 9) this.log.pop();
      // delay send -> visual echoes
      const reps = Math.floor(pv(13) * 4.99);
      const dt = this.bpm ? 30 / this.bpm : 0.25;   // eighth note
      for (let k = 1; k <= reps; k++) this.echoes.push({ n, v: v * Math.pow(0.6, k), ch, at: t + dt * k });
    }
    SCENES[cur].spawn({ n, v, ch, t, echo: !!echo });
  },
  noteOff(n, ch) {
    const k = ch + ":" + n, o = this.notes.get(k);
    if (o && o.rel === null) o.rel = this.now;
  },
  level(o) {
    const t = this.now;
    let a = clamp((t - o.t0) / attackTime(), 0, 1);
    if (o.rel !== null) a *= clamp(1 - (t - o.rel) / releaseTime(), 0, 1);
    return a * o.v;
  },
  clockTick() {
    this.ticks++;
    if (this.ticks % 24 === 0) {
      const ms = performance.now();
      if (this.lastBeatMs) this.bpm = this.bpm ? this.bpm * 0.8 + (60000 / (ms - this.lastBeatMs)) * 0.2 : 60000 / (ms - this.lastBeatMs);
      this.lastBeatMs = ms; this.beatNow();
    }
  },
  clockStart() { this.ticks = 0; this.running = true; this.beatNow(); },
  clockStop() { this.running = false; },
  beatNow() { this.beat++; this.beatT = this.now; SCENES[cur].onBeat && SCENES[cur].onBeat(); },
  update() {
    const t = this.now;
    for (let k = this.echoes.length - 1; k >= 0; k--) {
      const e = this.echoes[k];
      if (t >= e.at) { this.echoes.splice(k, 1); this.noteOn(e.n, e.v * 127, e.ch, true); }
    }
    this.notes.forEach((o, k) => { if (o.rel !== null && t - o.rel > releaseTime()) this.notes.delete(k); });
  },
};

// pitch -> position helpers
const pitchX = n => 0.06 + ((n - 24) / 72) * 0.88;     // C1..C7 across
const gridCell = n => ({ c: n % 16, r: 15 - ((n >> 3) % 16) });

// ---------------------------------------------------------------------
// SCENE 01 — GRID (Wipeout poster: paper, ink hairlines, red events)
// ---------------------------------------------------------------------
const sceneGrid = {
  name: "GRID",
  cells: new Float32Array(256), hot: new Float32Array(256), rings: [], last: -1, lastT: -10,
  reset() { this.rings = []; },
  spawn(e) {
    const { c, r } = gridCell(e.n), k = r * 16 + c;
    this.cells[k] = Math.max(this.cells[k], e.v);
    if (!e.echo) { this.hot[k] = 1; this.last = k; this.lastT = e.t; }
    this.rings.push({ k, t: e.t, v: e.v, echo: e.echo });
  },
  draw(dt) {
    const t = V.now;
    ctx.fillStyle = PAPER; ctx.fillRect(0, 0, W, H);
    const nameH = Math.min(H * 0.14, W * 0.1);
    const gy = 84;                                     // below the scene buttons
    const S0 = Math.max(120, Math.min(H - gy - nameH - 70, W * 0.52));
    const gx = Math.max(34, W * 0.04), cell = S0 / 16;

    // chassis: cut-corner double hairline
    ctx.strokeStyle = INK; ctx.lineWidth = 1;
    for (const o of [0, 6]) {
      const x0 = gx - 10 - o, y0 = gy - 10 - o, x1 = gx + S0 + 10 + o, y1 = gy + S0 + 10 + o, cc = 14 + o;
      ctx.beginPath();
      ctx.moveTo(x0 + cc, y0); ctx.lineTo(x1, y0); ctx.lineTo(x1, y1 - cc); ctx.lineTo(x1 - cc, y1);
      ctx.lineTo(x0, y1); ctx.lineTo(x0, y0 + cc); ctx.closePath(); ctx.stroke();
    }

    // held notes keep their LED lit with envelope level
    const held = new Float32Array(256);
    V.notes.forEach(o => { const { c, r } = gridCell(o.n); const k = r * 16 + c; held[k] = Math.max(held[k], V.level(o)); });

    const decay = Math.exp(-dt / (0.08 + releaseTime() * 0.5));
    const rad = cell * (0.28 + pv(1) * 0.08);   // LP cutoff opens the LEDs
    for (let k = 0; k < 256; k++) {
      this.cells[k] *= decay; this.hot[k] *= Math.exp(-dt / 0.12);
      const x = gx + (k % 16 + 0.5) * cell, y = gy + ((k >> 4) + 0.5) * cell;
      const lv = Math.max(this.cells[k], held[k]);
      ctx.beginPath(); ctx.arc(x, y, rad, 0, Math.PI * 2);
      if (lv > 0.02) {
        ctx.fillStyle = this.hot[k] > 0.3 ? RED : `rgba(20,20,20,${clamp(lv * 1.2, 0.08, 1)})`;
        ctx.fill();
      } else {
        ctx.strokeStyle = "rgba(20,20,20,.28)"; ctx.stroke();
      }
    }

    // expanding red rings (resonance = sharper, thicker)
    for (let i = this.rings.length - 1; i >= 0; i--) {
      const g = this.rings[i], a = (t - g.t) / (0.5 + releaseTime() * 0.4);
      if (a >= 1) { this.rings.splice(i, 1); continue; }
      const x = gx + (g.k % 16 + 0.5) * cell, y = gy + ((g.k >> 4) + 0.5) * cell;
      ctx.beginPath(); ctx.arc(x, y, rad + a * cell * (2 + g.v * 4), 0, Math.PI * 2);
      ctx.strokeStyle = g.echo ? `rgba(20,20,20,${(1 - a) * 0.5})` : `rgba(255,58,31,${1 - a})`;
      ctx.lineWidth = 1 + pv(2) * 3 * (1 - a); ctx.stroke();
    }
    ctx.lineWidth = 1;

    // XY crosshair over the grid
    const cx = gx + S.x / 127 * S0, cy = gy + (1 - S.y / 127) * S0;
    ctx.strokeStyle = "rgba(20,20,20,.55)";
    ctx.beginPath(); ctx.moveTo(gx, cy); ctx.lineTo(gx + S0, cy); ctx.moveTo(cx, gy); ctx.lineTo(cx, gy + S0); ctx.stroke();
    xBox(cx, cy, 9);

    // info column
    const ix = gx + S0 + 48, iw = W - ix - 24;
    if (iw > 160) {
      let y = gy;
      tag(ix, y, "01 EVENTS"); y += 30;
      ctx.font = "600 13px 'Red Hat Mono', monospace"; ctx.textBaseline = "top";
      V.log.forEach((e, k) => {
        const age = t - e.t;
        ctx.fillStyle = k === 0 && age < 0.15 ? RED : `rgba(20,20,20,${clamp(1 - k * 0.09, 0.25, 1)})`;
        ctx.fillText(`${noteName(e.n).padEnd(4)} ${String(Math.round(e.v * 127)).padStart(3, "0")}  CH${String(e.ch).padStart(2, "0")}`, ix, y + k * 18);
      });
      // kinked route from last LED to the log
      if (this.last >= 0 && t - this.lastT < 1.2) {
        const a = 1 - (t - this.lastT) / 1.2;
        const sx = gx + (this.last % 16 + 0.5) * cell, sy = gy + ((this.last >> 4) + 0.5) * cell;
        const ex = ix - 8, ey = gy + 37;
        const kx = gx + S0 + 22;
        ctx.strokeStyle = `rgba(255,58,31,${a})`;
        ctx.beginPath(); ctx.moveTo(sx, sy); ctx.lineTo(kx - Math.abs(ey - sy) * 0, sy);
        ctx.lineTo(kx, sy); ctx.lineTo(kx + Math.min(14, Math.abs(ey - sy)), ey); ctx.lineTo(ex, ey); ctx.stroke();
        ctx.fillStyle = `rgba(255,58,31,${a})`; ctx.fillRect(ex - 3, ey - 3, 6, 6);
      }
      y += 9 * 18 + 22;
      tag(ix, y, "02 VOICE"); y += 32;
      const gauges = [[1, "LP"], [2, "RES"], [3, "ATK"], [6, "REL"], [13, "DLY"], [14, "REV"]];
      const gw = Math.min(iw - 60, 260);
      gauges.forEach(([i, lbl], k) => {
        const yy = y + k * 20;
        ctx.fillStyle = INK; ctx.font = "600 11px 'Red Hat Mono', monospace";
        ctx.fillText(lbl, ix, yy);
        const bx = ix + 40, segs = 24;
        for (let s = 0; s < segs; s++) {
          const on = s / segs < pv(i);
          ctx.fillStyle = on ? (S.touching.has(i) ? RED : INK) : "rgba(20,20,20,.14)";
          ctx.fillRect(bx + s * (gw / segs), yy + 1, gw / segs - 2, 10);
        }
      });
      y += 6 * 20 + 18;
      // beat marker + bpm
      const bAge = t - V.beatT;
      ctx.fillStyle = bAge < 0.12 ? RED : INK;
      ctx.fillRect(ix, y, 12, 12);
      ctx.fillStyle = INK; ctx.font = "600 12px 'Red Hat Mono', monospace";
      ctx.fillText(V.bpm ? `${Math.round(V.bpm)} BPM` : "NO CLOCK", ix + 22, y);
      ctx.fillText(`BEAT ${String(V.beat % 1000).padStart(3, "0")}`, ix + 120, y);
    }

    // giant name across the bottom
    ctx.fillStyle = INK; ctx.textBaseline = "alphabetic";
    let fs = nameH * 0.9;
    ctx.font = `${fs}px Boldonse, 'Arial Black', sans-serif`;
    const tw = ctx.measureText("PLINKY//12").width;
    if (tw > W - 48) { fs *= (W - 48) / tw; ctx.font = `${fs}px Boldonse, 'Arial Black', sans-serif`; }
    ctx.fillText("PLINKY//12", 24, H - 18);
  },
};

function tag(x, y, txt) {
  ctx.font = "700 11px 'Red Hat Mono', monospace"; ctx.textBaseline = "top";
  const w = ctx.measureText(txt).width + 12;
  ctx.fillStyle = INK; ctx.fillRect(x, y, w, 18);
  ctx.fillStyle = PAPER; ctx.fillText(txt, x + 6, y + 3);
}
function xBox(x, y, r) {
  ctx.strokeStyle = INK; ctx.lineWidth = 1;
  ctx.strokeRect(x - r, y - r, r * 2, r * 2);
  ctx.beginPath(); ctx.moveTo(x - r, y - r); ctx.lineTo(x + r, y + r); ctx.moveTo(x + r, y - r); ctx.lineTo(x - r, y + r); ctx.stroke();
}

// ---------------------------------------------------------------------
// SCENE 02 — BLOOM (dark, additive particles, reverb = trails)
// ---------------------------------------------------------------------
const sceneBloom = {
  name: "BLOOM",
  parts: [], rings: [], cleared: false,
  reset() { this.parts = []; this.rings = []; this.cleared = false; },
  pos(n, v) {
    const ox = (S.x - 64) / 64 * 0.08, oy = (S.y - 64) / 64 * 0.08;
    return { x: (pitchX(n) + ox) * W, y: (0.78 - v * 0.5 - oy) * H };
  },
  spawn(e) {
    const p = this.pos(e.n, e.v);
    const count = Math.round((8 + e.v * 46) * (e.echo ? 0.4 : 1));
    for (let k = 0; k < count; k++) {
      const a = Math.random() * Math.PI * 2, sp = rnd(20, 60 + e.v * 260);
      this.parts.push({ x: p.x, y: p.y, vx: Math.cos(a) * sp, vy: Math.sin(a) * sp, life: 1, dec: rnd(0.5, 1.4) / (0.4 + releaseTime()), red: Math.random() < 0.35 + pv(2) * 0.5 });
    }
    this.rings.push({ x: p.x, y: p.y, t: e.t, v: e.v, echo: e.echo });
    if (this.parts.length > 2500) this.parts.splice(0, this.parts.length - 2500);
  },
  onBeat() { this.beatFlash = 1; },
  beatFlash: 0,
  draw(dt) {
    const t = V.now;
    // trails: more reverb = longer persistence
    ctx.globalCompositeOperation = "source-over";
    ctx.fillStyle = `rgba(10,10,11,${this.cleared ? 0.06 + (1 - pv(14)) * 0.32 : 1})`;
    ctx.fillRect(0, 0, W, H); this.cleared = true;

    ctx.globalCompositeOperation = "lighter";
    const jit = Math.max(0, bp(11)) * 120;      // fold -> jitter
    const size = 1 + pv(1) * 2.6;               // LP cutoff -> particle size
    for (let i = this.parts.length - 1; i >= 0; i--) {
      const q = this.parts[i];
      q.life -= q.dec * dt;
      if (q.life <= 0) { this.parts.splice(i, 1); continue; }
      q.vx *= Math.exp(-dt * 1.6); q.vy = q.vy * Math.exp(-dt * 1.6) + 14 * dt;
      q.x += (q.vx + rnd(-jit, jit)) * dt; q.y += (q.vy + rnd(-jit, jit)) * dt;
      ctx.fillStyle = q.red ? `rgba(255,58,31,${q.life})` : `rgba(230,230,227,${q.life * 0.8})`;
      const s = size * (0.5 + q.life);
      ctx.fillRect(q.x - s / 2, q.y - s / 2, s, s);
    }
    // held notes: glowing orbs
    V.notes.forEach(o => {
      const lv = V.level(o); if (lv < 0.01) return;
      const p = this.pos(o.n, o.v), r = 18 + lv * 70 * (0.6 + pv(1));
      const g = ctx.createRadialGradient(p.x, p.y, 0, p.x, p.y, r);
      g.addColorStop(0, `rgba(255,90,60,${lv * 0.55})`);
      g.addColorStop(0.4, `rgba(255,58,31,${lv * 0.18})`);
      g.addColorStop(1, "rgba(255,58,31,0)");
      ctx.fillStyle = g; ctx.beginPath(); ctx.arc(p.x, p.y, r, 0, Math.PI * 2); ctx.fill();
    });
    // rings
    for (let i = this.rings.length - 1; i >= 0; i--) {
      const g = this.rings[i], a = (t - g.t) / (0.7 + releaseTime() * 0.5);
      if (a >= 1) { this.rings.splice(i, 1); continue; }
      ctx.strokeStyle = g.echo ? `rgba(230,230,227,${(1 - a) * 0.3})` : `rgba(255,58,31,${(1 - a) * 0.9})`;
      ctx.lineWidth = 0.5 + pv(2) * 3 * (1 - a);
      ctx.beginPath(); ctx.arc(g.x, g.y, 6 + a * (60 + g.v * 220), 0, Math.PI * 2); ctx.stroke();
    }
    ctx.globalCompositeOperation = "source-over"; ctx.lineWidth = 1;
    // beat: thin frame pulse
    this.beatFlash *= Math.exp(-dt * 8);
    if (this.beatFlash > 0.02) {
      ctx.strokeStyle = `rgba(230,230,227,${this.beatFlash * 0.5})`;
      ctx.strokeRect(10.5, 10.5, W - 21, H - 21);
    }
  },
};

// ---------------------------------------------------------------------
// SCENE 03 — BARCODE (black/white data, strobe on hard hits)
// ---------------------------------------------------------------------
const sceneBarcode = {
  name: "BARCODE",
  bars: [], flashT: -10, lastFlash: -10, scroll: 0,
  reset() { this.bars = []; },
  spawn(e) {
    const cx = pitchX(e.n) * W;
    const n = Math.round(3 + e.v * 14) * (e.echo ? 0.5 : 1);
    const spread = 30 + e.v * 160;
    for (let k = 0; k < n; k++) {
      const w = Math.random() < 0.2 ? rnd(8, 26) : rnd(1, 5);
      this.bars.push({ x: cx + rnd(-spread, spread), w, life: 1, dec: 1 / (0.15 + releaseTime() * 0.6), band: Math.random() < 0.3 });
    }
    // strobe on hard non-echo hits, rate-limited (max ~3 per second)
    if (!e.echo && e.v > 0.85 && e.t - this.lastFlash > 0.35) { this.flashT = e.t; this.lastFlash = e.t; }
    if (this.bars.length > 900) this.bars.splice(0, this.bars.length - 900);
  },
  draw(dt) {
    const t = V.now;
    const inv = t - this.flashT < 0.07;
    const bg = inv ? PAPER : "#000", fg = inv ? "#000" : PAPER;
    ctx.fillStyle = bg; ctx.fillRect(0, 0, W, H);
    const midY = H * 0.5, bandH = H * 0.18;
    for (let i = this.bars.length - 1; i >= 0; i--) {
      const b = this.bars[i];
      b.life -= b.dec * dt;
      if (b.life <= 0) { this.bars.splice(i, 1); continue; }
      ctx.globalAlpha = b.life;
      ctx.fillStyle = fg;
      if (b.band) ctx.fillRect(b.x, midY - bandH / 2, b.w, bandH);
      else ctx.fillRect(b.x, 0, b.w, H);
    }
    ctx.globalAlpha = 1;
    // held notes: hairlines with level
    V.notes.forEach(o => {
      const lv = V.level(o); if (lv < 0.01) return;
      ctx.fillStyle = `rgba(255,58,31,${lv})`;
      ctx.fillRect(pitchX(o.n) * W, 0, 1.5, H);
    });
    // data line with scrolling ticks (speed follows clock)
    const speed = V.bpm ? V.bpm * 1.2 : 80;
    this.scroll = (this.scroll + dt * speed) % 40;
    ctx.fillStyle = fg; ctx.fillRect(0, midY, W, 1);
    for (let x = -this.scroll; x < W; x += 40) ctx.fillRect(x, midY - 4, 1, 9);
    // fold -> noise scanlines
    const nz = Math.round(Math.max(0, bp(11)) * 60 + pv(2) * 10);
    for (let k = 0; k < nz; k++) {
      ctx.globalAlpha = Math.random() * 0.6;
      ctx.fillRect(0, Math.random() * H, W, 1);
    }
    ctx.globalAlpha = 1;
    // readouts
    ctx.font = "600 11px 'Red Hat Mono', monospace"; ctx.textBaseline = "top";
    ctx.fillStyle = fg;
    const e = V.log[0];
    ctx.fillText(e ? `${noteName(e.n)} / ${String(Math.round(e.v * 127)).padStart(3, "0")} / CH${e.ch}` : "WAITING FOR SIGNAL", 24, H - 60);
    ctx.fillText(`LP ${String(S.vals[1]).padStart(3, "0")}  RES ${String(S.vals[2]).padStart(3, "0")}  FOLD ${String(S.vals[11] - 64).padStart(3, " ")}  XY ${S.x}.${S.y}`, 24, H - 42);
    const bAge = t - V.beatT;
    ctx.fillStyle = bAge < 0.1 ? RED : fg;
    ctx.fillRect(W - 40, H - 58, 14, 14);
  },
};

const SCENES = [sceneGrid, sceneBloom, sceneBarcode];
let cur = 0;
const scenesEl = document.getElementById("scenes");
const sceneBtns = SCENES.map((s, k) => {
  const b = document.createElement("button");
  b.className = "btn"; b.textContent = String(k + 1).padStart(2, "0") + " " + s.name;
  b.onclick = () => setScene(k);
  scenesEl.appendChild(b); return b;
});
function setScene(k) {
  cur = k; SCENES[k].reset && SCENES[k].reset();
  sceneBtns.forEach((b, i) => b.classList.toggle("cur", i === k));
}
setScene(0);

// Tap on the stage = visual-only note (handy for previews)
const tapNotes = new Map();
cv.addEventListener("pointerdown", e => {
  const r = cv.getBoundingClientRect();
  const n = clamp(Math.round(24 + ((e.clientX - r.left) / r.width - 0.06) / 0.88 * 72), 0, 127);
  const v = clamp(Math.round((1 - (e.clientY - r.top) / r.height) * 127), 20, 127);
  tapNotes.set(e.pointerId, n); V.noteOn(n, v, 16);
});
const tapEnd = e => { if (tapNotes.has(e.pointerId)) { V.noteOff(tapNotes.get(e.pointerId), 16); tapNotes.delete(e.pointerId); } };
cv.addEventListener("pointerup", tapEnd);
cv.addEventListener("pointercancel", tapEnd);

// Demo mode: random pentatonic notes + fake clock (visual only, sends nothing)
let demo = false, demoNext = 0, demoTick = 0;
const demoOff = [];
const bDemo = document.getElementById("bDemo");
function setDemo(on) { demo = on; bDemo.classList.toggle("cur", on); if (!on) V.bpm = 0; }
bDemo.onclick = () => setDemo(!demo);
function runDemo() {
  if (!demo) return;
  const t = V.now, step = 0.25;      // 120 BPM eighths
  if (t >= demoNext) {
    demoNext = t + step; demoTick++;
    if (demoTick % 2 === 0) { V.bpm = 120; V.beatNow(); }
    if (Math.random() < 0.65) {
      const sc = [0, 3, 5, 7, 10];
      const n = 48 + sc[(Math.random() * 5) | 0] + 12 * ((Math.random() * 3) | 0);
      const v = Math.random() < 0.15 ? 127 : Math.round(rnd(40, 110));
      V.noteOn(n, v, 1); demoOff.push({ n, at: t + rnd(0.1, 0.6) });
    }
  }
  for (let k = demoOff.length - 1; k >= 0; k--) if (t >= demoOff[k].at) { V.noteOff(demoOff[k].n, 1); demoOff.splice(k, 1); }
}

// Hide UI (fullscreen visuals)
document.getElementById("bHide").onclick = () => {
  document.body.classList.toggle("hide");
  document.getElementById("bHide").textContent = document.body.classList.contains("hide") ? "SHOW UI" : "HIDE UI";
  resize();
};

// =====================================================================
// MAIN LOOP
// =====================================================================
let lastMs = performance.now();
function frame(ms) {
  const dt = Math.min(0.05, (ms - lastMs) / 1000); lastMs = ms;
  V.now = ms / 1000;
  flushCC();
  runDemo();
  V.update();
  if (document.getElementById("visual").classList.contains("on")) {
    if (cv.width < 2) resize();
    SCENES[cur].draw(dt);
  }
  if (midi && midi.inputs.size) dot.className = "dot " + (ms - hitT < 80 ? "hit" : "ok");
  requestAnimationFrame(frame);
}
resize();
requestAnimationFrame(frame);
</script>
</body>
</html>
