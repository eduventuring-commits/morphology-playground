index.html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Morpheme Matrix Playground (Grades 3–5)</title>
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
    .subtitle{ margin:0; color: var(--muted); line-height: 1.35; font-size: 14px; font-weight: 850; }

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
  </style>
</head>
<body>
<header>
  <h1>Morpheme Matrix Playground</h1>
  <p class="subtitle">Pick a matrix (from the PDF), then choose a left part + the base + a right part. Then decide: <b>Is it a real word or a fake word?</b></p>
</header>

<main>
  <div class="app" role="application" aria-label="Morpheme Matrix Playground">
    <div class="topbar">
      <div class="control">
        <label for="matrixSelect">Matrix:</label>
        <select id="matrixSelect" aria-label="Choose a matrix"></select>
      </div>
      <div class="control">
        <button id="clearBtn" class="ghost">Clear</button>
        <button id="randomBtn" class="primary">Random Combo</button>
      </div>
    </div>

    <div class="matrix">
      <section class="pane" aria-label="Left side parts">
        <div class="paneTitle">
          <h2 id="leftTitle">Prefixes / Left Forms</h2>
          <span class="pill" id="leftCount">0 options</span>
        </div>
        <div class="list" id="leftList" tabindex="0" aria-label="Left list"></div>
        <div class="tiny">Click again to turn a selection off.</div>
      </section>

      <section class="rootCard" aria-label="Base or middle form">
        <div class="rootText" id="rootText">—</div>
        <div class="rootAlt" id="rootAlt"></div>
        <div class="rootMeaning" id="rootMeaning">—</div>
        <div class="tiny" id="rootMeta"></div>
        <div id="statusBubble"></div>
      </section>

      <section class="pane" aria-label="Right side parts">
        <div class="paneTitle">
          <h2 id="rightTitle">Suffixes / Right Parts</h2>
          <span class="pill" id="rightCount">0 options</span>
        </div>
        <div class="list" id="rightList" tabindex="0" aria-label="Right list"></div>
        <div class="tiny">Pick a right part too — you can do BOTH.</div>
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
      </div>
    </div>
  </div>
</main>

<script>
const NONE = { text:"—", value:"", meaning:"(none)" };

function part(text, meaning=""){
  const v = (text||"").trim();
  return { text: v || "—", value: v, meaning: meaning || "" };
}

const MEAN = {
  // Common prefixes
  in: "in, into, or toward",
  im: "in, into, or toward",
  un: "not or opposite of",
  mis: "bad or wrong",
  dis: "not or apart",
  re: "again or back",
  de: "down or away from",
  pre: "before or earlier",
  en: "put into or onto",
  em: "put into or onto",
  sub: "below or under",
  inter: "between",
  con: "together",
  com: "together",
  pro: "forward",
  per: "through",
  trans: "across",
  sus: "under",
  ex: "out",
  e: "out",

  // Common suffix meanings
  "s": "plural noun / singular verb",
  "s/es": "plural noun / singular verb",
  "es": "plural noun / singular verb",
  "ed": "past tense",
  "ing": "happening now",
  "ly": "in the manner of",
  "er": "someone who",
  "or": "someone who",
  "ion": "act/state of",
  "tion": "act/state of",
  "sion": "act/state of",
  "ation": "act/state of",
  "ion, ation": "act/state of",
  "able": "can be",
  "ible": "can be",
  "able, ible": "can be",
  "al": "relating to",
  "ive": "causing/making",
  "ic": "relating to",
  "ical": "relating to",
  "ist": "one who",
  "y": "subject or science",
  "eme": "unit"
};

function withMeanings(list){
  return [NONE, ...list.map(x => part(x, MEAN[x] || ""))];
}

