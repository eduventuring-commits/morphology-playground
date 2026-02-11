index.html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Morpheme Playground (3–6)</title>
  <style>
    :root { --bg:#0b1020; --card:#121a33; --muted:#a9b2d6; --text:#eef1ff; --accent:#7aa7ff; }
    * { box-sizing: border-box; }
    body { margin:0; font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif; background:linear-gradient(180deg,#070a14, var(--bg)); color:var(--text); }
    header { padding:22px 16px 8px; max-width: 1040px; margin:0 auto; }
    h1 { margin:0 0 6px; font-size: 22px; letter-spacing: .2px; }
    .sub { color: var(--muted); margin:0; font-size: 14px; line-height: 1.45; }

    main { max-width: 1040px; margin: 0 auto; padding: 16px; }
    .grid { display:grid; gap:12px; grid-template-columns: 1fr; }
    @media (min-width: 920px){ .grid { grid-template-columns: 1.2fr .8fr; } }

    .card { background: rgba(18,26,51,.9); border: 1px solid rgba(255,255,255,.08); border-radius: 16px; padding: 14px; box-shadow: 0 8px 30px rgba(0,0,0,.25); }
    .row { display:flex; gap:10px; align-items:center; flex-wrap: wrap; }
    label { font-size: 13px; color: var(--muted); }
    select, button, input {
      background: rgba(255,255,255,.06);
      border: 1px solid rgba(255,255,255,.12);
      color: var(--text);
      padding: 10px 10px;
      border-radius: 12px;
      font-size: 14px;
      outline: none;
    }
    button { cursor:pointer; }
    button:hover { border-color: rgba(122,167,255,.6); }

    .pickerWrap { display:grid; grid-template-columns: auto 1fr auto; gap:10px; align-items:center; }
    .strip {
      display:flex; gap:10px; overflow:auto; scroll-snap-type: x mandatory;
      padding: 8px; border-radius: 14px;
      border: 1px dashed rgba(255,255,255,.14);
      background: rgba(255,255,255,.03);
    }
    .chip {
      flex: 0 0 auto;
      min-width: 168px;
      scroll-snap-align: start;
      padding: 10px 12px;
      border-radius: 14px;
      border: 1px solid rgba(255,255,255,.10);
      background: rgba(0,0,0,.12);
      user-select: none;
    }
    .chip strong { display:block; font-size: 16px; }
    .chip small { display:block; color: var(--muted); margin-top: 2px; }
    .chip.active { border-color: rgba(122,167,255,.8); box-shadow: 0 0 0 3px rgba(122,167,255,.18); }

    .matrix { display:grid; gap:12px; grid-template-columns: 1fr; }
    @media (min-width: 620px){ .matrix { grid-template-columns: 1fr 1fr 1fr; } }

    .cellTitle { color: var(--muted); font-size: 12px; margin-bottom: 8px; }
    .rootBox { text-align:center; padding: 16px 12px; border-radius: 16px; border: 1px solid rgba(255,255,255,.10); background: rgba(122,167,255,.10); }
    .rootBox .root { font-size: 30px; font-weight: 800; letter-spacing: .3px; }
    .rootBox .alts { margin-top: 6px; font-size: 13px; color: var(--muted); }
    .rootBox .meaning { color: var(--text); margin-top: 10px; font-size: 15px; font-weight: 650; }
    .rootBox .type { margin-top: 8px; font-size: 12px; color: var(--muted); }

    .outLine {
      display:grid; grid-template-columns: 1fr; gap:10px;
      padding: 10px 12px; border-radius: 14px;
      border: 1px solid rgba(255,255,255,.10); background: rgba(255,255,255,.03);
      margin-top: 12px;
    }
    @media (min-width: 620px){ .outLine { grid-template-columns: 1fr 1fr; } }
    .outLine .k { color: var(--muted); font-size: 12px; }
    .outLine .v { font-size: 18px; font-weight: 800; margin-top: 2px; }

    .meaningBox { margin-top: 12px; padding: 12px; border-radius: 14px; border: 1px solid rgba(255,255,255,.10); background: rgba(0,0,0,.12); }
    .meaningBox h3 { margin:0 0 8px; font-size: 14px; color: var(--muted); font-weight: 650; }
    .meaningBox p { margin:0; line-height: 1.45; }
    .tiny { font-size: 12px; color: var(--muted); margin-top: 8px; }
    .badge { display:inline-block; padding: 2px 8px; border-radius: 999px; background: rgba(122,167,255,.18); border: 1px solid rgba(122,167,255,.35); color: var(--text); font-size: 12px; }
    .hint { color: var(--muted); font-size: 13px; margin-top: 8px; line-height: 1.45; }
    code { font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace; font-size: 12px; }
  </style>
</head>
<body>
  <header>
    <h1>Morpheme Playground</h1>
    <p class="sub">
      Pick a <span class="badge">root/base</span>. Then scroll to choose a <span class="badge">prefix</span> and <span class="badge">suffix</span>.
      Read the word sum and the meaning.
    </p>
  </header>

  <main class="grid">
    <section class="card">
      <div class="row" style="justify-content:space-between;">
        <div style="min-width: 280px;">
          <label for="matrixSelect">Choose a root/base:</label><br/>
          <select id="matrixSelect"></select>
          <div class="hint" id="rootHint"></div>
        </div>
        <div class="row">
          <button id="randomBtn" title="Pick random prefix/suffix">Random</button>
          <button id="clearBtn" title="Clear prefix/suffix">Clear</button>
        </div>
      </div>

      <div style="margin-top:14px;" class="matrix">
        <div>
          <div class="cellTitle">Prefixes (scroll + click)</div>
          <div class="pickerWrap">
            <button id="prefLeft" aria-label="Scroll prefixes left">◀</button>
            <div id="prefixStrip" class="strip" aria-label="Prefix picker"></div>
            <button id="prefRight" aria-label="Scroll prefixes right">▶</button>
          </div>
        </div>

        <div>
          <div class="cellTitle">Root/Base</div>
          <div class="rootBox">
            <div class="root" id="rootText">—</div>
            <div class="alts" id="rootAlts"></div>
            <div class="meaning" id="rootMeaning">—</div>
            <div class="type" id="rootType">—</div>
          </div>
        </div>

        <div>
          <div class="cellTitle">Suffixes (scroll + click)</div>
          <div class="pickerWrap">
            <button id="sufLeft" aria-label="Scroll suffixes left">◀</button>
            <div id="suffixStrip" class="strip" aria-label="Suffix picker"></div>
            <button id="sufRight" aria-label="Scroll suffixes right">▶</button>
          </div>
        </div>
      </div>

      <div class="outLine">
        <div>
          <div class="k">Word sum</div>
          <div class="v" id="wordSum">—</div>
        </div>
        <div>
          <div class="k">Word</div>
          <div class="v" id="wordFinal">—</div>
        </div>
      </div>

      <div class="meaningBox">
        <h3>Word-part meanings</h3>
        <p id="partMeanings">—</p>
        <div class="tiny" id="noteLine"></div>
      </div>

      <div class="meaningBox">
        <h3>Word meaning</h3>
        <p id="wordMeaning">Choose a prefix and/or suffix to see a combined meaning.</p>
      </div>

      <p class="tiny">Tip: Click a selected chip again to turn it off.</p>
    </section>

    <section class="card">
      <h2 style="margin:0 0 10px; font-size:16px;">Teacher / Product Notes</h2>

      <div class="meaningBox">
        <h3>What you can sell (easy upgrades)</h3>
        <p class="sub">
          1) Add more roots (already set up) • 2) Add “Challenge Mode” prompts • 3) Add audio (read parts aloud) •
          4) Add built-in definitions for common real words (glossary).
        </p>
      </div>

      <div class="meaningBox">
        <h3>How “Word meaning” works</h3>
        <p class="sub">
          The app makes a kid-friendly meaning from the parts.
          For cleaner definitions (like “transport = carry across”), add entries to <code>wordGlossary</code>.
        </p>
      </div>

      <div class="meaningBox">
        <h3>Hosting (free)</h3>
        <p class="sub">
          This is one file (<code>index.html</code>). You can host on GitHub Pages or Netlify, or embed in Google Sites.
        </p>
      </div>
    </section>
  </main>

  <script>
    // =========================
    // 3–6 FRIENDLY MORPHEME SETS
    // =========================

    const DEFAULT_PREFIXES = [
      { text: "—", meaning: "(no prefix)", value: "" },
      { text: "re", meaning: "again", value: "re" },
      { text: "un", meaning: "not / undo", value: "un" },
      { text: "dis", meaning: "not / opposite", value: "dis" },
      { text: "mis", meaning: "wrong / badly", value: "mis" },
      { text: "pre", meaning: "before", value: "pre" },
      { text: "non", meaning: "not", value: "non" },
      { text: "over", meaning: "too much", value: "over" },
      { text: "under", meaning: "not enough / below", value: "under" },
      { text: "sub", meaning: "under / below", value: "sub" },
      { text: "super", meaning: "above / extra", value: "super" },
      { text: "inter", meaning: "between", value: "inter" },
      { text: "trans", meaning: "across", value: "trans" },
      { text: "tele", meaning: "far / distant", value: "tele" },
      { text: "bio", meaning: "life", value: "bio" },
      { text: "geo", meaning: "earth", value: "geo" },
      { text: "auto", meaning: "self", value: "auto" },
      { text: "micro", meaning: "small", value: "micro" },
      { text: "photo", meaning: "light", value: "photo" },
      { text: "demo", meaning: "people", value: "demo" },
      { text: "techno", meaning: "skill / art", value: "techno" }
    ];

    const DEFAULT_SUFFIXES = [
      { text: "—", meaning: "(no suffix)", value: "" },
      { text: "s", meaning: "plural / 3rd-person verb", value: "s" },
      { text: "ed", meaning: "past tense", value: "ed" },
      { text: "ing", meaning: "happening now", value: "ing" },
      { text: "er", meaning: "a person/thing that", value: "er" },
      { text: "or", meaning: "a person/thing that", value: "or" },
      { text: "ion", meaning: "act / process", value: "ion" },
      { text: "tion", meaning: "act / process", value: "tion" },
      { text: "ment", meaning: "result / thing", value: "ment" },
      { text: "able", meaning: "can be", value: "able" },
      { text: "ive", meaning: "tending to / having", value: "ive" },
      { text: "al", meaning: "relating to", value: "al" },
      { text: "ful", meaning: "full of", value: "ful" },
      { text: "less", meaning: "without", value: "less" },
      { text: "ology", meaning: "study of", value: "ology" },
      { text: "meter", meaning: "measure", value: "meter" },
      { text: "scope", meaning: "watch / see", value: "scope" },
      { text: "graph", meaning: "written / drawn", value: "graph" },
      { text: "gram", meaning: "written / drawn", value: "gram" },
      { text: "cracy", meaning: "rule / government", value: "cracy" },
      { text: "crat", meaning: "a person who rules", value: "crat" }
    ];

    // =========================
    // YOUR ROOT/BASE LIST
    // =========================
    // Each matrix can also include a custom wordGlossary for better kid-friendly definitions.
    const MATRICES = [
      mkRoot("form", ["form"], "to shape", "free", { inform: "to tell someone", reform: "to change to make better", deform: "to change shape in a bad way", "informal": "not fancy; relaxed" }),
      mkRoot("port", ["port"], "to carry", "bound", { transport: "carry across", import: "carry into", export: "carry out", portable: "easy to carry" }),
      mkRoot("rupt", ["rupt"], "to break or burst", "bound", { erupt: "burst out", disrupt: "break up / interrupt" }),
      mkRoot("tract", ["tract"], "to draw or pull", "bound", { attract: "pull toward", retract: "pull back", distract: "pull attention away" }),
      mkRoot("scrib / script", ["scrib","script"], "to write", "bound", { describe: "write/tell details about", transcript: "written copy", scribble: "write messily (kid-friendly)" }),
      mkRoot("spect", ["spect"], "to see / watch / observe", "bound", { inspect: "look at closely", respect: "to look up to", spectator: "a watcher" }),
      mkRoot("struct", ["struct"], "to build", "bound", { construct: "build", destruct: "break down", instruct: "teach; tell how" }),
      mkRoot("flect / flex", ["flect","flex"], "to bend or curve", "bound", { reflect: "bend back; think back", flexible: "can bend easily" }),
      mkRoot("dict", ["dict"], "to say or tell", "bound", { predict: "say before", verdict: "final decision said" }),
      mkRoot("fer", ["fer"], "to bear or carry / yield", "bound", { transfer: "carry across", prefer: "like more" }),
      mkRoot("mit / miss", ["mit","miss"], "to send", "bound", { transmit: "send across", submit: "send under; turn in", dismiss: "send away" }),
      mkRoot("duce / duct", ["duce","duct"], "to lead", "bound", { reduce: "lead back / make smaller", conduct: "lead; behave" }),
      mkRoot("vers / vert", ["vers","vert"], "to turn", "bound", { convert: "turn into", reverse: "turn back", invert: "turn upside down" }),
      mkRoot("fact / fect / fict", ["fact","fect","fict"], "to make or do", "bound", { manufacture: "make", effective: "works well", fiction: "made-up story" }),
      mkRoot("tend / tens / tent", ["tend","tens","tent"], "to stretch or strain", "bound", { extend: "stretch out", tension: "tight pulling feeling" }),
      mkRoot("ceipt / ceive / cept", ["ceipt","ceive","cept"], "to take or catch", "bound", { receive: "take in", accept: "take / agree to", intercept: "catch in between" }),
      mkRoot("tain / ten / tin", ["tain","ten","tin"], "to hold", "bound", { contain: "hold inside", maintain: "keep holding / keep up" }),

      // Combo/other roots you listed:
      mkRoot("phon / phono", ["phon","phono"], "sound", "bound", { telephone: "sound from far away", microphone: "small sound tool" }),
      mkRoot("scope", ["scope"], "watch / see", "bound", { microscope: "tool to see small things", telescope: "tool to see far things" }),
      mkRoot("photo", ["photo"], "light", "bound", { photograph: "picture made with light" }),
      mkRoot("metro", ["metro"], "city or measure", "bound", { metropolis: "big city" }),
      mkRoot("gram / graph", ["gram","graph"], "written or drawn", "bound", { autograph: "your written name", paragraph: "a block of writing" }),
      mkRoot("dem / demo", ["dem","demo"], "people", "bound", { democracy: "rule by the people" }),
      mkRoot("meter / metr", ["meter","metr"], "measure", "bound", { thermometer: "measure heat", perimeter: "measure around" }),
      mkRoot("geo", ["geo"], "earth", "bound", { geology: "study of earth" }),
      mkRoot("tele", ["tele"], "distant / far", "bound", { telescope: "see far away", telegraph: "send writing far away" }),
      mkRoot("techn", ["techn"], "skill or art", "bound", { technology: "tools made from skill" }),
      mkRoot("bio", ["bio"], "life", "bound", { biology: "study of life" }),
      mkRoot("chron / chrono", ["chron","chrono"], "time", "bound", { chronological: "in time order" }),
      mkRoot("micro", ["micro"], "small", "bound", { microscope: "tool to see small things", microbe: "tiny living thing" }),
      mkRoot("psych", ["psych"], "mind or soul", "bound", { psychology: "study of the mind" }),
      mkRoot("hydra / hydro", ["hydra","hydro"], "water", "bound", { hydrate: "add water", hydroelectric: "power from water" }),
      mkRoot("auto", ["auto"], "self", "bound", { autograph: "self-written name", automatic: "works by itself" }),
      mkRoot("therm / thermo", ["therm","thermo"], "heat / hot", "bound", { thermometer: "tool that measures heat" }),

      // Endings you listed:
      mkRoot("logy / ology", ["logy","ology"], "study of", "suffix", { geology: "study of earth", biology: "study of life" }),
      mkRoot("cracy / crat", ["cracy","crat"], "government / ruler", "suffix", { democracy: "rule by the people", autocrat: "one ruler" }),
    ];

    // Build a matrix from a root list with defaults, but allow custom glossary + hinting
    function mkRoot(id, variants, meaning, type, wordGlossary = null) {
      return {
        id,
        root: variants[0],
        variants,
        rootMeaning: meaning,
        rootType: type, // free | bound | suffix
        prefixes: DEFAULT_PREFIXES,
        suffixes: DEFAULT_SUFFIXES,
        wordGlossary: wordGlossary || {}
      };
    }

    // =========================
    // UI STATE + RENDERING
    // =========================
    let currentMatrix = MATRICES[0];
    let selectedPrefix = currentMatrix.prefixes[0];
    let selectedSuffix = currentMatrix.suffixes[0];

    const el = (id) => document.getElementById(id);

    function buildWord(prefixVal, rootVal, suffixVal) {
      return `${prefixVal}${rootVal}${suffixVal}`.replace(/\s+/g, "");
    }

    function computedWordMeaning(matrix, prefixObj, suffixObj) {
      const finalWord = buildWord(prefixObj.value, matrix.root, suffixObj.value).toLowerCase();
      if (matrix.wordGlossary && matrix.wordGlossary[finalWord]) return matrix.wordGlossary[finalWord];

      const p = prefixObj.value ? prefixObj.meaning : "";
      const r = matrix.rootMeaning;
      const s = suffixObj.value ? suffixObj.meaning : "";

      // Kid-friendly sentence stems
      if (prefixObj.value && suffixObj.value) {
        return `A word that means: ${p} + ${r}, and it shows ${s}.`;
      }
      if (prefixObj.value) {
        return `A word that means: ${p} + ${r}.`;
      }
      if (suffixObj.value) {
        return `A word that means: ${r}, and it shows ${s}.`;
      }
      return `This root means: ${r}.`;
    }

    function renderMatrixSelect() {
      const sel = el("matrixSelect");
      sel.innerHTML = "";
      MATRICES.forEach((m, i) => {
        const opt = document.createElement("option");
        opt.value = m.id;
        const label = `${m.variants.join(", ")} — ${m.rootMeaning}`;
        opt.textContent = label;
        if (i === 0) opt.selected = true;
        sel.appendChild(opt);
      });
    }

    function chip(prefixOrSuffix, item, isActive) {
      const d = document.createElement("div");
      d.className = "chip" + (isActive ? " active" : "");
      d.tabIndex = 0;
      d.role = "button";
      d.setAttribute("aria-pressed", String(isActive));
      d.innerHTML = `<strong>${item.text}</strong><small>${item.meaning}</small>`;

      const toggle = () => {
        if (prefixOrSuffix === "prefix") {
          selectedPrefix = (selectedPrefix === item) ? currentMatrix.prefixes[0] : item;
        } else {
          selectedSuffix = (selectedSuffix === item) ? currentMatrix.suffixes[0] : item;
        }
        renderAll();
      };

      d.addEventListener("click", toggle);
      d.addEventListener("keydown", (e) => {
        if (e.key === "Enter" || e.key === " ") { e.preventDefault(); toggle(); }
      });
      return d;
    }

    function renderStrips() {
      const pStrip = el("prefixStrip");
      const sStrip = el("suffixStrip");
      pStrip.innerHTML = "";
      sStrip.innerHTML = "";
      currentMatrix.prefixes.forEach((p) => pStrip.appendChild(chip("prefix", p, p === selectedPrefix)));
      currentMatrix.suffixes.forEach((s) => sStrip.appendChild(chip("suffix", s, s === selectedSuffix)));
    }

    function renderRoot() {
      el("rootText").textContent = currentMatrix.root;
      el("rootAlts").textContent =
        (currentMatrix.variants && currentMatrix.variants.length > 1)
          ? `Also seen as: ${currentMatrix.variants.slice(1).join(", ")}`
          : "";
      el("rootMeaning").textContent = currentMatrix.rootMeaning;
      el("rootType").textContent = currentMatrix.rootType ? `Type: ${currentMatrix.rootType}` : "";

      // A tiny hint line for kids
      const examples =
        Object.keys(currentMatrix.wordGlossary || {}).slice(0, 3);
      el("rootHint").textContent =
        examples.length
          ? `Examples you might know: ${examples.join(", ")}`
          : "Try making words—then see if you recognize a real one!";
    }

    function renderOutputs() {
      const sumParts = [
        selectedPrefix.value ? selectedPrefix.text : null,
        currentMatrix.root,
        selectedSuffix.value ? selectedSuffix.text : null
      ].filter(Boolean);

      el("wordSum").textContent = sumParts.join(" + ") || currentMatrix.root;

      const finalWord = buildWord(selectedPrefix.value, currentMatrix.root, selectedSuffix.value);
      el("wordFinal").textContent = finalWord || currentMatrix.root;

      const partMeaningLine = [
        selectedPrefix.value ? `(${selectedPrefix.text}-) ${selectedPrefix.meaning}` : null,
        `${currentMatrix.root} = ${currentMatrix.rootMeaning}`,
        selectedSuffix.value ? `(-${selectedSuffix.text}) ${selectedSuffix.meaning}` : null
      ].filter(Boolean).join(" • ");

      el("partMeanings").textContent = partMeaningLine || `${currentMatrix.root} = ${currentMatrix.rootMeaning}`;

      const wm = computedWordMeaning(currentMatrix, selectedPrefix, selectedSuffix);
      el("wordMeaning").textContent = wm;

      const finalKey = (finalWord || currentMatrix.root).toLowerCase();
      const hasGloss = currentMatrix.wordGlossary && currentMatrix.wordGlossary[finalKey];
      el("noteLine").textContent = hasGloss ? "Using glossary definition ✨" : "Using generated meaning (teacher can override).";
    }

    function scrollStrip(which, dir) {
      const strip = which === "prefix" ? el("prefixStrip") : el("suffixStrip");
      strip.scrollBy({ left: dir * 240, behavior: "smooth" });
    }

    function setMatrixById(id) {
      currentMatrix = MATRICES.find(m => m.id === id) || MATRICES[0];
      selectedPrefix = currentMatrix.prefixes[0];
      selectedSuffix = currentMatrix.suffixes[0];
      renderAll();
    }

    function randomPick() {
      const p = currentMatrix.prefixes[Math.floor(Math.random() * currentMatrix.prefixes.length)];
      const s = currentMatrix.suffixes[Math.floor(Math.random() * currentMatrix.suffixes.length)];
      selectedPrefix = p;
      selectedSuffix = s;
      renderAll();
    }

    function clearPick() {
      selectedPrefix = currentMatrix.prefixes[0];
      selectedSuffix = currentMatrix.suffixes[0];
      renderAll();
    }

    function renderAll() {
      renderRoot();
      renderStrips();
      renderOutputs();
    }

    // INIT
    renderMatrixSelect();
    setMatrixById(MATRICES[0].id);

    el("matrixSelect").addEventListener("change", (e) => setMatrixById(e.target.value));
    el("prefLeft").addEventListener("click", () => scrollStrip("prefix", -1));
    el("prefRight").addEventListener("click", () => scrollStrip("prefix", 1));
    el("sufLeft").addEventListener("click", () => scrollStrip("suffix", -1));
    el("sufRight").addEventListener("click", () => scrollStrip("suffix", 1));
    el("randomBtn").addEventListener("click", randomPick);
    el("clearBtn").addEventListener("click", clearPick);
  </script>
</body>
</html>
