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

    /* Lists: always readable (no hidden chips) */
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
    .item.disabled{
      opacity:.45;
      cursor:not-allowed;
      filter: grayscale(.2);
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
    .ok{
      display:inline-flex;
      align-items:center;
      gap:8px;
      font-weight:1100;
      color: #0e3b1f;
      background: rgba(46,204,113,.18);
      border: 2px solid rgba(46,204,113,.35);
      padding: 6px 10px;
      border-radius: 999px;
      font-size: 12px;
      margin-top: 10px;
    }
    .warn{
      display:inline-flex;
      align-items:center;
      gap:8px;
      font-weight:1100;
      color: #5d2a00;
      background: rgba(255,159,28,.18);
      border: 2px solid rgba(255,159,28,.35);
      padding: 6px 10px;
      border-radius: 999px;
      font-size: 12px;
      margin-top: 10px;
    }

    /* Hidden teacher loader modal (not visible unless opened with shortcut) */
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
    <p class="subtitle">Pick a root/base. Then click a prefix and suffix to build a <b>real word</b>.</p>
  </header>

  <main>
    <div class="app" role="application" aria-label="Morpheme Matrix Builder">
      <div class="topbar">
        <div class="control">
          <label for="matrixSelect">Matrix:</label>
          <select id="matrixSelect" aria-label="Choose a matrix"></select>
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
          <div class="tiny">Tip: click again to turn a selection off.</div>
        </section>

        <section class="rootCard" aria-label="Root or base">
          <div class="rootText" id="rootText">—</div>
          <div class="rootAlt" id="rootAlt"></div>
          <div class="rootMeaning" id="rootMeaning">—</div>
          <div class="tiny" id="rootMeta"></div>
          <div id="statusPill"></div>
        </section>

        <section class="pane" aria-label="Suffixes">
          <div class="paneTitle">
            <h2>Suffixes</h2>
            <span class="pill" id="sufCount">0 options</span>
          </div>
          <div class="list" id="suffixList" tabindex="0" aria-label="Suffix list"></div>
          <div class="tiny">Suffix choices update so words stay real.</div>
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
          <div class="meaning" id="finalMeaning">Choose parts to build a real word.</div>
        </div>
      </div>
    </div>
  </main>

  <!-- Hidden teacher loader (Ctrl/Cmd + Shift + L) -->
  <div class="modalWrap" id="modalWrap" aria-hidden="true">
    <div class="modal" role="dialog" aria-modal="true" aria-label="Load real word list">
      <div class="modalTop">
        <strong id="modalTitle">Load Word Key list</strong>
        <button id="closeModalBtn" class="ghost">Close</button>
      </div>
      <div class="modalBody">
        <div class="tiny">
          Paste the comma-separated word list for this matrix from the PDF Word Key.
          (Example format: <b>forms, formed, forming, former</b> …)
        </div>
        <textarea id="wordListBox" placeholder="Paste words here..."></textarea>
        <div class="modalRow">
          <div class="tiny" id="modalInfo"></div>
          <div class="control">
            <button id="resetMatrixBtn" class="ghost">Reset this matrix</button>
            <button id="saveMatrixBtn" class="primary">Save</button>
          </div>
        </div>
        <div class="tiny" style="margin-top:10px;">
          Stored on this device/browser only (local storage).
        </div>
      </div>
    </div>
  </div>

  <script>
    // ------------------------------------------------------------
    // DATA: Matrix structure (prefixes/roots/suffixes) from the PDF
    // The "real words" list is loaded from the Word Key via the
    // hidden loader (Ctrl/Cmd+Shift+L). :contentReference[oaicite:2]{index=2}
    //
    // IMPORTANT: This file includes a SMALL starter real-word set so
    // it works immediately. For full fidelity, paste each matrix’s
    // Word Key list into the loader.
    // ------------------------------------------------------------

    const NONE = { text:"—", meaning:"(none)", value:"" };

    // quick meanings (kid-friendly)
    const prefixMeaning = {
      "in":"in/into/toward", "im":"in/into/toward",
      "un":"not/opposite", "mis":"bad/wrong", "dis":"not/apart",
      "re":"again/back", "de":"down/away", "pre":"before",
      "en":"put into/on", "em":"put into/on", "sub":"under/below",
      "inter":"between", "con":"together", "com":"together",
      "pro":"forward", "per":"through", "ex":"out",
      "at":"toward", "re-":"again/back", "e":"out"
    };
    const suffixMeaning = {
      "s":"plural / verb -s", "es":"plural / verb -es",
      "ed":"past tense", "ing":"happening now",
      "er":"a person/thing", "or":"a person/thing",
      "ion":"act/state", "sion":"act/state", "tion":"act/state", "ation":"act/state",
      "able":"can be", "ible":"can be",
      "al":"relating to", "ial":"relating to",
      "y":"noun/subject word", "ive":"relating to",
      "ic":"relating to", "ical":"relating to",
      "ist":"one who"
    };

    function P(p){ return { text:p, value:p, meaning: prefixMeaning[p] || "prefix" }; }
    function S(s){ return { text:s, value:s, meaning: suffixMeaning[s] || "suffix" }; }

    // Matrices 1–18 are Latin-root matrices; 19–25 are Greek-form matrices. :contentReference[oaicite:3]{index=3}
    const MATRICES = [
      mkLatin(1, ["in","re","de"], "form", ["form"], "to shape", "free", ["s","ed","ing","er","ation","al"]),
      mkLatin(2, ["im","re","de"], "port", ["port"], "to carry", "free", ["s","ed","ing","er","ion","ation","able","al"]),
      mkLatin(3, ["dis"], "rupt", ["rupt"], "to break or burst", "bound", ["s","ed","ing","e","er","tion","ible","ive"], { combos:[{prefix:"inter", root:"rupt", wordStarts:"inter"}] }),
      mkLatin(4, ["dis","re","de","sub"], "tract", ["tract"], "to draw or pull", "bound", ["s","or","ion","able"]),
      mkLatin(5, ["in","de","pre","sub"], "script", ["scrib","scribe","script"], "to write", "bound", ["s","ed","ing","er","ion"]),
      mkLatin(6, ["in","re","sus"], "spect", ["spect","pect"], "to see/watch/observe", "bound", ["s","ed","ing","or","ion","able","ive"]),
      mkLatin(7, ["in","de","con"], "struct", ["struct"], "to build", "bound", ["s","ed","ing","or","ion","ive"]),
      mkLatin(8, ["in","re","de"], "flex", ["flex","flect"], "to bend/curve", "bound", ["es","ed","ing","or","ion","ive"]),
      mkLatin(9, ["in","pre","inter"], "dict", ["dict"], "to say/tell", "bound", ["s","ed","ing","ion","able","ive"]),
      mkLatin(10, ["in","re","de","pre","trans"], "fer", ["fer"], "to bear/yield", "bound", ["s","ed","ing","al","al"], { note:"Some words double consonants (e.g., referred)." }),
      mkLatin(11, ["re","sub","trans","dis"], "mit", ["mit","miss"], "to send", "bound", ["s","ed","ing","er","able","ion"]),
      mkLatin(12, ["in","de","re","pro","con"], "duce", ["duce","duct"], "to lead", "bound", ["s","ed","ing","ion","ive","t"], { note:"Some words use duct (conduct, product)." }),
      mkLatin(13, ["in","re","sub","con"], "vert", ["vert","vers","verse"], "to turn", "bound", ["es","ed","ing","ion","ive"]),
      mkLatin(14, ["in","de","per"], "fect", ["fact","fect","fict"], "to make/do", "bound", ["s","ed","ing","or","ion","ive","al"]),
      mkLatin(15, ["in","dis","pre","con"], "tend", ["tend","tens","tent","tense"], "to stretch/strain", "bound", ["s","ed","ing","er","ion","ive"]),
      mkLatin(16, ["de","re","con","per","inter","ex"], "ceive", ["ceive","ceit","cept","cept"], "to take/catch", "bound", ["s","ed","ing","er","ion","ive","able"]),
      mkLatin(17, ["re","de","con","per","sus"], "tain", ["tain","ten","tin"], "to hold", "bound", ["s","ed","ing","er","able","ion"]),
      mkLatin(18, ["im","dis","de","com","ex","pro"], "pose", ["pose","pound"], "to put/place", "bound", ["s","d","ing","er","ition","al"], { note:"Includes pound/pose family (compound, impound, expose, etc.)." }),

      mkGreek(19, ["auto","bio","chrono","demo","geo","hydro","phono","photo","tele","thermo","autobio"], ["gram","graph"], "written/drawn", ["er","ic","ical","y"]),
      mkGreek(20, ["bio","chrono","geo","hydro","graph","meter","phono","psych","techn","microbio"], ["logy","ology"], "study of", ["ic","ical","ist"]),
      mkGreek(21, ["chrono","geo","hydro","micro","photo","tele","thermo","bio"], ["meter","metr","metry"], "measure", ["ic","y"]),
      mkGreek(22, ["phono","geo","gram","hydro","micro","tele"], ["phon","phone","phoneme","phonic"], "sound", ["ic"]),
      mkGreek(23, ["astro","bio","eco","geo","hemi","hydro","micro","photo","thermo"], ["sphere"], "circle", ["ic","ical"]),
      mkGreek(24, ["auto","demo","techno"], ["cracy","crat"], "rule/government", ["ic"]),
      mkGreek(25, ["bio","chrono","hydro","micro","phono","tele"], ["scope"], "watch/see", ["ic"])
    ];

    function mkLatin(n, prefixes, rootLabel, rootForms, meaning, type, suffixes, extra={}){
      return {
        id: `matrix${n}`,
        label: `Matrix ${n}`,
        kind: "latin",
        prefixes: [NONE, ...prefixes.map(P)],
        rootLabel,
        rootForms,
        rootMeaning: meaning,
        rootType: type,
        suffixes: [NONE, ...suffixes.map(S)],
        extra
      };
    }
    function mkGreek(n, leftForms, midForms, midMeaning, derivSuffixes){
      return {
        id: `matrix${n}`,
        label: `Matrix ${n}`,
        kind: "greek",
        prefixes: [NONE, ...leftForms.map(P)],
        rootLabel: midForms.join("/"),
        rootForms: midForms, // treat as “middle form”
        rootMeaning: midMeaning,
        rootType: "Greek form",
        suffixes: [NONE, ...derivSuffixes.map(S)]
      };
    }

    // ------------------------------------------------------------
    // REAL WORD LISTS (loaded from Word Key via localStorage)
    // We store: { [matrixId]: ["word1","word2", ...] }
    // ------------------------------------------------------------
    const LS_KEY = "morpheme_matrix_wordkey_v1";

    function loadAllWordLists(){
      try{
        const raw = localStorage.getItem(LS_KEY);
        if(!raw) return {};
        const obj = JSON.parse(raw);
        if(!obj || typeof obj !== "object") return {};
        // normalize lowercase
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

    // Starter real-word sets (small) so it works immediately.
    // For full fidelity, paste each matrix list from the PDF Word Key into the loader. :contentReference[oaicite:4]{index=4}
    const STARTER = {
      matrix1: ["forms","formed","forming","former","formal","inform","reform","deform","information","informal","reformer"],
      matrix2: ["ports","ported","porting","porter","portal","portable","import","report","deport","importation","deportation"],
      matrix4: ["subtract","retract","distract","traction","tractor","tractable"],
      matrix6: ["inspect","inspector","inspection","respect","suspect"],
      matrix7: ["construct","construction","instruct","instruction","destruct","destruction"],
      matrix19: ["photograph","photography","biography","telegraph","autograph"],
      matrix20: ["biology","geology","hydrology","psychology","technology"],
      matrix21: ["thermometer","geometry","telemetry","micrometer"],
      matrix25: ["telescope","microscope","telescopic","microscopic"]
    };

    function getWordLists(){
      const stored = loadAllWordLists();
      // merge starter where matrix has no stored list yet
      for(const [k,v] of Object.entries(STARTER)){
        if(!stored[k] || !stored[k].length) stored[k] = v.map(x=>x.toLowerCase());
      }
      return stored;
    }

    // ------------------------------------------------------------
    // PARSING: derive (prefix, rootForm, suffix) for each word
    // so we can filter options to "real words only".
    // ------------------------------------------------------------

    function longestMatchAtStart(word, candidates){
      let best = "";
      for(const c of candidates){
        if(!c) continue;
        if(word.startsWith(c.toLowerCase()) && c.length > best.length) best = c.toLowerCase();
      }
      return best;
    }
    function longestMatchAtEnd(word, candidates){
      let best = "";
      for(const c of candidates){
        if(!c) continue;
        if(word.endsWith(c.toLowerCase()) && c.length > best.length) best = c.toLowerCase();
      }
      return best;
    }

    function buildEntries(matrix, wordList){
      // candidates (strings)
      const prefCands = matrix.prefixes.map(p=>p.value).filter(Boolean);
      const sufCands  = matrix.suffixes.map(s=>s.value).filter(Boolean);

      // For Latin matrices, the root might be in the middle after stripping prefix/suffix.
      // For Greek matrices, treat rootForms as the “middle form” (gram/graph/logy/etc.).
      const rootCands = (matrix.rootForms || []).map(r=>String(r).toLowerCase());

      const entries = [];

      for(const raw of wordList){
        const word = String(raw).trim().toLowerCase();
        if(!word) continue;

        // pick prefix + suffix (longest matches)
        const pref = longestMatchAtStart(word, prefCands);
        const suf  = longestMatchAtEnd(word, sufCands);

        const mid = word.slice(pref.length, word.length - suf.length);

        let rootMatch = "";
        if(matrix.kind === "greek"){
          // find which middle form appears inside the remainder (prefer longer)
          rootMatch = rootCands.sort((a,b)=>b.length-a.length).find(r => mid.includes(r)) || "";
        } else {
          // exact remainder should be (a) root form or (b) root form + small spelling tweak
          rootMatch = rootCands.sort((a,b)=>b.length-a.length).find(r => mid === r) || "";
          if(!rootMatch){
            // allow common silent-e patterns used in the Word Key (scribe/ceive/pose families)
            rootMatch = rootCands.sort((a,b)=>b.length-a.length).find(r => mid === r + "e") || "";
            if(!rootMatch){
              rootMatch = rootCands.sort((a,b)=>b.length-a.length).find(r => (r.endsWith("e") && mid === r.slice(0,-1))) || "";
            }
          }
        }

        // If we can't confidently parse, still keep the word as “valid” for random selection,
        // but it won't drive prefix/suffix filtering.
        entries.push({ word, pref, suf, root: rootMatch, mid });
      }

      return entries;
    }

    // Very lightweight “meaning” generator (teacher-friendly, kid-friendly).
    // You can optionally add a definition map later if you want.
    function autoMeaning(matrix, pref, suf){
      const p = pref ? (prefixMeaning[pref] || "prefix") : "";
      const r = matrix.rootMeaning;
      const s = suf ? (suffixMeaning[suf] || "suffix") : "";
      if(pref && suf) return `${p} + ${r} + (${s})`;
      if(pref) return `${p} + ${r}`;
      if(suf) return `${r} + (${s})`;
      return r;
    }

    // ------------------------------------------------------------
    // UI
    // ------------------------------------------------------------
    const matrixSelect = document.getElementById("matrixSelect");
    const prefixListEl = document.getElementById("prefixList");
    const suffixListEl = document.getElementById("suffixList");
    const prefCountEl  = document.getElementById("prefCount");
    const sufCountEl   = document.getElementById("sufCount");

    const rootTextEl   = document.getElementById("rootText");
    const rootAltEl    = document.getElementById("rootAlt");
    const rootMeaningEl= document.getElementById("rootMeaning");
    const rootMetaEl   = document.getElementById("rootMeta");
    const statusPillEl = document.getElementById("statusPill");

    const wordSumEl    = document.getElementById("wordSum");
    const partMeaningsEl = document.getElementById("partMeanings");
    const finalWordEl  = document.getElementById("finalWord");
    const finalMeaningEl = document.getElementById("finalMeaning");

    const clearBtn = document.getElementById("clearBtn");
    const randomBtn = document.getElementById("randomBtn");

    // modal
    const modalWrap = document.getElementById("modalWrap");
    const closeModalBtn = document.getElementById("closeModalBtn");
    const saveMatrixBtn = document.getElementById("saveMatrixBtn");
    const resetMatrixBtn = document.getElementById("resetMatrixBtn");
    const wordListBox = document.getElementById("wordListBox");
    const modalTitle = document.getElementById("modalTitle");
    const modalInfo = document.getElementById("modalInfo");

    let current = MATRICES[0];
    let chosenPrefix = "";
    let chosenSuffix = "";

    let wordLists = getWordLists();
    let entries = buildEntries(current, wordLists[current.id] || []);

    function setStatus(){
      const stored = loadAllWordLists();
      const hasFull = stored[current.id] && stored[current.id].length;
      statusPillEl.innerHTML = hasFull
        ? `<div class="ok">✅ Real-word list loaded for this matrix</div>`
        : `<div class="warn">⭐ Using starter list (press Ctrl/Cmd+Shift+L to load full Word Key list)</div>`;
    }

    function renderMatrixSelect(){
      matrixSelect.innerHTML = "";
      MATRICES.forEach(m=>{
        const opt = document.createElement("option");
        opt.value = m.id;
        opt.textContent = m.label;
        matrixSelect.appendChild(opt);
      });
    }

    function renderRoot(){
      rootTextEl.textContent = current.rootLabel;
      rootAltEl.textContent  = current.rootForms?.length ? `Forms: ${current.rootForms.join(", ")}` : "";
      rootMeaningEl.textContent = current.rootMeaning;
      rootMetaEl.textContent = `Type: ${current.rootType || ""}`;
      setStatus();
    }

    function buildAllowedSuffixSetForPrefix(p){
      // From parsed entries: suffixes that appear with this prefix in real words
      const set = new Set();
      entries.forEach(e=>{
        if((e.pref || "") === (p || "")) set.add(e.suf || "");
      });
      // Always allow NONE
      set.add("");
      return set;
    }

    function buildAllowedPrefixSet(){
      // prefixes that appear in real words
      const set = new Set(entries.map(e => e.pref || ""));
      set.add("");
      return set;
    }

    function mkItem(obj, isActive, isDisabled){
      const div = document.createElement("div");
      div.className = "item" + (isActive ? " active" : "") + (isDisabled ? " disabled" : "");
      div.innerHTML = `<strong>${obj.text}</strong><span>${obj.meaning}</span>`;
      return div;
    }

    function renderLists(){
      prefixListEl.innerHTML = "";
      suffixListEl.innerHTML = "";

      const allowedPrefixes = buildAllowedPrefixSet();
      const allowedSuffixes = buildAllowedSuffixSetForPrefix(chosenPrefix);

      // prefixes
      let prefEnabledCount = 0;
      current.prefixes.forEach(p=>{
        const disabled = !allowedPrefixes.has(p.value);
        if(!disabled) prefEnabledCount++;
        const div = mkItem(p, p.value === chosenPrefix, disabled);
        if(!disabled){
          div.addEventListener("click", ()=>{
            chosenPrefix = (chosenPrefix === p.value) ? "" : p.value;
            // if current suffix becomes invalid, clear it
            const allowed = buildAllowedSuffixSetForPrefix(chosenPrefix);
            if(!allowed.has(chosenSuffix)) chosenSuffix = "";
            renderAll();
          });
        }
        prefixListEl.appendChild(div);
      });

      // suffixes
      let sufEnabledCount = 0;
      current.suffixes.forEach(s=>{
        const disabled = !allowedSuffixes.has(s.value);
        if(!disabled) sufEnabledCount++;
        const div = mkItem(s, s.value === chosenSuffix, disabled);
        if(!disabled){
          div.addEventListener("click", ()=>{
            chosenSuffix = (chosenSuffix === s.value) ? "" : s.value;
            renderAll();
          });
        }
        suffixListEl.appendChild(div);
      });

      prefCountEl.textContent = `${prefEnabledCount} options`;
      sufCountEl.textContent  = `${sufEnabledCount} options`;
    }

    function findExactWord(){
      const hit = entries.find(e => (e.pref || "") === chosenPrefix && (e.suf || "") === chosenSuffix);
      return hit ? hit.word : "";
    }

    function renderOutputs(){
      const sum = [chosenPrefix || null, current.rootLabel, chosenSuffix || null].filter(Boolean).join(" + ");
      wordSumEl.textContent = sum || current.rootLabel;

      const parts = [];
      if(chosenPrefix) parts.push(`(${chosenPrefix}-) ${prefixMeaning[chosenPrefix] || "prefix"}`);
      parts.push(`${current.rootLabel} = ${current.rootMeaning}`);
      if(chosenSuffix) parts.push(`(-${chosenSuffix}) ${suffixMeaning[chosenSuffix] || "suffix"}`);
      partMeaningsEl.textContent = parts.join(" • ");

      const word = findExactWord();
      if(word){
        finalWordEl.textContent = word;
        finalMeaningEl.textContent = autoMeaning(current, chosenPrefix, chosenSuffix);
      } else {
        finalWordEl.textContent = "—";
        finalMeaningEl.textContent = "Choose parts to build a real word.";
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
      entries = buildEntries(current, wordLists[current.id] || []);
      renderAll();
    }

    // Buttons
    clearBtn.addEventListener("click", ()=>{
      chosenPrefix = "";
      chosenSuffix = "";
      renderAll();
    });

    randomBtn.addEventListener("click", ()=>{
      const pool = entries.filter(e => (e.pref || "") !== undefined);
      if(!pool.length) return;
      // Prefer entries that can actually be clicked into (prefix/suffix known)
      const clickable = pool.filter(e => e.pref !== undefined && e.suf !== undefined);
      const pick = (clickable.length ? clickable : pool)[Math.floor(Math.random() * (clickable.length ? clickable.length : pool.length))];
      chosenPrefix = pick.pref || "";
      chosenSuffix = pick.suf || "";
      renderAll();
    });

    matrixSelect.addEventListener("change", ()=> setCurrentMatrix(matrixSelect.value));

    // ------------------------------------------------------------
    // Hidden loader modal (Ctrl/Cmd + Shift + L)
    // ------------------------------------------------------------
    function openModal(){
      modalWrap.style.display = "flex";
      modalWrap.setAttribute("aria-hidden","false");

      const stored = loadAllWordLists();
      const existing = stored[current.id] || [];
      modalTitle.textContent = `Load Word Key list — ${current.label}`;
      modalInfo.textContent = existing.length ? `Currently saved: ${existing.length} words` : `No saved list yet (starter list is being used).`;

      // Show existing list if present; else blank.
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

      // refresh
      wordLists = getWordLists();
      entries = buildEntries(current, wordLists[current.id] || []);
      closeModal();
      renderAll();
    });

    resetMatrixBtn.addEventListener("click", ()=>{
      const stored = loadAllWordLists();
      delete stored[current.id];
      saveAllWordLists(stored);

      wordLists = getWordLists();
      entries = buildEntries(current, wordLists[current.id] || []);
      closeModal();
      renderAll();
    });

    document.addEventListener("keydown", (e)=>{
      const key = e.key.toLowerCase();
      const isMac = navigator.platform.toLowerCase().includes("mac");
      const ctrlOrCmd = isMac ? e.metaKey : e.ctrlKey;

      // Ctrl/Cmd + Shift + L opens loader
      if(ctrlOrCmd && e.shiftKey && key === "l"){
        e.preventDefault();
        openModal();
      }
      // Esc closes modal
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