// --- Base matrix templates (some have center with multiple options)
const BASE_MATRICES = [
  // Latin (unchanged)
  { id:"latin_01_form", label:"Latin 1: form (to shape)", family:"Latin", base:"form", baseMeaning:"to shape", baseType:"free",
    leftTitle:"Prefixes", rightTitle:"Suffixes",
    left: withMeanings(["in","re","de"]),
    right: withMeanings(["s","ed","ing","er","ation","al"])
  },
  { id:"latin_02_port", label:"Latin 2: port (to carry)", family:"Latin", base:"port", baseMeaning:"to carry", baseType:"free",
    leftTitle:"Prefixes", rightTitle:"Suffixes",
    left: withMeanings(["im","re","de"]),
    right: withMeanings(["s","ed","ing","er","ion, ation","able","al"])
  },
  { id:"latin_03_rupt", label:"Latin 3: rupt (to break or burst)", family:"Latin", base:"rupt", baseMeaning:"to break or burst", baseType:"bound",
    leftTitle:"Prefixes", rightTitle:"Suffixes",
    left: withMeanings(["dis","inter","e"]),
    right: withMeanings(["s","ed","ing","er","tion","ible","ive"])
  },
  { id:"latin_04_tract", label:"Latin 4: tract (to draw or pull)", family:"Latin", base:"tract", baseMeaning:"to draw or pull", baseType:"free",
    leftTitle:"Prefixes", rightTitle:"Suffixes",
    left: withMeanings(["dis","re","de","sub"]),
    right: withMeanings(["s","ed","ing","or","ion","able, ible"])
  },
  { id:"latin_05_scrib_script", label:"Latin 5: scrib / script (to write)", family:"Latin", base:"scrib", baseMeaning:"to write", baseType:"bound & free (script is free*)",
    leftTitle:"Prefixes", rightTitle:"Suffixes",
    left: withMeanings(["in","de","pre","sub"]),
    right: withMeanings(["s","ed","ing","er","ion","able"])
  },
  { id:"latin_06_spect", label:"Latin 6: spect (to see, watch, observe)", family:"Latin", base:"spect", baseMeaning:"to see, watch, or observe", baseType:"bound",
    leftTitle:"Prefixes", rightTitle:"Suffixes",
    left: withMeanings(["in","re","sus"]),
    right: withMeanings(["s","ed","ing","er","or","ion","able","ive"])
  },
  { id:"latin_07_struct", label:"Latin 7: struct (to build)", family:"Latin", base:"struct", baseMeaning:"to build", baseType:"bound",
    leftTitle:"Prefixes", rightTitle:"Suffixes",
    left: withMeanings(["in","de","con"]),
    right: withMeanings(["s","ed","ing","or","ion","ive"])
  },
  { id:"latin_08_flect_flex", label:"Latin 8: flect / flex (to bend or curve)", family:"Latin", base:"flect", baseMeaning:"to bend or curve", baseType:"bound & free (flex is free*)",
    leftTitle:"Prefixes", rightTitle:"Suffixes",
    left: withMeanings(["in","re","de"]),
    right: withMeanings(["s/es","ed","ing","or","ion","ive"])
  },
  { id:"latin_09_dict", label:"Latin 9: dict (to say or tell)", family:"Latin", base:"dict", baseMeaning:"to say or tell", baseType:"bound",
    leftTitle:"Prefixes", rightTitle:"Suffixes",
    left: withMeanings(["in","pre","inter"]),
    right: withMeanings(["s","ed","ing","ion","able","ive"])
  },
  { id:"latin_10_fer", label:"Latin 10: fer (to bear or yield)", family:"Latin", base:"fer", baseMeaning:"to bear or yield", baseType:"bound",
    leftTitle:"Prefixes", rightTitle:"Suffixes",
    left: withMeanings(["in","re","de","pre","trans"]),
    right: withMeanings(["s","ed","ing","able","al"])
  },
  { id:"latin_11_mit_miss", label:"Latin 11: mit / miss (to send)", family:"Latin", base:"mit", baseMeaning:"to send", baseType:"bound",
    leftTitle:"Prefixes", rightTitle:"Suffixes",
    left: withMeanings(["dis","re","sub","trans"]),
    right: withMeanings(["s/es","ed","ing","er","ion","ible","al"])
  },
  { id:"latin_12_duce_duct", label:"Latin 12: duce / duct (to lead)", family:"Latin", base:"duce", baseMeaning:"to lead", baseType:"bound & free (duct is free*)",
    leftTitle:"Prefixes", rightTitle:"Suffixes",
    left: withMeanings(["in","de","re","con","pro"]),
    right: withMeanings(["s","ed","ing","or","ion","ible","ive"])
  },
  { id:"latin_13_vers_vert", label:"Latin 13: vers / vert (to turn)", family:"Latin", base:"vers", baseMeaning:"to turn", baseType:"bound",
    leftTitle:"Prefixes", rightTitle:"Suffixes",
    left: withMeanings(["in","re","sub","con"]),
    right: withMeanings(["s","ed","ing","ion","ive"])
  },
  { id:"latin_14_fact_fect_fict", label:"Latin 14: fact / fect / fict (to make or do)", family:"Latin", base:"fact", baseMeaning:"to make or do", baseType:"bound & free (fact is free*)",
    leftTitle:"Prefixes", rightTitle:"Suffixes",
    left: withMeanings(["in","de","per"]),
    right: withMeanings(["s","ed","ing","or","ion","al","ive"])
  },
  { id:"latin_15_tend_tent_tens", label:"Latin 15: tend / tent / tens (to stretch or strain)", family:"Latin", base:"tend", baseMeaning:"to stretch or strain", baseType:"bound",
    leftTitle:"Prefixes", rightTitle:"Suffixes",
    left: withMeanings(["in","dis","pre","con"]),
    right: withMeanings(["s","ed","ing","er","ion","ive"])
  },
  { id:"latin_16_ceipt_ceive_cept", label:"Latin 16: ceit / ceive / cept (to take or catch)", family:"Latin", base:"cept", baseMeaning:"to take or catch", baseType:"bound",
    leftTitle:"Prefixes", rightTitle:"Suffixes",
    left: withMeanings(["de","re","inter","con","per","ex"]),
    right: withMeanings(["s","ed","ing","er","or","ion","able","ive"])
  },
  { id:"latin_17_tain_ten_tin", label:"Latin 17: tain / ten / tin (to hold)", family:"Latin", base:"tain", baseMeaning:"to hold", baseType:"bound",
    leftTitle:"Prefixes", rightTitle:"Suffixes",
    left: withMeanings(["re","de","con","per","sus"]),
    right: withMeanings(["s","ed","ing","er","able","tion"])
  },
  { id:"latin_18_pos_pound", label:"Latin 18: pos / pound (to put in place or set)", family:"Latin", base:"pos", baseMeaning:"to put in place or set", baseType:"bound & free (pound is free*)",
    leftTitle:"Prefixes", rightTitle:"Suffixes",
    left: withMeanings(["im","dis","de","com","ex","pro"]),
    right: withMeanings(["s","ed","ing","er","or","tion","al"])
  },

  // Greek templates with multi-center bases (we will SPLIT these)
  { id:"greek_19_gram_graph", label:"Greek 19: gram/graph (written or drawn)", family:"Greek", base:"gram / graph", baseMeaning:"written or drawn", baseType:"middle form",
    leftTitle:"Left Forms", rightTitle:"Suffixes",
    left: [NONE,
      part("auto","self"), part("bio","life"), part("chrono","time"), part("demo","people"),
      part("geo","earth"), part("hydro","water"), part("phono","sound"), part("photo","light"),
      part("tele","distant"), part("thermo","heat"), part("autobio","self + life")
    ],
    right: [NONE, part("er","one who"), part("ic","relating to"), part("ical","relating to"), part("y","subject or science")]
  },
  { id:"greek_20_logy_ology", label:"Greek 20: logy/ology (study of)", family:"Greek", base:"logy / ology", baseMeaning:"study of", baseType:"middle form",
    leftTitle:"Left Forms", rightTitle:"Suffixes",
    left: [NONE,
      part("bio","life"), part("chrono","time"), part("geo","earth"), part("hydro","water"),
      part("graph","written"), part("meter","measure"), part("phono","sound"), part("psych","mind"),
      part("techn","skill"), part("microbio","small + life")
    ],
    right: [NONE, part("ic","relating to"), part("ical","relating to"), part("ist","one who")]
  },
  { id:"greek_21_meter_metr", label:"Greek 21: meter/metr (measure)", family:"Greek", base:"meter / metr", baseMeaning:"measure", baseType:"middle form",
    leftTitle:"Left Forms", rightTitle:"Suffixes",
    left: [NONE,
      part("bio","life"), part("chrono","time"), part("geo","earth"), part("hydro","water"),
      part("micro","small"), part("photo","light"), part("sphero","circle"), part("tele","distant"), part("thermo","heat")
    ],
    right: [NONE, part("ic","relating to"), part("y","subject or science")]
  },
  { id:"greek_22_phone_phon", label:"Greek 22: phone/phon (sound)", family:"Greek", base:"phone / phon", baseMeaning:"sound", baseType:"middle form",
    leftTitle:"Left Forms", rightTitle:"Right Parts",
    left: [NONE, part("geo","earth"), part("gramo","written"), part("hydro","water"), part("micro","small"), part("tele","distant")],
    right: [NONE, part("eme","unit"), part("ic","relating to")]
  },
  { id:"greek_23_sphere", label:"Greek 23: sphere (circle)", family:"Greek", base:"sphere", baseMeaning:"circle", baseType:"middle form",
    leftTitle:"Left Forms", rightTitle:"Suffixes",
    left: [NONE,
      part("astro","star"), part("bio","life"), part("eco","house"), part("geo","earth"),
      part("hemi","half"), part("hydro","water"), part("micro","small"), part("photo","light"), part("thermo","heat")
    ],
    right: [NONE, part("ic","relating to")]
  },
  { id:"greek_24_cracy_crat", label:"Greek 24: cracy/crat (rule)", family:"Greek", base:"cracy / crat", baseMeaning:"rule", baseType:"middle form",
    leftTitle:"Left Forms", rightTitle:"Suffixes",
    left: [NONE, part("auto","self"), part("demo","people"), part("techno","skill")],
    right: [NONE, part("ic","relating to")]
  },
  { id:"greek_25_scope", label:"Greek 25: scope (watch or see)", family:"Greek", base:"scope", baseMeaning:"watch or see", baseType:"middle form",
    leftTitle:"Left Forms", rightTitle:"Suffixes",
    left: [NONE,
      part("bio","life"), part("chrono","time"), part("hydro","water"), part("micro","small"),
      part("phono","sound"), part("photo","light"), part("tele","distant"), part("thermo","heat")
    ],
    right: [NONE, part("ic","relating to")]
  }
];

