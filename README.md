index.html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Morpheme Matrix Builder (Grades 3–5)</title>
  <style>
    :root{
      --bg1:#7dd3ff; --bg2:#b8ffcf; --bg3:#ffe38d;
      --card:#ffffffee; --ink:#13233f; --muted:#3b5876;
      --a:#7a5cff; --b:#ff4fb6; --c:#ff9f1c;
      --shadow: 0 18px 60px rgba(0,0,0,.16);
      --r: 18px;
    }
    *{ box-sizing:border-box; }
    body{
      margin:0;
      font-family: ui-rounded, system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
      color:var(--ink);
      background:
        radial-gradient(circle at 15% 10%, rgba(255,255,255,.9) 0 7%, transparent 9%),
        radial-gradient(circle at 85% 18%, rgba(255,255,255,.85) 0 6%, transparent 8%),
        radial-gradient(circle at 22% 84%, rgba(255,255,255,.8) 0 7%, transparent 9%),
        linear-gradient(155deg, var(--bg1), var(--bg2), var(--bg3));
      min-height:100vh;
    }
    header{ max-width: 1100px; margin: 0 auto; padding: 18px 14px 6px; }
    h1{ margin:0 0 6px; font-size: 22px; letter-spacing:.2px; }
    .subtitle{ margin:0; color: var(--muted); line-height: 1.35; font-size: 14px; font-weight: 800; }

    main{ max-width: 1100px; margin: 0 auto; padding: 12px 14px 24px; }
    .app{
      background: var(--card);
      border-radius: var(--r);
      box-shadow: var(--shadow);
      border: 2px solid rgba(19,35,63,.10);
      overflow:hidden;
    }

    .topbar{
      display:flex; gap:12px; flex-wrap:wrap;
      align-items:center; justify-content:space-between;
      padding: 14px;
      background: linear-gradient(90deg, rgba(122,92,255,.16), rgba(255,79,182,.12), rgba(255,159,28,.14));
      border-bottom: 2px dashed rgba(19,35,63,.18);
    }
    .control{ display:flex; gap:10px; align-items:center; flex-wrap:wrap; }
    label{ font-size:13px; color: var(--muted); font-weight: 1000; letter-spacing: .15px; }
    select, button{
      border-radius: 14px;
      border: 2px solid rgba(19,35,63,.16);
      background: #fff;
      color: var(--ink);
      padding: 10px 12px;
      font-size: 14px;
      font-weight: 1000;
      outline: none;
    }
    button{
      cursor:pointer;
      box-shadow: 0 6px 0 rgba(19,35,63,.10);
      transition: transform .06s ease;
    }
    button:active{ transform: translateY(1px); }
    button.primary{ border-color: rgba(122,92,255,.35); background: rgba(122,92,255,.12); }
    button.ghost{ background: rgba(255,255,255,.75); }

    .matrix{ display:grid; grid-template-columns: 1fr; gap: 12px; padding: 14px; }
    @media (min-width: 900px){ .matrix{ grid-template-columns: 1fr 1fr 1fr; } }

    .pane{
      border-radius: 16px;
      border: 2px solid rgba(19,35,63,.12);
      background: rgba(255,255,255,.80);
      padding: 12px;
      min-height: 300px;
    }
    .paneTitle{
      display:flex; align-items:center; justify-content:space-between;
      gap:10px; margin-bottom: 10px;
    }
    .paneTitle h2{ margin:0; font-size: 14px; letter-spacing:.2px; color: var(--muted); font-weight: 1100; }
    .pill{
      display:inline-block; padding: 4px 10px; border-radius: 999px;
      font-size: 12px; font-weight: 1100;
      border: 2px solid rgba(19,35,63,.12);
      background: rgba(255,159,28,.18);
    }

    .rootCard{
      text-align:center;
      padding: 16px 12px;
      border-radius: 16px;
      border: 2px solid rgba(122,92,255,.22);
      background:
        radial-gradient(circle at 20% 10%, rgba(255,79,182,.16), transparent 35%),
        radial-gradient(circle at 85% 30%, rgba(122,92,255,.18), transparent 35%),
        radial-gradient(circle at 30% 90%, rgba(255,159,28,.20), transparent 35%),
        rgba(122,92,255,.06);
      min-height: 300px;
      display:flex; flex-direction:column; justify-content:center; gap:10px;
    }
    .rootText{ font-size: 44px; font-weight: 1200; letter-spacing: .6px; }
    .rootAlt{ font-size: 13px; color: var(--muted); font-weight: 950; }
    .rootMeaning{ font-size: 16px; font-weight: 1200; }
    .tiny{ font-size: 12px; color: var(--muted); font-weight: 900; line-height: 1.35; }

    .list{
      height: 240px; overflow:auto; padding-right: 4px;
      border-radius: 14px; border: 2px dashed rgba(19,35,63,.18);
      background: rgba(255,255,255,.78);
    }
    .item{
      display:flex; justify-content:space-between; gap:10px; align-items:center;
      padding: 10px 10px; margin: 8px;
      border-radius: 14px; border: 2px solid rgba(19,35,63,.12);
      background: #fff; cursor:pointer; user-select:none;
    }
    .item strong{ font-size: 16px; font-weight: 1200; }
    .item span{ color: var(--muted); font-size: 12px; font-weight: 950; }
    .item.active{
      border-color: rgba(255,79,182,.70);
      box-shadow: 0 0 0 4px rgba(255,79,182,.16);
      background: rgba(255,79,182,.08);
    }

    .outputs{ padding: 0 14px 14px; display:grid; grid-template-columns: 1fr; gap: 10px; }
    @media (min-width: 900px){ .outputs{ grid-template-columns: 1fr 1fr; } }
    .outCard{
      border-radius: 16px;
      border: 2px solid rgba(19,35,63,.12);
      background: rgba(255,255,255,.88);
      padding: 12px;
    }
    .outLabel{ font-size: 12px; color: var(--muted); font-weight: 1100; letter-spacing:.2px; margin-bottom: 6px; }
    .outValue{ font-size: 22px; font-weight: 1250; letter-spacing:.2px; min-height: 28px; }
    .meaning{ font-size: 14px; color: var(--ink); font-weight: 950; line-height: 1.4; }

    .bubble{
      display:inline-flex; align-items:center; gap:8px;
      font-weight:1100;
      padding: 6px 10px; border-radius: 999px;
      font-size: 12px; margin-top: 10px;
      border: 2px solid rgba(19,35,63,.12);
      background: rgba(255,255,255,.65);
      color: var(--muted);
    }
    .good{
      border-color: rgba(46,204,113,.35);
      background: rgba(46,204,113,.16);
      color: #0e3b1f;
    }

    .question{ font-size: 14px; font-weight: 1200; color: var(--ink); margin: 6px 0 10px; }

    /* Hidden Word Key loader modal */
    .modalWrap{
      position: fixed; inset: 0;
      background: rgba(0,0,0,.45);
      display:none; align-items:center; justify-content:center;
      padding: 16px; z-index: 9999;
    }
    .modal{
      width: min(900px, 100%);
      background: #fff;
      border-radius: 18px;
      border: 2px solid rgba(19,35,63,.16);
      box-shadow: 0 22px 80px rgba(0,0,0,.25);
      overflow:hidden;
    }
    .modalTop{
      padding: 12px 14px;
      background: linear-gradient(90deg, rgba(122,92,255,.14), rgba(255,79,182,.12), rgba(255,159,28,.14));
      border-bottom: 2px dashed rgba(19,35,63,.18);
      display:flex; align-items:center; justify-content:space-between; gap:10px;
    }
    .modalTop strong{ font-weight: 1200; }
    .modalBody{ padding: 14px; }
    textarea{
      width: 100%;
      min-height: 160px;
      resize: vertical;
      border-radius: 14px;
      border: 2px solid rgba(19,35,63,.16);
      padding: 10px;
      font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace;
      font-size: 13px;
      outline:none;
    }
    .modalRow{
      display:flex; gap:10px; flex-wrap:wrap;
      align-items:center; justify-content:space-between;
      margin-top: 10px;
    }
    .hint{
      font-size: 12px;
      color: var(--muted);
      font-weight: 900;
      line-height: 1.35;
      margin-top: 6px;
    }
  </style>
