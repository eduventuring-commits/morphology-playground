index.html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Morpheme Matrix Builder (Grades 3–5)</title>
  <style>
    :root{
      --bg1:#7dd3ff;
      --bg2:#b8ffcf;
      --bg3:#ffe38d;
      --card:#ffffffee;
      --ink:#13233f;
      --muted:#3b5876;
      --a:#7a5cff;
      --b:#ff4fb6;
      --c:#ff9f1c;
      --ok:#2ecc71;
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
    header{
      max-width: 1100px;
      margin: 0 auto;
      padding: 18px 14px 6px;
    }
    h1{
      margin:0 0 6px;
      font-size: 22px;
      letter-spacing:.2px;
    }
    .subtitle{
      margin:0;
      color: var(--muted);
      line-height: 1.35;
      font-size: 14px;
      font-weight: 700;
    }

    main{
      max-width: 1100px;
      margin: 0 auto;
      padding: 12px 14px 24px;
    }
    .app{
      background: var(--card);
      border-radius: var(--r);
      box-shadow: var(--shadow);
      border: 2px solid rgba(19,35,63,.10);
      overflow:hidden;
    }

    .topbar{
      display:flex;
      gap:12px;
      flex-wrap:wrap;
      align-items:center;
      justify-content:space-between;
      padding: 14px;
      background:
        linear-gradient(90deg, rgba(122,92,255,.16), rgba(255,79,182,.12), rgba(255,159,28,.14));
      border-bottom: 2px dashed rgba(19,35,63,.18);
    }
    .control{
      display:flex;
      gap:10px;
      align-items:center;
      flex-wrap:wrap;
    }
    label{
      font-size:13px;
      color: var(--muted);
      font-weight: 900;
      letter-spacing: .15px;
    }
    select, button{
      border-radius: 14px;
      border: 2px solid rgba(19,35,63,.16);
      background: #fff;
      color: var(--ink);
      padding: 10px 12px;
      font-size: 14px;
      font-weight: 900;
      outline: none;
    }
    button{
      cursor:pointer;
      box-shadow: 0 6px 0 rgba(19,35,63,.10);
      transition: transform .06s ease;
    }
    button:active{ transform: translateY(1px); }
    button.primary{
      border-color: rgba(122,92,255,.35);
      background: rgba(122,92,255,.12);
    }
    button.ghost{
      background: rgba(255,255,255,.75);
    }

    .matrix{
      display:grid;
      grid-template-columns: 1fr;
      gap: 12px;
      padding: 14px;
    }
    @media (min-width: 900px){
      .matrix{ grid-template-columns: 1fr 1fr 1fr; }
    }

    .pane{
      border-radius: 16px;
      border: 2px solid rgba(19,35,63,.12);
      background: rgba(255,255,255,.80);
      padding: 12px;
      min-height: 300px;
    }
    .paneTitle{
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:10px;
      margin-bottom: 10px;
    }
    .paneTitle h2{
      margin:0;
      font-size: 14px;
      letter-spacing:.2px;
      color: var(--muted);
      font-weight: 1000;
    }
    .pill{
      display:inline-block;
      padding: 4px 10px;
      border-radius: 999px;
      font-size: 12px;
      font-weight: 1000;
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
      display:flex;
      flex-direction:column;
      justify-content:center;
      gap:10px;
    }
    .rootText{
      font-size: 42px;
      font-weight: 1100;
      letter-spacing: .6px;
    }
    .rootAlt{
      font-size: 13px;
      color: var(--muted);
      font-weight: 900;
    }
    .rootMeaning{
      font-size: 16px;
      font-weight: 1100;
    }
    .tiny{
      font-size: 12px;
      color: var(--muted);
      font-weight: 900;
      line-height: 1.35;
    }

    .list{
      height: 240px;
      overflow:auto;
      padding-right: 4px;
      border-radius: 14px;
      border: 2px dashed rgba(19,35,63,.18);
      background: rgba(255,255,255,.78);
    }
    .item{
      display:flex;
      justify-content:space-between;
      gap:10px;
      align-items:center;
      padding: 10px 10px;
      margin: 8px;
      border-radius: 14px;
      border: 2px solid rgba(19,35,63,.12);
      background: #fff;
      cursor:pointer;
      user-select:none;
    }
    .item strong{ font-size: 16px; font-weight: 1100; }
    .item span{ color: var(--muted); font-size: 12px; font-weight: 900; }
    .item.active{
      border-color: rgba(255,79,182,.70);
      box-shadow: 0 0 0 4px rgba(255,79,182,.16);
      background: rgba(255,79,182,.08);
    }

    .outputs{
      padding: 0 14px 14px;
      display:grid;
      grid-template-columns: 1fr;
      gap: 10px;
    }
    @media (min-width: 900px){
      .outputs{ grid-template-columns: 1fr 1fr; }
    }
    .outCard{
      border-radius: 16px;
      border: 2px solid rgba(19,35,63,.12);
      background: rgba(255,255,255,.88);
      padding: 12px;
    }
    .outLabel{
      font-size: 12px;
      color: var(--muted);
      font-weight: 1100;
      letter-spacing:.2px;
      margin-bottom: 6px;
    }
    .outValue{
      font-size: 22px;
      font-weight: 1200;
      letter-spacing:.2px;
      min-height: 28px;
    }
    .meaning{
      font-size: 14px;
      color: var(--ink);
      font-weight: 900;
      line-height: 1.4;
    }
    .bubble{
      display:inline-flex;
      align-items:center;
      gap:8px;
      font-weight:1100;
      padding: 6px 10px;
      border-radius: 999px;
      font-size: 12px;
      margin-top: 10px;
      border: 2px solid rgba(19,35,63,.12);
      background: rgba(255,255,255,.65);
      color: var(--muted);
    }
    .bad{
      border-color: rgba(255,79,182,.35);
      background: rgba(255,79,182,.12);
      color: #5e143b;
    }
    .good{
      border-color: rgba(46,204,113,.35);
      background: rgba(46,204,113,.16);
      color: #0e3b1f;
    }

    /* Hidden Word Key loader modal (NOT visible unless shortcut is used) */
    .modalWrap{
      position: fixed;
      inset: 0;
      background: rgba(0,0,0,.45);
      display:none;
      align-items:center;
      justify-content:center;
      padding: 16px;
      z-index: 9999;
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
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:10px;
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
      display:flex;
      gap:10px;
      flex-wrap:wrap;
      align-items:center;
      justify-content:space-between;
      margin-top: 10px;
    }
  </style>
</head>
<body>
  <header>
    <h1>Morpheme Matrix Builder</h1>
    <p class="subtitle">Pick a root/base, then click a prefix and suffix. If it’s not in the real-word list, you’ll see “not a real word combo.”</p>
  </header>

  <main>
    <div class="app" role="application" aria-label="Morpheme Matrix Builder">
      <div class="topbar">
        <div class="control">
          <label for="matrixSelect">Root/Base:</label>
          <select id="matrixSelect" aria-label="Choose a root or base"></select>
        </div>
        <div class="control">
          <button id="clearBtn" class="ghost" aria-label="Clear prefix and suffix">Clear</button>
          <button id="randomBtn" class="primary" aria-label="Choose a random real word">Random Real Word</button>
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

        <section class="rootCard" aria-label="Root or base">
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
          <div class="tiny">Pick any suffix — some combos won’t be real words (and that’s okay!).</div>
        </section>
      </div>

      <div class="outputs">
        <div class="outCard" aria-label="Word sum">
          <div class="outLabel">WORD SUM</div>
          <div class="outValue" id="wordSum">—</div>
          <div class="tiny" id="partMeanings">—</div>
        </div>

        <div class="outCard" aria-label="Word and meaning">
          <div class="outLabel">WORD + MEANING</div>
          <div class="outValue" id="finalWord">—</div>
          <div class="meaning" id="finalMeaning">Choose parts to build a word.</div>
          <div id="comboBubble"></div>
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
          Paste the comma-separated “Word Key” list for this root/base from the PDF.
          Example: <b>forms, formed, forming, former</b>…
        </div>
        <textarea id="wordListBox" placeholder="Paste words here..."></textarea>
        <div class="modalRow">
          <div class="tiny" id="modalInfo"></div>
          <div class="control">
            <button id="resetMatrixBtn" class="ghost">Reset this root</button>
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
    // ============================================================
    // MATRIX SETUP
    // ============================================================
    const NONE = { text:"—", value:"", meaning:"(none)" };

    const prefixMeaning = {
      "in":"in/into", "im":"in/into", "un":"not", "mis":"wrong/bad", "dis":"not/apart",
      "re":"again/back", "de":"down/away", "pre":"before", "sub":"under",
      "inter":"between", "con":"together", "com":"together", "pro":"forward",
      "per":"through", "ex":"out", "sus":"under", "at":"toward", "e":"out",
      "tele":"far", "micro":"small", "photo":"light", "geo":"earth", "bio":"life",
      "demo":"people", "hydro":"water", "thermo":"heat", "chrono":"time",
      "psycho":"mind", "techno":"skill/art"
    };

    const suffixMeaning = {
      "s":"plural / verb -s", "es":"plural", "ed":"past tense", "ing":"happening now",
      "er":"person/thing", "or":"person/thing", "ion":"act/state", "tion":"act/state",
      "ation":"act/state", "al":"relating to", "ive":"relating to",
      "able":"can be", "ible":"can be", "y":"noun ending", "ic":"relating to", "ical":"relating to",
      "ist":"one who"
    };

    function P(p){ return { text:p, value:p, meaning: prefixMeaning[p] || "prefix" }; }
    function S(s){ return { text:s, value:s, meaning: suffixMeaning[s] || "suffix" }; }

    // Keep these as your base list (you can add more matrices later).
    // Real words come from the Word Key list you paste in (hidden loader). 
    const MATRICES = [
      mkLatin("form", ["in","re","de"], ["form"], "to shape", ["s","ed","ing","er","ation","al"]),
      mkLatin("port", ["im","re","de","trans"], ["port"], "to carry", ["s","ed","ing","er","ion","ation","able","al"]),
      mkLatin("rupt", ["dis","e","inter"], ["rupt"], "to break or burst", ["s","ed","ing","ion","ive","ible"]),
      mkLatin("tract", ["dis","re","sub","at"], ["tract"], "to draw or pull", ["s","or","ion","able"]),
      mkLatin("script", ["de","pre","sub","tran"], ["scrib","scribe","script"], "to write", ["s","ed","ing","ion"]),
      mkLatin("spect", ["in","re","sus"], ["spect","pect"], "to see/watch/observe", ["or","ion","ive"]),
      mkLatin("struct", ["con","in","de"], ["struct"], "to build", ["or","ion"]),
      mkLatin("flex", ["re","de","in"], ["flex","flect"], "to bend/curve", ["ible","ion","ive"]),
      mkLatin("dict", ["pre","in","inter"], ["dict"], "to say/tell", ["ion","ive"]),
      mkLatin("fer", ["pre","re","in","trans"], ["fer"], "to carry/bear", ["s","ed","al"]),
      mkLatin("mit", ["sub","trans","dis","re"], ["mit","miss"], "to send", ["s","ed","ion","able"]),
      mkLatin("duce", ["re","in","pro","de","con"], ["duce","duct"], "to lead", ["s","ion","t"]),
      mkLatin("vert", ["con","in","re","sub"], ["vert","vers","verse"], "to turn", ["ed","ion","ive"]),
      mkLatin("fect", ["per","de","in","ef"], ["fact","fect","fict"], "to make/do", ["ion","ive","al"]),
      mkLatin("tend", ["in","pre","con"], ["tend","tens","tent"], "to stretch/strain", ["ion","ive"]),
      mkLatin("ceive", ["re","de","per","inter","con","ex"], ["ceive","ceit","cept"], "to take/catch", ["ed","ing","ion"]),
      mkLatin("tain", ["con","re","de","sus"], ["tain","ten","tin"], "to hold", ["ed","ing","er"]),

      mkGreek("graph/gram", ["photo","tele","bio","auto","geo","demo"], ["graph","gram"], "written/drawn", ["y","ic"]),
      mkGreek("logy/ology", ["bio","geo","hydro","psycho","techno"], ["logy","ology"], "study of", ["ical","ist"]),
      mkGreek("meter/metr", ["thermo","geo","tele","micro"], ["meter","metr","metry"], "measure", ["y","ic"]),
      mkGreek("phon/phono", ["tele","micro"], ["phone","phon","phono"], "sound", ["ic"]),
      mkGreek("cracy/crat", ["demo","auto"], ["cracy","crat"], "rule/government", ["ic"]),
      mkGreek("scope", ["tele","micro"], ["scope"], "watch/see", ["ic"])
    ];

    function mkLatin(rootLabel, prefixes, rootForms, meaning, suffixes){
      return {
        id: `root_${rootLabel}`,
        rootLabel,
        rootForms,
        rootMeaning: meaning,
        kind: "latin",
        prefixes: [NONE, ...prefixes.map(P)],
        suffixes: [NONE, ...suffixes.map(S)]
      };
    }
    function mkGreek(rootLabel, leftForms, midForms, meaning, derivSuffixes){
      return {
        id: `root_${rootLabel.replaceAll("/","_")}`,
        rootLabel,
        rootForms: midForms,
        rootMeaning: meaning,
        kind: "greek",
        prefixes: [NONE, ...leftForms.map(P)],
        suffixes: [NONE, ...derivSuffixes.map(S)]
      };
    }

    // ============================================================
    // WORD KEY STORAGE (hidden loader)
    // ============================================================
    const LS_KEY = "morpheme_matrix_wordkey_v2";

    function loadAllWordLists(){
      try{
        const raw = localStorage.getItem(LS_KEY);
        if(!raw) return {};
        const obj = JSON.parse(raw);
        if(!obj || typeof obj !== "object") return {};
        for(const k of Object.keys(obj)){
          obj[k] = (obj[k] || []).map(w => String(w).trim()).filter(Boolean).map(w => w.toLowerCase());
        }
        return obj;
      }catch{
        return {};
      }
    }
    function saveAllWordLists(obj){
      localStorage.setItem(LS_KEY, JSON.stringify(obj));
    }

    // A small starter set so it works immediately (you can overwrite via loader)
    const STARTER = {
      root_form: ["forms","formed","forming","former","formal","inform","reform","deform","information","informal","reformation"],
      root_port: ["import","report","deport","transport","portable","importation","deportation"],
      root_tract: ["attract","retract","distract","subtract","traction","distraction"],
      root_spect: ["inspect","inspection","inspector","respect","suspect"],
      root_struct: ["construct","construction","instruct","instruction","destruct","destruction"],
      root_graph_gram: ["photograph","photography","telegraph","biography","geography","demography","autograph"],
      root_logy_ology: ["biology","geology","hydrology","psychology","technology"],
      root_meter_metr: ["thermometer","geometry","telemetry","micrometer"],
      root_scope: ["telescope","microscope","telescopic","microscopic"]
    };

    function getWordLists(){
      const stored = loadAllWordLists();
      for(const [k,v] of Object.entries(STARTER)){
        if(!stored[k] || !stored[k].length) stored[k] = v.map(x=>x.toLowerCase());
      }
      return stored;
    }

    // ============================================================
    // PARSING: build (prefix, rootForm, suffix) combos from Word Key list
    // ============================================================
    function normalize(s){ return String(s || "").toLowerCase(); }

    function parseWordToCombo(word, matrix){
      const w = normalize(word);

      const prefCands = matrix.prefixes.map(p=>p.value).map(normalize);
      const sufCands  = matrix.suffixes.map(s=>s.value).map(normalize);
      const rootCands = (matrix.rootForms || []).map(normalize);

      // Try all (prefix, suffix, root) combos (safe + accurate enough for classroom sets)
      // Handles common e-drop/e-add patterns (pose/ceive/scribe families).
      let best = null;

      for(const p of prefCands){
        if(p && !w.startsWith(p)) continue;
        const afterP = w.slice(p.length);

        for(const s of sufCands){
          if(s && !afterP.endsWith(s)) continue;
          const mid = afterP.slice(0, afterP.length - s.length);

          for(const r of rootCands){
            if(!r) continue;

            // exact
            if(mid === r) best = pickBetter(best, {pref:p, root:r, suf:s, word:w});
            // e-add
            if(mid === r + "e") best = pickBetter(best, {pref:p, root:r, suf:s, word:w});
            // e-drop
            if(r.endsWith("e") && mid === r.slice(0,-1)) best = pickBetter(best, {pref:p, root:r, suf:s, word:w});

            // Greek-ish: allow root form appearing inside mid (for things like geometry)
            if(matrix.kind === "greek" && mid.includes(r)) best = pickBetter(best, {pref:p, root:r, suf:s, word:w});
          }
        }
      }

      return best; // may be null if we can’t parse (we ignore those)
    }

    function pickBetter(a, b){
      if(!a) return b;
      // prefer longer root match, then longer prefix+suffix
      const score = (x) => (x.root?.length||0)*10 + (x.pref?.length||0) + (x.suf?.length||0);
      return score(b) > score(a) ? b : a;
    }

    function buildCombos(matrix, wordList){
      const combos = [];
      const wordSet = new Set();

      for(const w of wordList){
        const word = normalize(w);
        if(!word) continue;
        wordSet.add(word);

        const combo = parseWordToCombo(word, matrix);
        if(combo) combos.push(combo);
      }

      return { combos, wordSet };
    }

    // ============================================================
    // UI
    // ============================================================
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
    const finalMeaningEl = document.getElementById("finalMeaning");
    const comboBubbleEl = document.getElementById("comboBubble");

    const clearBtn = document.getElementById("clearBtn");
    const randomBtn = document.getElementById("randomBtn");

    // Hidden loader modal
    const modalWrap = document.getElementById("modalWrap");
    const closeModalBtn = document.getElementById("closeModalBtn");
    const saveMatrixBtn = document.getElementById("saveMatrixBtn");
    const resetMatrixBtn = document.getElementById("resetMatrixBtn");
    const wordListBox = document.getElementById("wordListBox");
    const modalTitle = document.getElementById("modalTitle");
    const modalInfo = document.getElementById("modalInfo");

    let wordLists = getWordLists();

    let current = MATRICES[0];
    let chosenPrefix = "";
    let chosenSuffix = "";

    let comboData = buildCombos(current, wordLists[current.id] || []);

    function renderMatrixSelect(){
      matrixSelect.innerHTML = "";
      MATRICES.forEach(m=>{
        const opt = document.createElement("option");
        opt.value = m.id;
        // Show root/base instead of “Matrix #”
        opt.textContent = `${m.rootLabel} — ${m.rootMeaning}`;
        matrixSelect.appendChild(opt);
      });
    }

    function setStatus(){
      const stored = loadAllWordLists();
      const hasLoaded = stored[current.id] && stored[current.id].length;
      statusBubble.innerHTML = hasLoaded
        ? `<div class="bubble good">✅ Real-word list loaded</div>`
        : `<div class="bubble">⭐ Using starter list (Ctrl/Cmd + Shift + L to load Word Key list)</div>`;
    }

    function rootFormsLine(){
      const forms = (current.rootForms || []);
      if(!forms.length) return "";
      if(forms.length === 1) return `Form: ${forms[0]}`;
      return `Forms: ${forms.join(", ")}`;
    }

    function renderRoot(){
      rootTextEl.textContent = current.rootLabel;
      rootAltEl.textContent  = rootFormsLine();
      rootMeaningEl.textContent = current.rootMeaning;
      rootMetaEl.textContent = current.kind === "greek" ? "Type: Greek form" : "Type: Root/base";
      setStatus();
    }

    // For your rule: only show affixes that COULD make a real word for this base (individually).
    function allowedPrefixSet(){
      const set = new Set(comboData.combos.map(c => c.pref || ""));
      set.add("");
      return set;
    }
    function allowedSuffixSet(){
      const set = new Set(comboData.combos.map(c => c.suf || ""));
      set.add("");
      return set;
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

      const pAllowed = allowedPrefixSet();
      const sAllowed = allowedSuffixSet();

      const prefixes = current.prefixes.filter(p => pAllowed.has(p.value));
      const suffixes = current.suffixes.filter(s => sAllowed.has(s.value));

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
      parts.push(current.rootLabel);
      if(chosenSuffix) parts.push(chosenSuffix);
      return parts.join(" + ");
    }

    function partMeanings(){
      const parts = [];
      if(chosenPrefix) parts.push(`(${chosenPrefix}-) ${prefixMeaning[chosenPrefix] || "prefix"}`);
      parts.push(`${current.rootLabel} = ${current.rootMeaning}`);
      if(chosenSuffix) parts.push(`(-${chosenSuffix}) ${suffixMeaning[chosenSuffix] || "suffix"}`);
      return parts.join(" • ");
    }

    function findRealWord(){
      // Find a combo in parsed combos that exactly matches chosen prefix+suffix
      const hit = comboData.combos.find(c => (c.pref||"") === chosenPrefix && (c.suf||"") === chosenSuffix);
      return hit ? hit.word : "";
    }

    function renderOutputs(){
      wordSumEl.textContent = wordSum();
      partMeaningsEl.textContent = partMeanings();

      const real = findRealWord();

      if(real){
        finalWordEl.textContent = real;
        finalMeaningEl.textContent = "Real word ✅ (from the Word Key list)";
        comboBubbleEl.innerHTML = `<div class="bubble good">✅ Real word combo</div>`;
      } else {
        // If either is unselected, give a gentle prompt. If both selected, show your requested message.
        finalWordEl.textContent = "—";
        if(chosenPrefix || chosenSuffix){
          if(chosenPrefix && chosenSuffix){
            finalMeaningEl.textContent = "Not a real word combo (for this matrix). Try a different prefix or suffix!";
            comboBubbleEl.innerHTML = `<div class="bubble bad">🚫 Not a real word combo</div>`;
          } else {
            finalMeaningEl.textContent = "Pick one more part to test your combo.";
            comboBubbleEl.innerHTML = `<div class="bubble">✨ Keep building!</div>`;
          }
        } else {
          finalMeaningEl.textContent = "Choose parts to build a word.";
          comboBubbleEl.innerHTML = `<div class="bubble">✨ Start with a prefix or suffix</div>`;
        }
      }
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
      wordLists = getWordLists();
      comboData = buildCombos(current, wordLists[current.id] || []);
      renderAll();
    }

    // Buttons
    clearBtn.addEventListener("click", ()=>{
      chosenPrefix = "";
      chosenSuffix = "";
      renderAll();
    });

    randomBtn.addEventListener("click", ()=>{
      const pool = comboData.combos;
      if(!pool.length) return;
      const pick = pool[Math.floor(Math.random() * pool.length)];
      chosenPrefix = pick.pref || "";
      chosenSuffix = pick.suf || "";
      renderAll();
    });

    matrixSelect.addEventListener("change", ()=> setCurrentMatrix(matrixSelect.value));

    // ============================================================
    // Hidden Loader: Ctrl/Cmd + Shift + L
    // ============================================================
    function openModal(){
      modalWrap.style.display = "flex";
      modalWrap.setAttribute("aria-hidden","false");

      const stored = loadAllWordLists();
      const existing = stored[current.id] || [];
      modalTitle.textContent = `Load Word Key list — ${current.rootLabel}`;
      modalInfo.textContent = existing.length ? `Currently saved: ${existing.length} words` : `No saved list yet (starter list is being used).`;

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
      const words = text
        .split(",")
        .map(w => w.trim())
        .filter(Boolean)
        .map(w => w.toLowerCase());

      const stored = loadAllWordLists();
      stored[current.id] = words;
      saveAllWordLists(stored);

      wordLists = getWordLists();
      comboData = buildCombos(current, wordLists[current.id] || []);
      closeModal();
      renderAll();
    });

    resetMatrixBtn.addEventListener("click", ()=>{
      const stored = loadAllWordLists();
      delete stored[current.id];
      saveAllWordLists(stored);

      wordLists = getWordLists();
      comboData = buildCombos(current, wordLists[current.id] || []);
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

    // Init
    renderMatrixSelect();
    setCurrentMatrix(MATRICES[0].id);
    matrixSelect.value = MATRICES[0].id;
  </script>
</body>
</html>