// --- Split any matrix whose base contains " / " into separate matrices
function splitMultiCenterMatrices(list){
  const out = [];
  for(const m of list){
    if(typeof m.base === "string" && m.base.includes("/")){
      const parts = m.base.split("/").map(s => s.trim()).filter(Boolean);
      for(const center of parts){
        out.push({
          ...m,
          id: `${m.id}__${center}`,
          base: center,
          label: m.label.replace(m.base, center) // nicer dropdown label
        });
      }
    } else {
      out.push(m);
    }
  }
  return out;
}

const MATRICES = splitMultiCenterMatrices(BASE_MATRICES);

/* ===== Definitions (real if found, otherwise “possible”) ===== */
function norm(s){ return String(s||"").toLowerCase().trim(); }

async function fetchDefinition(word){
  const w = norm(word);
  if(!w || w === "—") return null;
  try{
    const res = await fetch(`https://api.dictionaryapi.dev/api/v2/entries/en/${encodeURIComponent(w)}`);
    if(!res.ok) return null;
    const data = await res.json();
    const first = Array.isArray(data) ? data[0] : null;
    const def = first?.meanings?.[0]?.definitions?.[0]?.definition;
    return (typeof def === "string" && def.trim()) ? def.trim() : null;
  }catch{
    return null;
  }
}