</head>
<body>
<header>
  <h1>Morpheme Matrix Builder</h1>
  <p class="subtitle">Pick a matrix (one spelling per matrix), then build a word. You’ll see a real definition if it exists — or a possible definition if it doesn’t.</p>
</header>

<main>
  <div class="app" role="application" aria-label="Morpheme Matrix Builder">
    <div class="topbar">
      <div class="control">
        <label for="matrixSelect">Matrix:</label>
        <select id="matrixSelect" aria-label="Choose a matrix"></select>
      </div>
      <div class="control">
        <button id="clearBtn" class="ghost">Clear</button>
        <button id="randomBtn" class="primary">Random Real Word</button>
      </div>
    </div>

    <div class="matrix">
      <section class="pane" aria-label="Prefixes">
        <div class="paneTitle">
          <h2>Prefixes</h2>
          <span class="pill" id="prefCount">0 options</span>
        </div>
        <div class="list" id="prefixList" tabindex="0" aria-label="Prefix list"></div>
        <div class="tiny">Click again to turn a selection off.</div>
      </section>

      <section class="rootCard" aria-label="Base or root">
        <div class="rootText" id="rootText">—</div>
        <div class="rootAlt" id="rootAlt"></div>
        <div class="rootMeaning" id="rootMeaning">—</div>
        <div class="tiny" id="rootMeta"></div>
        <div id="statusBubble"></div>
      </section>

      <section class="pane" aria-label="Suffixes">
        <div class="paneTitle">
          <h2>Suffixes</h2>
          <span class="pill" id="sufCount">0 options</span>
        </div>
        <div class="list" id="suffixList" tabindex="0" aria-label="Suffix list"></div>
        <div class="tiny">Pick any suffix — then decide if it’s real or “word try.”</div>
      </section>
    </div>

    <div class="outputs">
      <div class="outCard" aria-label="Word sum">
        <div class="outLabel">WORD SUM</div>
        <div class="outValue" id="wordSum">—</div>
        <div class="tiny" id="partMeanings">—</div>
      </div>

      <div class="outCard" aria-label="Word and definition">
        <div class="outLabel">WORD + DEFINITION</div>
        <div class="question">Is this a real word or a fake word? How do you know?</div>
        <div class="outValue" id="finalWord">—</div>
        <div class="meaning" id="finalDefinition">Build a combo to see a definition.</div>
        <div id="comboBubble"></div>
        <div class="hint" id="howToKnowHint">
          Clues: Have you heard/seen it before? Does a dictionary recognize it? Does it make sense in a sentence?
        </div>
      </div>
    </div>
  </div>
</main>

<!-- Hidden Word Key Loader (Ctrl/Cmd + Shift + L) -->
<div class="modalWrap" id="modalWrap" aria-hidden="true">
  <div class="modal" role="dialog" aria-modal="true" aria-label="Load Word Key list">
    <div class="modalTop">
      <strong id="modalTitle">Load Word Key list</strong>
      <button id="closeModalBtn" class="ghost">Close</button>
    </div>
    <div class="modalBody">
      <div class="tiny">
        Paste comma-separated real words for THIS matrix (from the PDF Word Key).
      </div>
      <textarea id="wordListBox" placeholder="Example: reform, reforms, reformed, reforming..."></textarea>
      <div class="modalRow">
        <div class="tiny" id="modalInfo"></div>
        <div class="control">
          <button id="resetMatrixBtn" class="ghost">Reset</button>
          <button id="saveMatrixBtn" class="primary">Save</button>
        </div>
      </div>
      <div class="tiny" style="margin-top:10px;">
        Saved on this device/browser only (local storage).
      </div>
    </div>
  </div>
</div>

<script>
/* =========================================================
   DATA (1 spelling per matrix)
   Latin roots + Greek forms list comes from the PDF chart. :contentReference[oaicite:4]{index=4}
   Word Key is intended as the “correctly assembled words” source. :contentReference[oaicite:5]{index=5}
   ========================================================= */