function generatedDefinition(leftMeaning, baseMeaning, rightMeaning){
  const parts = [];
  if(leftMeaning) parts.push(leftMeaning);
  if(baseMeaning) parts.push(baseMeaning);
  let core = parts.filter(Boolean).join(" + ");
  if(!core) core = baseMeaning || "something";
  if(rightMeaning) return `Possible meaning (if it were a word): something related to “${core}” and “${rightMeaning}.”`;
  return `Possible meaning (if it were a word): something related to “${core}.”`;
}

/* ===== UI ===== */
const matrixSelect = document.getElementById("matrixSelect");
const leftListEl = document.getElementById("leftList");
const rightListEl = document.getElementById("rightList");
const leftCountEl  = document.getElementById("leftCount");
const rightCountEl = document.getElementById("rightCount");

const leftTitleEl = document.getElementById("leftTitle");
const rightTitleEl = document.getElementById("rightTitle");

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

let current = MATRICES[0];
let chosenLeft = "";
let chosenRight = "";
let defReqToken = 0;

function renderMatrixSelect(){
  matrixSelect.innerHTML = "";
  MATRICES.forEach(m=>{
    const opt = document.createElement("option");
    opt.value = m.id;
    opt.textContent = m.label;
    matrixSelect.appendChild(opt);
  });
}

function setStatus(){
  statusBubble.innerHTML = `<div class="bubble good">✅ Matrix options loaded</div>`;
}

function renderRoot(){
  leftTitleEl.textContent = current.leftTitle || "Left";
  rightTitleEl.textContent = current.rightTitle || "Right";

  rootTextEl.textContent = current.base;
  rootAltEl.textContent  = `${current.family} matrix • ${current.baseType}`;
  rootMeaningEl.textContent = current.baseMeaning;

  rootMetaEl.textContent = `Build: left + base + right (you can choose both)`;
  setStatus();
}