const NONE = { text:"—", value:"", meaning:"(none)" };

// Prefix/Suffix charts are also in the PDF. :contentReference[oaicite:6]{index=6}
const PREFIXES = [
  ["in","in/into/toward"], ["im","in/into/toward"], ["un","not/opposite"],
  ["mis","bad/wrong"], ["dis","not/apart"], ["re","again/back"],
  ["de","down/away"], ["pre","before"], ["en","put into/on"], ["em","put into/on"],
  ["sub","below/under"], ["inter","between"]
];

const SUFFIXES = [
  ["s","plural / verb -s"], ["es","plural / verb -es"], ["ed","past tense"],
  ["ing","happening now"], ["ly","in the manner of"], ["er","someone who"], ["or","someone who"],
  ["ion","act/state of"], ["sion","act/state of"], ["tion","act/state of"], ["ation","act/state of"],
  ["able","can be"], ["ible","can be"], ["al","relating to"], ["ial","relating to"],
  ["y","marked by"], ["ive","causing/making"]
];

function P(p){ return { text:p[0], value:p[0], meaning:p[1] }; }
function S(s){ return { text:s[0], value:s[0], meaning:s[1] }; }

const LATIN = [
  // one spelling per matrix (your rule)
  mkMatrix("Latin", "form", "to shape"),
  mkMatrix("Latin", "port", "to carry"),
  mkMatrix("Latin", "rupt", "to break or burst"),
  mkMatrix("Latin", "tract", "to draw or pull"),

  mkMatrix("Latin", "scrib", "to write"),
  mkMatrix("Latin", "script", "to write"),

  mkMatrix("Latin", "spect", "to see, watch, or observe"),
  mkMatrix("Latin", "struct", "to build"),

  mkMatrix("Latin", "flect", "to bend or curve"),
  mkMatrix("Latin", "flex", "to bend or curve"),

  mkMatrix("Latin", "dict", "to say or tell"),
  mkMatrix("Latin", "fer", "to bear or yield"),

  mkMatrix("Latin", "mit", "to send"),
  mkMatrix("Latin", "miss", "to send"),

  mkMatrix("Latin", "duce", "to lead"),
  mkMatrix("Latin", "duct", "to lead"),

  mkMatrix("Latin", "vers", "to turn"),
  mkMatrix("Latin", "vert", "to turn"),

  mkMatrix("Latin", "fact", "to make or do"),
  mkMatrix("Latin", "fect", "to make or do"),
  mkMatrix("Latin", "fict", "to make or do"),

  mkMatrix("Latin", "tend", "to stretch or strain"),
  mkMatrix("Latin", "tens", "to stretch or strain"),
  mkMatrix("Latin", "tent", "to stretch or strain"),

  mkMatrix("Latin", "ceipt", "to take or catch"),
  mkMatrix("Latin", "ceive", "to take or catch"),
  mkMatrix("Latin", "cept", "to take or catch"),

  mkMatrix("Latin", "tain", "to hold"),
  mkMatrix("Latin", "ten", "to hold"),
  mkMatrix("Latin", "tin", "to hold"),

  mkMatrix("Latin", "pos", "to put in place or set"),
  mkMatrix("Latin", "pound", "to put in place or set")
];

const GREEK = [
  mkMatrix("Greek", "phon", "sound"),
  mkMatrix("Greek", "phono", "sound"),

  mkMatrix("Greek", "photo", "light"),
  mkMatrix("Greek", "metro", "city or measure"),

  mkMatrix("Greek", "gram", "written or drawn"),
  mkMatrix("Greek", "graph", "written or drawn"),

  mkMatrix("Greek", "dem", "people"),
  mkMatrix("Greek", "demo", "people"),

  mkMatrix("Greek", "meter", "measure"),
  mkMatrix("Greek", "metr", "measure"),

  mkMatrix("Greek", "geo", "earth"),
  mkMatrix("Greek", "tele", "distant"),
  mkMatrix("Greek", "techn", "skill or art"),

  mkMatrix("Greek", "bio", "life"),

  mkMatrix("Greek", "chron", "time"),
  mkMatrix("Greek", "chrono", "time"),

  mkMatrix("Greek", "micro", "small"),
  mkMatrix("Greek", "psych", "mind or soul"),

  mkMatrix("Greek", "hydra", "water"),
  mkMatrix("Greek", "hydro", "water"),

  mkMatrix("Greek", "auto", "self"),

  mkMatrix("Greek", "therm", "heat or hot"),
  mkMatrix("Greek", "thermo", "heat or hot"),

  mkMatrix("Greek", "logy", "study of"),
  mkMatrix("Greek", "ology", "study of"),

  mkMatrix("Greek", "cracy", "rule"),
  mkMatrix("Greek", "crat", "rule"),

  mkMatrix("Greek", "sphere", "circle"),
  mkMatrix("Greek", "scope", "watch or see")
];

// Combine for dropdown
const MATRICES = [...LATIN, ...GREEK];

function mkMatrix(family, base, baseMeaning){
  return {
    id: `${family.toLowerCase()}_${base}`,
    family,
    base,
    baseMeaning,
    // Start from common charts; you will load the real-word list per matrix from the Word Key.
    // The app will automatically narrow to only affixes that appear in your loaded list.
    prefixes: [NONE, ...PREFIXES.map(P)],
    suffixes: [NONE, ...SUFFIXES.map(S)]
  };
}

/* =========================================================
   WORD LIST STORAGE (per-matrix Word Key paste)
   ========================================================= */
const LS_KEY = "morph_playground_wordkeys_v1";

function loadAllWordLists(){
  try{
    const raw = localStorage.getItem(LS_KEY);
    if(!raw) return {};
    const obj = JSON.parse(raw);
    if(!obj || typeof obj !== "object") return {};
    for(const k of Object.keys(obj)){
      obj[k] = (obj[k] || [])
        .map(w => String(w).trim())
        .filter(Boolean)
        .map(w => w.toLowerCase());
    }
    return obj;
  }catch{ return {}; }
}
function saveAllWordLists(obj){
  localStorage.setItem(LS_KEY, JSON.stringify(obj));
}

/* =========================================================
   PARSING: build prefix/suffix combos from Word Key list
   (Best-effort; avoids declaring “wrong”)
   ========================================================= */
function norm(s){ return String(s||"").toLowerCase(); }

function buildComboData(matrix, wordList){
  const combos = [];
  const wordSet = new Set(wordList.map(norm));

  const prefCands = matrix.prefixes.map(p=>norm(p.value)).filter(Boolean);
  const sufCands  = matrix.suffixes.map(s=>norm(s.value)).filter(Boolean);

  for(const w0 of wordList){
    const w = norm(w0);
    if(!w) continue;

    // Try to find (prefix, suffix) that leave a middle containing the base.
    let best = null;

    const prefOpts = ["", ...prefCands];
    const sufOpts = ["", ...sufCands];

    for(const p of prefOpts){
      if(p && !w.startsWith(p)) continue;
      const afterP = w.slice(p.length);

      for(const s of sufOpts){
        if(s && !afterP.endsWith(s)) continue;
        const mid = afterP.slice(0, afterP.length - s.length);

        // We allow base to appear in the remaining mid (helps with Greek combining forms)
        if(mid.includes(norm(matrix.base))){
          const score = (p.length*2) + (s.length*2) + (mid.length);
          if(!best || score > best.score){
            best = { pref:p, suf:s, score };
          }
        }
      }
    }

    if(best){
      combos.push({ pref: best.pref, suf: best.suf, word: w });
    }
  }

  return { combos, wordSet };
}

function allowedSetFromCombos(combos, key){
  const set = new Set();
  for(const c of combos){
    set.add(c[key] || "");
  }
  set.add("");
  return set;
}