function mkItem(obj, isActive){
  const div = document.createElement("div");
  div.className = "item" + (isActive ? " active" : "");
  div.innerHTML = `<strong>${obj.text}</strong><span>${obj.meaning || ""}</span>`;
  return div;
}

function renderLists(){
  leftListEl.innerHTML = "";
  rightListEl.innerHTML = "";

  current.left.forEach(p=>{
    const div = mkItem(p, p.value === chosenLeft);
    div.addEventListener("click", ()=>{
      chosenLeft = (chosenLeft === p.value) ? "" : p.value;
      renderAll();
    });
    leftListEl.appendChild(div);
  });

  current.right.forEach(s=>{
    const div = mkItem(s, s.value === chosenRight);
    div.addEventListener("click", ()=>{
      chosenRight = (chosenRight === s.value) ? "" : s.value;
      renderAll();
    });
    rightListEl.appendChild(div);
  });

  leftCountEl.textContent = `${current.left.length} options`;
  rightCountEl.textContent  = `${current.right.length} options`;
}

function wordSum(){
  const parts = [];
  if(chosenLeft) parts.push(chosenLeft);
  parts.push(current.base);
  if(chosenRight) parts.push(chosenRight);
  return parts.join(" + ");
}

function partMeanings(){
  const leftM = chosenLeft ? (current.left.find(x=>x.value===chosenLeft)?.meaning || "") : "";
  const rightM = chosenRight ? (current.right.find(x=>x.value===chosenRight)?.meaning || "") : "";
  const parts = [];
  if(chosenLeft) parts.push(`(${chosenLeft}-) ${leftM || "left meaning"}`);
  parts.push(`${current.base} = ${current.baseMeaning}`);
  if(chosenRight) parts.push(`(-${chosenRight}) ${rightM || "right meaning"}`);
  return parts.join(" • ");
}

function wordCandidate(){
  const left = chosenLeft || "";
  const right = chosenRight || "";
  return `${left}${current.base}${right}` || "—";
}

async function renderDefinition(){
  const token = ++defReqToken;

  if(!chosenLeft && !chosenRight){
    finalWordEl.textContent = "—";
    finalDefEl.textContent = "Build a combo to see a definition.";
    comboBubbleEl.innerHTML = "";
    return;
  }

  const candidate = wordCandidate();
  finalWordEl.textContent = candidate;

  const leftMeaning = chosenLeft ? (current.left.find(x=>x.value===chosenLeft)?.meaning || "") : "";
  const rightMeaning = chosenRight ? (current.right.find(x=>x.value===chosenRight)?.meaning || "") : "";

  const realDef = await fetchDefinition(candidate);

  if(token !== defReqToken) return;

  if(realDef){
    finalDefEl.textContent = realDef;
    comboBubbleEl.innerHTML = `<div class="bubble good">Dictionary found a definition</div>`;
  } else {
    finalDefEl.textContent = generatedDefinition(leftMeaning, current.baseMeaning, rightMeaning);
    comboBubbleEl.innerHTML = `<div class="bubble">Dictionary didn’t recognize it — try it in a sentence!</div>`;
  }
}

function renderOutputs(){
  wordSumEl.textContent = wordSum();
  partMeaningsEl.textContent = partMeanings();
  renderDefinition();
}

function renderAll(){
  renderRoot();
  renderLists();
  renderOutputs();
}

function setCurrentMatrix(id){
  current = MATRICES.find(m=>m.id===id) || MATRICES[0];
  chosenLeft = "";
  chosenRight = "";
  renderAll();
}

/* Actions */
clearBtn.addEventListener("click", ()=>{
  chosenLeft = "";
  chosenRight = "";
  renderAll();
});

randomBtn.addEventListener("click", ()=>{
  const leftOpts = current.left.filter(x=>x.value);
  const rightOpts = current.right.filter(x=>x.value);

  chosenLeft = (Math.random() < 0.75 && leftOpts.length) ? leftOpts[Math.floor(Math.random()*leftOpts.length)].value : "";
  chosenRight = (Math.random() < 0.75 && rightOpts.length) ? rightOpts[Math.floor(Math.random()*rightOpts.length)].value : "";
  renderAll();
});

matrixSelect.addEventListener("change", ()=> setCurrentMatrix(matrixSelect.value));

/* Init */
renderMatrixSelect();
setCurrentMatrix(MATRICES[0].id);
matrixSelect.value = MATRICES[0].id;
</script>
</body>
</html>