/* =========================================================
   DEFINITIONS
   - Real word: fetch from dictionary API
   - If not found: generate plausible definition from morphemes
   ========================================================= */
async function fetchDefinition(word){
  const w = norm(word);
  if(!w || w === "—") return null;

  // Free dictionary API (no key). If it fails, we fall back.
  // Note: network failures are possible on school filters.
  try{
    const res = await fetch(`https://api.dictionaryapi.dev/api/v2/entries/en/${encodeURIComponent(w)}`);
    if(!res.ok) return null;
    const data = await res.json();
    const first = Array.isArray(data) ? data[0] : null;
    const meanings = first?.meanings;
    const def = meanings?.[0]?.definitions?.[0]?.definition;
    if(def && typeof def === "string") return def;
    return null;
  }catch{
    return null;
  }
}

function generatedDefinition(prefixMeaning, baseMeaning, suffixMeaning){
  const p = prefixMeaning ? prefixMeaning : "";
  const r = baseMeaning || "";
  const s = suffixMeaning ? suffixMeaning : "";

  // Make it read like a kid-friendly dictionary definition.
  if(p && s) return `A made-up word that could mean: “${p} ${r}” and that is “${s}.”`;
  if(p) return `A made-up word that could mean: “${p} ${r}.”`;
  if(s) return `A made-up word that could mean: “${r}” that is “${s}.”`;
  return `A made-up word that could mean: “${r}.”`;
}

/* =========================================================
   UI
   ========================================================= */
const matrixSelect = document.getElementById("matrixSelect");
const prefixListEl = document.getElementById("prefixList");
const suffixListEl = document.getElementById("suffixList");
const prefCountEl  = document.getElementById("prefCount");
const sufCountEl   = document.getElementById("sufCount");

const rootTextEl   = document.getElementById("rootText");
const rootAltEl    = document.getElementById("rootAlt");
const rootMeaningEl= document.getElementById("rootMeaning");
const rootMetaEl   = document.getElementById("rootMeta");
const statusBubble = document.getElementById("statusBubble");

const wordSumEl    = document.getElementById("wordSum");
const partMeaningsEl = document.getElementById("partMeanings");
const finalWordEl  = document.getElementById("finalWord");
const finalDefEl   = document.getElementById("finalDefinition");
const comboBubbleEl= document.getElementById("comboBubble");

const clearBtn = document.getElementById("clearBtn");
const randomBtn = document.getElementById("randomBtn");

// Modal
const modalWrap = document.getElementById("modalWrap");
const closeModalBtn = document.getElementById("closeModalBtn");
const saveMatrixBtn = document.getElementById("saveMatrixBtn");
const resetMatrixBtn = document.getElementById("resetMatrixBtn");
const wordListBox = document.getElementById("wordListBox");
const modalTitle = document.getElementById("modalTitle");
const modalInfo = document.getElementById("modalInfo");

let allWordLists = loadAllWordLists();

let current = MATRICES[0];
let chosenPrefix = "";
let chosenSuffix = "";

let comboData = buildComboData(current, allWordLists[current.id] || []);

let defReqToken = 0;

function renderMatrixSelect(){
  matrixSelect.innerHTML = "";
  MATRICES.forEach(m=>{
    const opt = document.createElement("option");
    opt.value = m.id;
    opt.textContent = `${m.family}: ${m.base} — ${m.baseMeaning}`;
    matrixSelect.appendChild(opt);
  });
}

function setStatus(){
  const hasLoaded = (allWordLists[current.id] && allWordLists[current.id].length);
  statusBubble.innerHTML = hasLoaded
    ? `<div class="bubble good">✅ Word Key list loaded for this matrix</div>`
    : `<div class="bubble">⭐ Press Ctrl/Cmd + Shift + L to paste the Word Key list for THIS matrix</div>`;
}

function renderRoot(){
  rootTextEl.textContent = current.base;
  rootAltEl.textContent  = `${current.family} matrix (one spelling)`;
  rootMeaningEl.textContent = current.baseMeaning;
  rootMetaEl.textContent = `Build: prefix + ${current.base} + suffix`;
  setStatus();
}

function mkItem(obj, isActive){
  const div = document.createElement("div");
  div.className = "item" + (isActive ? " active" : "");
  div.innerHTML = `<strong>${obj.text}</strong><span>${obj.meaning}</span>`;
  return div;
}

function renderLists(){
  prefixListEl.innerHTML = "";
  suffixListEl.innerHTML = "";

  const combos = comboData.combos;

  // Only show affixes that appear in at least one real word for this matrix (from your pasted Word Key list)
  const allowedPrefixes = allowedSetFromCombos(combos, "pref");
  const allowedSuffixes = allowedSetFromCombos(combos, "suf");

  const prefixes = current.prefixes.filter(p => allowedPrefixes.has(p.value));
  const suffixes = current.suffixes.filter(s => allowedSuffixes.has(s.value));

  prefixes.forEach(p=>{
    const div = mkItem(p, p.value === chosenPrefix);
    div.addEventListener("click", ()=>{
      chosenPrefix = (chosenPrefix === p.value) ? "" : p.value;
      renderAll();
    });
    prefixListEl.appendChild(div);
  });

  suffixes.forEach(s=>{
    const div = mkItem(s, s.value === chosenSuffix);
    div.addEventListener("click", ()=>{
      chosenSuffix = (chosenSuffix === s.value) ? "" : s.value;
      renderAll();
    });
    suffixListEl.appendChild(div);
  });

  prefCountEl.textContent = `${prefixes.length} options`;
  sufCountEl.textContent  = `${suffixes.length} options`;
}

function wordSum(){
  const parts = [];
  if(chosenPrefix) parts.push(chosenPrefix);
  parts.push(current.base);
  if(chosenSuffix) parts.push(chosenSuffix);
  return parts.join(" + ");
}

function partMeanings(){
  const p = chosenPrefix ? current.prefixes.find(x=>x.value===chosenPrefix)?.meaning : "";
  const s = chosenSuffix ? current.suffixes.find(x=>x.value===chosenSuffix)?.meaning : "";
  const parts = [];
  if(chosenPrefix) parts.push(`(${chosenPrefix}-) ${p || "prefix meaning"}`);
  parts.push(`${current.base} = ${current.baseMeaning}`);
  if(chosenSuffix) parts.push(`(-${chosenSuffix}) ${s || "suffix meaning"}`);
  return parts.join(" • ");
}

function wordCandidate(){
  // We show the student-built attempt (not a verdict).
  return `${chosenPrefix || ""}${current.base}${chosenSuffix || ""}` || "—";
}

function isInWordKey(word){
  return comboData.wordSet.has(norm(word));
}

async function renderDefinition(){
  const token = ++defReqToken;

  const candidate = wordCandidate();
  finalWordEl.textContent = (chosenPrefix || chosenSuffix) ? candidate : "—";

  if(!chosenPrefix && !chosenSuffix){
    finalDefEl.textContent = "Build a combo to see a definition.";
    comboBubbleEl.innerHTML = "";
    return;
  }

  const pMean = chosenPrefix ? (current.prefixes.find(x=>x.value===chosenPrefix)?.meaning || "") : "";
  const sMean = chosenSuffix ? (current.suffixes.find(x=>x.value===chosenSuffix)?.meaning || "") : "";

  // First: try real dictionary definition
  const realDef = await fetchDefinition(candidate);

  if(token !== defReqToken) return; // ignore stale responses

  if(realDef){
    finalDefEl.textContent = realDef;
    comboBubbleEl.innerHTML = isInWordKey(candidate)
      ? `<div class="bubble good">Found in dictionary + appears in your Word Key list</div>`
      : `<div class="bubble good">Found in dictionary</div>`;
  } else {
    // Fake/unknown: generate plausible definition
    finalDefEl.textContent = generatedDefinition(pMean, current.baseMeaning, sMean);
    comboBubbleEl.innerHTML = isInWordKey(candidate)
      ? `<div class="bubble good">Appears in your Word Key list (dictionary may be blocked or missing)</div>`
      : `<div class="bubble">Not found in dictionary — try it in a sentence!</div>`;
  }
}

function renderOutputs(){
  wordSumEl.textContent = wordSum();
  partMeaningsEl.textContent = partMeanings();
  // Definition is async
  renderDefinition();
}

function renderAll(){
  renderRoot();
  renderLists();
  renderOutputs();
}

function setCurrentMatrix(id){
  current = MATRICES.find(m=>m.id===id) || MATRICES[0];
  chosenPrefix = "";
  chosenSuffix = "";
  allWordLists = loadAllWordLists();
  comboData = buildComboData(current, allWordLists[current.id] || []);
  renderAll();
}

/* =========================================================
   Actions
   ========================================================= */
clearBtn.addEventListener("click", ()=>{
  chosenPrefix = "";
  chosenSuffix = "";
  renderAll();
});

randomBtn.addEventListener("click", ()=>{
  allWordLists = loadAllWordLists();
  const list = allWordLists[current.id] || [];
  if(!list.length){
    alert("No Word Key list loaded for this matrix yet. Press Ctrl/Cmd + Shift + L to paste it.");
    return;
  }
  const pick = list[Math.floor(Math.random() * list.length)];
  // Try to infer prefix/suffix to highlight something:
  comboData = buildComboData(current, list);
  const parsed = comboData.combos.find(c => c.word === norm(pick));
  if(parsed){
    chosenPrefix = parsed.pref || "";
    chosenSuffix = parsed.suf || "";
  }else{
    chosenPrefix = "";
    chosenSuffix = "";
  }
  renderAll();
});

matrixSelect.addEventListener("change", ()=> setCurrentMatrix(matrixSelect.value));

/* =========================================================
   Hidden Loader: Ctrl/Cmd + Shift + L
   ========================================================= */
function openModal(){
  modalWrap.style.display = "flex";
  modalWrap.setAttribute("aria-hidden","false");

  allWordLists = loadAllWordLists();
  const existing = allWordLists[current.id] || [];
  modalTitle.textContent = `Load Word Key list — ${current.family}: ${current.base}`;
  modalInfo.textContent = existing.length ? `Currently saved: ${existing.length} words` : `No saved list yet.`;
  wordListBox.value = existing.length ? existing.join(", ") : "";
  wordListBox.focus();
}
function closeModal(){
  modalWrap.style.display = "none";
  modalWrap.setAttribute("aria-hidden","true");
  wordListBox.value = "";
}

closeModalBtn.addEventListener("click", closeModal);
modalWrap.addEventListener("click", (e)=>{ if(e.target === modalWrap) closeModal(); });

saveMatrixBtn.addEventListener("click", ()=>{
  const text = wordListBox.value || "";
  const words = text.split(",").map(w => w.trim()).filter(Boolean).map(w => w.toLowerCase());

  const stored = loadAllWordLists();
  stored[current.id] = words;
  saveAllWordLists(stored);

  allWordLists = loadAllWordLists();
  comboData = buildComboData(current, allWordLists[current.id] || []);
  closeModal();
  renderAll();
});

resetMatrixBtn.addEventListener("click", ()=>{
  const stored = loadAllWordLists();
  delete stored[current.id];
  saveAllWordLists(stored);

  allWordLists = loadAllWordLists();
  comboData = buildComboData(current, allWordLists[current.id] || []);
  closeModal();
  renderAll();
});

document.addEventListener("keydown", (e)=>{
  const key = e.key.toLowerCase();
  const isMac = navigator.platform.toLowerCase().includes("mac");
  const ctrlOrCmd = isMac ? e.metaKey : e.ctrlKey;

  if(ctrlOrCmd && e.shiftKey && key === "l"){
    e.preventDefault();
    openModal();
  }
  if(key === "escape" && modalWrap.style.display === "flex"){
    closeModal();
  }
});

/* =========================================================
   Init
   ========================================================= */
renderMatrixSelect();
setCurrentMatrix(MATRICES[0].id);
matrixSelect.value = MATRICES[0].id;
</script>
</body>
</html>
