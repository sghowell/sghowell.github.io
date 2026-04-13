# Personal Website Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build and deploy a minimal static personal website for Sean Howell at `https://sghowell.github.io`, implementing the design spec at `docs/superpowers/specs/2026-04-13-personal-website-design.md`.

**Architecture:** Two primary source files (`index.html`, `style.css`) plus a self-hosted variable font under `fonts/`, the resume PDF under `assets/`, and a `.nojekyll` marker. Zero JavaScript, zero build tools, deployed from the `main` branch root via GitHub Pages user-site auto-publishing.

**Tech Stack:** HTML5, CSS3 (custom properties, `prefers-color-scheme`, grid), Berkeley Mono Variable (self-hosted `.woff2`), inline SVG for icons, GitHub Pages.

**Spec reference:** `docs/superpowers/specs/2026-04-13-personal-website-design.md` — read this first. Content quotes (bios, job descriptions, publications) should be taken verbatim from the spec.

**TDD note:** This is a pure static-content project with no executable code paths. "Tests" are replaced by explicit verification steps — file existence checks, byte-level content inspections, local browser preview with JavaScript disabled, and a final HTTP verification against the deployed URL. Automated unit tests would add zero value here.

---

## File Structure

```
sghowell.github.io/
├── .nojekyll                        # NEW — empty file, disables Jekyll
├── index.html                       # NEW — all page content + inline SVG icons
├── style.css                        # NEW — all styling (@font-face, vars, layout, dark mode)
├── README.md                        # MODIFY — replace one-liner with dev notes
├── fonts/
│   ├── BerkeleyMonoVariable.woff2   # NEW — copied from ~/Downloads/...
│   └── LICENSE.md                   # NEW — font license acknowledgment
├── assets/
│   └── seanhowell-resume-042026.pdf # MOVED from repo root
├── LICENSE                          # UNCHANGED
├── .gitignore                       # UNCHANGED (already has .superpowers/)
└── docs/superpowers/...             # UNCHANGED (spec + this plan)
```

**Responsibility per file:**

- `index.html` — content, semantic structure, document meta, inline SVGs. Should be readable top-to-bottom as a single document.
- `style.css` — font face declaration, CSS custom properties (light + dark), layout grid, typography scale, entry styles, link/icon styles. One file; no fragmentation.
- `fonts/BerkeleyMonoVariable.woff2` — the variable font file. Referenced only via CSS `@font-face`.
- `fonts/LICENSE.md` — plain-text note about the personal license.
- `assets/seanhowell-resume-042026.pdf` — the downloadable resume, linked from the masthead.
- `.nojekyll` — empty file at repo root that tells GitHub Pages not to run Jekyll.
- `README.md` — short dev notes (local preview command, deploy note, font licensing note, link to spec).

---

## Chunk 1: Scaffolding and assets

### Task 1: Create directory skeleton and move the resume PDF

**Files:**
- Create dir: `fonts/`
- Create dir: `assets/`
- Move: `seanhowell-resume-042026.pdf` → `assets/seanhowell-resume-042026.pdf`

- [ ] **Step 1: Create the two new directories**

```bash
mkdir -p fonts assets
```

- [ ] **Step 2: Move the resume PDF using `git mv` to preserve history**

```bash
git mv seanhowell-resume-042026.pdf assets/seanhowell-resume-042026.pdf
```

- [ ] **Step 3: Verify the move**

```bash
ls -la assets/ && ls -la . | grep -c 'seanhowell-resume' || true
```
Expected: `seanhowell-resume-042026.pdf` appears in `assets/`, and the repo root no longer has a top-level copy.

- [ ] **Step 4: Commit**

```bash
git add -A
git commit -m "chore: relocate resume PDF to assets/"
```

---

### Task 2: Install Berkeley Mono font file and license note

**Files:**
- Create: `fonts/BerkeleyMonoVariable.woff2` (copied from `~/Downloads/260413562L7Y7245/TX-02-1WL5N5WL/Berkeley Mono Variable.woff2`)
- Create: `fonts/LICENSE.md`

- [ ] **Step 1: Copy the font file**

```bash
cp "/Users/seanhowell/Downloads/260413562L7Y7245/TX-02-1WL5N5WL/Berkeley Mono Variable.woff2" fonts/BerkeleyMonoVariable.woff2
```

- [ ] **Step 2: Verify the file arrived and is the expected format**

```bash
ls -la fonts/BerkeleyMonoVariable.woff2
file fonts/BerkeleyMonoVariable.woff2
```
Expected: file exists, ~166KB, and `file` reports a Web Open Font Format 2 file.

- [ ] **Step 3: Write the font license acknowledgment**

Create `fonts/LICENSE.md` with exactly this content:

```markdown
# Font License Notice

This directory contains `BerkeleyMonoVariable.woff2`, used under a Berkeley Graphics **Personal Font License** (Document ID DX-100-05, April 2023) issued to Sean Howell.

The font is used solely for Web Font Use as permitted by §1.08 and §2 of the license:
the `.woff2` file is referenced only from `/style.css` via the CSS `@font-face` rule.

**Do not redistribute.** The file is not to be copied, linked to directly, or used
for any purpose other than rendering `sghowell.github.io` in a web browser.
Commercial use is not permitted.

For license inquiries: license@berkeleygraphics.com
```

- [ ] **Step 4: Verify and commit**

```bash
cat fonts/LICENSE.md | head -5
git add fonts/
git commit -m "fonts: add Berkeley Mono variable woff2 and license note"
```

---

### Task 3: Create the `.nojekyll` marker

**Files:**
- Create: `.nojekyll` (empty)

- [ ] **Step 1: Create the empty file**

```bash
touch .nojekyll
```

- [ ] **Step 2: Verify**

```bash
ls -la .nojekyll
```
Expected: file exists, size 0.

- [ ] **Step 3: Commit**

```bash
git add .nojekyll
git commit -m "chore: add .nojekyll to disable Jekyll processing"
```

---

## Chunk 2: Stylesheet

The stylesheet is written as one coherent file. Build it in one task with the complete content, then verify it loads in the next chunk when paired with HTML.

### Task 4: Write `style.css` in full

**Files:**
- Create: `style.css`

- [ ] **Step 1: Write the complete stylesheet**

Create `style.css` with the following content. Do not split into partials; a single file is clearer for this size.

```css
/* ==========================================================================
   sghowell.github.io — stylesheet
   Berkeley Mono, monochrome, zero dependencies, zero JS.
   ========================================================================== */

@font-face {
  font-family: "Berkeley Mono";
  src: url("/fonts/BerkeleyMonoVariable.woff2") format("woff2");
  font-weight: 100 900;
  font-style: normal;
  font-display: swap;
}

:root {
  --font: "Berkeley Mono", ui-monospace, "SF Mono", Menlo, Consolas, monospace;

  --bg: #fafafa;
  --fg: #111111;
  --muted: #666666;
  --faint: #999999;
  --rule: #111111;
  --subtle: #e5e5e5;
}

@media (prefers-color-scheme: dark) {
  :root {
    --bg: #0e0e0e;
    --fg: #ededed;
    --muted: #8a8a8a;
    --faint: #5f5f5f;
    --rule: #ededed;
    --subtle: #1f1f1f;
  }
}

*, *::before, *::after { box-sizing: border-box; }

html { -webkit-text-size-adjust: 100%; }

body {
  margin: 0;
  background: var(--bg);
  color: var(--fg);
  font-family: var(--font);
  font-size: 14px;
  line-height: 1.6;
  font-feature-settings: "ss01", "ss02";
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.container {
  max-width: 78ch;
  margin: 0 auto;
  padding: 48px 32px 96px;
}

@media (max-width: 640px) {
  .container { padding: 32px 20px 64px; }
}

/* ----- Masthead ----- */

.masthead { margin-bottom: 32px; }

.masthead h1 {
  margin: 0 0 4px;
  font-size: 1.625rem;
  font-weight: 700;
  letter-spacing: 0.06em;
  text-transform: uppercase;
}

.masthead .tagline {
  color: var(--muted);
  font-size: 0.8125rem;
  letter-spacing: 0.02em;
  margin: 0 0 2px;
}

.masthead .location {
  color: var(--muted);
  font-size: 0.75rem;
  margin: 0 0 16px;
}

.anchor-rail {
  font-size: 0.75rem;
  color: var(--muted);
  letter-spacing: 0.05em;
  margin: 0 0 8px;
}

.anchor-rail a {
  color: var(--muted);
  text-decoration: none;
  border-bottom: 1px solid transparent;
  padding-bottom: 1px;
}

.anchor-rail a:hover,
.anchor-rail a:focus-visible {
  color: var(--fg);
  border-bottom-color: var(--fg);
}

.anchor-rail .sep {
  margin: 0 6px;
  color: var(--faint);
}

.masthead .resume-link {
  display: inline-block;
  margin-top: 12px;
  font-size: 0.75rem;
  color: var(--fg);
  text-decoration: none;
  border-bottom: 1px solid var(--fg);
  padding-bottom: 1px;
}

.masthead .resume-link:hover,
.masthead .resume-link:focus-visible {
  border-bottom-width: 2px;
}

/* ----- Sections ----- */

section {
  border-top: 1px solid var(--rule);
  padding-top: 14px;
  margin-top: 28px;
}

section:first-of-type { margin-top: 0; }

section > h2 {
  font-size: 0.8125rem;
  font-weight: 700;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--muted);
  margin: 0 0 16px;
}

section > h2::before { content: "// "; color: var(--faint); }

/* ----- About ----- */

.about p {
  margin: 0 0 12px;
  max-width: 72ch;
}

.about .edu {
  color: var(--muted);
  font-size: 0.75rem;
  margin-top: 16px;
}

/* ----- Entries (experience, projects, pubs) ----- */

.entry {
  display: grid;
  grid-template-columns: 150px 1fr;
  gap: 24px;
  padding: 14px 0;
  border-bottom: 1px solid var(--subtle);
}

.entry:last-of-type { border-bottom: 0; }

.entry .lhs {
  color: var(--muted);
  font-size: 0.75rem;
  padding-top: 1px;
  letter-spacing: 0.02em;
}

.entry .rhs .title {
  font-weight: 700;
  font-size: 0.875rem;
  color: var(--fg);
}

.entry .rhs .sub {
  color: var(--muted);
  font-size: 0.75rem;
  margin-top: 2px;
}

.entry .rhs ul {
  margin: 8px 0 0;
  padding-left: 16px;
  color: var(--muted);
  font-size: 0.8125rem;
  line-height: 1.55;
}

.entry .rhs ul li { margin-bottom: 4px; }

.entry .rhs .desc {
  color: var(--muted);
  font-size: 0.8125rem;
  margin-top: 4px;
  line-height: 1.55;
}

@media (max-width: 640px) {
  .entry { grid-template-columns: 1fr; gap: 6px; }
  .entry .lhs { font-size: 0.6875rem; }
}

/* ----- Projects group headings ----- */

.project-group {
  color: var(--faint);
  font-size: 0.6875rem;
  font-weight: 700;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  margin: 22px 0 4px;
}

.project-group:first-child { margin-top: 0; }

.project .lhs .lang {
  display: inline-block;
  font-size: 0.6875rem;
  color: var(--muted);
}

.project .lhs .visibility {
  display: inline-block;
  font-size: 0.6875rem;
  color: var(--faint);
  margin-left: 6px;
}

/* ----- Prior positions dense list ----- */

.prior {
  margin-top: 14px;
  padding-top: 14px;
  border-top: 1px solid var(--subtle);
  list-style: none;
  padding-left: 0;
}

.prior li {
  display: grid;
  grid-template-columns: 150px 1fr;
  gap: 24px;
  padding: 6px 0;
  font-size: 0.8125rem;
  color: var(--muted);
}

.prior li .lhs { color: var(--muted); font-size: 0.75rem; }

.prior li .rhs .title { color: var(--fg); font-weight: 700; }

@media (max-width: 640px) {
  .prior li { grid-template-columns: 1fr; gap: 2px; padding: 8px 0; }
}

/* ----- Publications ----- */

.pubs li {
  padding: 10px 0;
  font-size: 0.8125rem;
  color: var(--muted);
  border-bottom: 1px solid var(--subtle);
  line-height: 1.55;
}

.pubs li:last-child { border-bottom: 0; }

.pubs em { color: var(--fg); font-style: normal; }

.pubs ol { list-style: decimal; padding-left: 20px; margin: 0; }

/* ----- Contact ----- */

.contact-list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.contact-list a {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  color: var(--fg);
  text-decoration: none;
  font-size: 0.875rem;
  border-bottom: 1px solid transparent;
  padding-bottom: 1px;
  width: fit-content;
}

.contact-list a:hover,
.contact-list a:focus-visible {
  border-bottom-color: var(--fg);
}

.icon {
  width: 16px;
  height: 16px;
  display: inline-block;
  flex-shrink: 0;
  color: currentColor;
}

/* ----- Focus styles ----- */

:focus-visible { outline: 2px solid var(--fg); outline-offset: 2px; }

a { color: var(--fg); }

/* ----- Footer ----- */

.site-footer {
  margin-top: 48px;
  padding-top: 14px;
  border-top: 1px solid var(--subtle);
  color: var(--faint);
  font-size: 0.6875rem;
  letter-spacing: 0.04em;
}
```

- [ ] **Step 2: Verify the file is syntactically valid CSS**

```bash
wc -l style.css
```
Expected: ~220 lines. No syntax checker is strictly required — the browser will flag errors during the verification task at the end of Chunk 3.

- [ ] **Step 3: Commit**

```bash
git add style.css
git commit -m "style: add sitewide stylesheet"
```

---

## Chunk 3: HTML document

`index.html` is written in a single task. The file will be ~300-350 lines including inline SVGs. Content prose must match the spec verbatim where the spec quotes it.

### Task 5: Write `index.html` in full

**Files:**
- Create: `index.html`

- [ ] **Step 1: Write the complete HTML document**

Create `index.html` with the following content. This is the entire file — do not omit any section.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Sean Howell — Researcher, Engineer, Technical Leader</title>
  <meta name="description" content="Sean Howell — researcher and engineer building AI agents, models, and infrastructure for machine-driven scientific discovery in physics and mathematics.">
  <link rel="stylesheet" href="/style.css">
</head>
<body>
  <main class="container">

    <header class="masthead">
      <h1>Sean Howell</h1>
      <p class="tagline">researcher // engineer // technical leader</p>
      <p class="location">Los Angeles &middot; San Francisco Bay Area</p>
      <nav class="anchor-rail" aria-label="Sections">
        <a href="#about">about</a><span class="sep">/</span><a href="#experience">experience</a><span class="sep">/</span><a href="#projects">projects</a><span class="sep">/</span><a href="#publications">publications</a><span class="sep">/</span><a href="#contact">contact</a>
      </nav>
      <a class="resume-link" href="/assets/seanhowell-resume-042026.pdf">[ download resume (pdf) ]</a>
    </header>

    <section id="about" class="about">
      <h2>about</h2>
      <p>Researcher and engineer focused on AI-driven scientific discovery &mdash; building agents, models, and infrastructure that allow machines to participate meaningfully in physics and mathematics research and control complex systems.</p>
      <p>Recent work includes neural decoders and RL for fault-tolerant quantum computing, VLA models for autonomous experimental science, autonomous agent systems for physics research, formalization and verification, and compiler toolchains grounded in ZX-Calculus. Fifteen years in AI/ML, with R&amp;D leadership across three quantum computing organizations.</p>
      <p class="edu">B.S. Physics &amp; Mathematics, University of Maryland, College Park.</p>
    </section>

    <section id="experience">
      <h2>experience</h2>

      <div class="entry">
        <div class="lhs">2024 &mdash; Present</div>
        <div class="rhs">
          <div class="title">Principal Engineer, AI for Quantum &amp; Director, Quantum OS</div>
          <div class="sub">PsiQuantum &middot; Palo Alto, CA</div>
          <ul>
            <li>Founded and leading AI for Quantum R&amp;D: ML-based QEC decoders, RL for adaptivity in fault-tolerant quantum computing, scientific agents for physics research, multi-agent systems for calibration, control, and operation of quantum systems.</li>
            <li>Building agentic discovery infrastructure for scientific AI: harnesses, trajectory capture, structured evaluation of AI-generated scientific artifacts.</li>
            <li>Directed Quantum OS: designed scalable control plane, fabrics, OS software for utility-scale photonic QC. Grew engineering org by 15+ ICs YoY.</li>
          </ul>
        </div>
      </div>

      <div class="entry">
        <div class="lhs">2025 &mdash; Present</div>
        <div class="rhs">
          <div class="title">Independent Research &amp; Development &mdash; AI for Science &amp; Systems</div>
          <div class="sub">Los Angeles, CA</div>
          <ul>
            <li>Developing architectures for AI-assisted scientific reasoning: evaluation, harnesses, provenance systems, and long-horizon agent frameworks for physics and mathematics.</li>
            <li>Research at the intersection of reasoning models and formal methods, targeting formalization of results and open problems in quantum information science.</li>
            <li>Structured representations of reasoning (typed graphs over claims, evidence, derivations, simulations, proofs) for verifiable multi-agent collaboration.</li>
          </ul>
        </div>
      </div>

      <div class="entry">
        <div class="lhs">2022 &mdash; 2024</div>
        <div class="rhs">
          <div class="title">Software Engineering Manager</div>
          <div class="sub">AWS Center for Quantum Computing &middot; Pasadena, CA</div>
          <ul>
            <li>Led engineering and research in quantum compilers, control software/hardware, QEC frameworks, and a distributed runtime for quantum information processing systems.</li>
            <li>Built and mentored a team of scientists and engineers (0&ndash;30 years experience) developing scalable, fault-tolerant quantum computing systems.</li>
          </ul>
        </div>
      </div>

      <div class="entry">
        <div class="lhs">2019 &mdash; 2022</div>
        <div class="rhs">
          <div class="title">Head of US Division</div>
          <div class="sub">Q-CTRL &middot; Los Angeles, CA</div>
          <ul>
            <li>Led US R&amp;D, software engineering, and product. Grew team 0&rarr;20 engineers, drove 10&times; YoY revenue growth, achieved 9000&times; algorithm performance improvement.</li>
            <li>First demonstration of reinforcement learning for gateset design and calibration on a superconducting quantum processor. Pioneering work in AI for NISQ hardware.</li>
          </ul>
        </div>
      </div>

      <ul class="prior" aria-label="Prior positions">
        <li><div class="lhs">2018 &mdash; 2019</div><div class="rhs"><span class="title">Quantitative Research Engineer, BlackRock.</span> Quantitative research and ML for systematic trading strategies.</div></li>
        <li><div class="lhs">2017</div><div class="rhs"><span class="title">AI Software Engineer, Cogitai.</span> Continual learning research and RL for robotics.</div></li>
        <li><div class="lhs">2016 &mdash; 2017</div><div class="rhs"><span class="title">Senior Scientist, Areté Associates.</span> Deep learning for computer vision and chemical detection in hyperspectral data.</div></li>
        <li><div class="lhs">2015 &mdash; 2016</div><div class="rhs"><span class="title">Computer Scientist, Novetta.</span> Deep learning for web-scale multimedia classification and indexing.</div></li>
        <li><div class="lhs">2013 &mdash; 2015</div><div class="rhs"><span class="title">Research Engineer, NASA Goddard Space Flight Center.</span> ML for multimodal geoscience and remote sensing.</div></li>
        <li><div class="lhs">2012 &mdash; 2013</div><div class="rhs"><span class="title">Software Engineer, Oceaneering Advanced Technologies.</span> Deep learning and computer vision for autonomous underwater robotics.</div></li>
        <li><div class="lhs">2009 &mdash; 2011</div><div class="rhs"><span class="title">Research Assistant, University of Maryland.</span> Scientific ML for cosmology and gravitational wave astrophysics.</div></li>
      </ul>
    </section>

    <section id="projects">
      <h2>projects</h2>

      <div class="project-group">agents &amp; research infrastructure</div>

      <div class="entry project">
        <div class="lhs"><span class="lang">rust</span><span class="visibility">private</span></div>
        <div class="rhs"><div class="title">lem</div><div class="desc">A scientific agent for physicists that unifies an Aletheia-style GVR loop and RLM techniques.</div></div>
      </div>
      <div class="entry project">
        <div class="lhs"><span class="lang">python</span><span class="visibility">public</span></div>
        <div class="rhs"><div class="title">deep-gvr</div><div class="desc">A minimal generate-validate-revise loop for Hermes-Agent, inspired by DeepMind&rsquo;s Aletheia agent for autonomous research with empirical, formal, and human verification.</div></div>
      </div>
      <div class="entry project">
        <div class="lhs"><span class="lang">rust</span><span class="visibility">private</span></div>
        <div class="rhs"><div class="title">skaffen-mono</div><div class="desc">A general-purpose personal research, development, engineering, and inquiry agent.</div></div>
      </div>
      <div class="entry project">
        <div class="lhs"><span class="lang">rust</span><span class="visibility">private</span></div>
        <div class="rhs"><div class="title">culture</div><div class="desc">Multi-agent orchestration and AI research assistants.</div></div>
      </div>
      <div class="entry project">
        <div class="lhs"><span class="lang">python</span><span class="visibility">private</span></div>
        <div class="rhs"><div class="title">parallax-science</div><div class="desc">A platform and protocols for AI agents to engage in structured scientific discourse.</div></div>
      </div>
      <div class="entry project">
        <div class="lhs"><span class="lang">tex</span><span class="visibility">private</span></div>
        <div class="rhs"><div class="title">hilbert</div><div class="desc">Solving open problems in quantum information science using AI.</div></div>
      </div>

      <div class="project-group">zx-calculus &amp; fault-tolerant quantum computing</div>

      <div class="entry project">
        <div class="lhs"><span class="lang">rust</span><span class="visibility">private</span></div>
        <div class="rhs"><div class="title">loom-zx</div><div class="desc">A ZX-Calculus focused proof-carrying FTQC platform.</div></div>
      </div>
      <div class="entry project">
        <div class="lhs"><span class="lang">python</span><span class="visibility">private</span></div>
        <div class="rhs"><div class="title">zx-webapp</div><div class="desc">An interactive webapp for exploring fault tolerance for quantum computing in the ZX-Calculus framework.</div></div>
      </div>

      <div class="project-group">quantum error correction &amp; decoders</div>

      <div class="entry project">
        <div class="lhs"><span class="lang">python</span><span class="visibility">public</span></div>
        <div class="rhs"><div class="title">nano-qec</div><div class="desc">Autoresearch-inspired neural decoder training using Hermes Agent and Stim.</div></div>
      </div>
      <div class="entry project">
        <div class="lhs"><span class="lang">python</span><span class="visibility">private</span></div>
        <div class="rhs"><div class="title">sabaki</div><div class="desc">RL training for QEC pre-decoders using Pufferlib, Stim, PyMatching, and sinter.</div></div>
      </div>
      <div class="entry project">
        <div class="lhs"><span class="lang">python</span><span class="visibility">private</span></div>
        <div class="rhs"><div class="title">open-qvla</div><div class="desc">Open vision-language-action models for quantum control, calibration, and error correction.</div></div>
      </div>

      <div class="project-group">systems &amp; platforms</div>

      <div class="entry project">
        <div class="lhs"><span class="lang">rust</span><span class="visibility">private</span></div>
        <div class="rhs"><div class="title">bandalong</div><div class="desc">Authority plane reference implementation in Rust.</div></div>
      </div>
      <div class="entry project">
        <div class="lhs"><span class="lang">go</span><span class="visibility">private</span></div>
        <div class="rhs"><div class="title">chapterhouse</div><div class="desc">A reference intelligence plane implementation.</div></div>
      </div>
      <div class="entry project">
        <div class="lhs"><span class="lang">python</span><span class="visibility">private</span></div>
        <div class="rhs"><div class="title">pelagos</div><div class="desc">A modern, scientist-focused platform for oceanography.</div></div>
      </div>

    </section>

    <section id="publications" class="pubs">
      <h2>publications</h2>
      <ol>
        <li>H. Levine, A. Haim, et al. <em>Demonstrating a long-coherence dual-rail erasure qubit using tunable transmons.</em> Physical Review X, 2024.</li>
        <li>Y. Baum, M. Amico, S. Howell, et al. <em>Experimental Deep Reinforcement Learning for Error-Robust Gateset Design on a Superconducting Quantum Computer.</em> PRX Quantum, 2021.</li>
        <li>Y. Baum, S. Howell, et al. <em>Reinforcement Learning for Error Robust Control on Cloud-Based Superconducting Hardware.</em> Bulletin of the American Physical Society, 2021.</li>
      </ol>
    </section>

    <section id="contact">
      <h2>contact</h2>
      <ul class="contact-list">
        <li>
          <a href="mailto:sean.g.howell@gmail.com">
            <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.75" stroke-linecap="square" stroke-linejoin="miter" aria-hidden="true"><rect x="3" y="5" width="18" height="14"/><path d="M3 6l9 7 9-7"/></svg>
            sean.g.howell@gmail.com
          </a>
        </li>
        <li>
          <a href="https://github.com/sghowell" rel="me">
            <svg class="icon" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><path d="M12 .5C5.73.5.5 5.73.5 12c0 5.08 3.29 9.38 7.86 10.9.58.11.79-.25.79-.56v-2c-3.2.7-3.87-1.37-3.87-1.37-.52-1.33-1.28-1.69-1.28-1.69-1.04-.71.08-.7.08-.7 1.16.08 1.77 1.19 1.77 1.19 1.03 1.76 2.7 1.25 3.36.96.1-.75.4-1.25.73-1.54-2.55-.29-5.24-1.28-5.24-5.69 0-1.26.45-2.28 1.19-3.09-.12-.3-.52-1.47.11-3.07 0 0 .97-.31 3.18 1.18a11.03 11.03 0 0 1 5.79 0c2.21-1.49 3.18-1.18 3.18-1.18.63 1.6.23 2.77.11 3.07.74.81 1.19 1.83 1.19 3.09 0 4.42-2.69 5.39-5.25 5.68.41.36.78 1.06.78 2.13v3.16c0 .31.21.67.8.56C20.21 21.38 23.5 17.08 23.5 12 23.5 5.73 18.27.5 12 .5z"/></svg>
            github.com/sghowell
          </a>
        </li>
        <li>
          <a href="https://scholar.google.com/citations?hl=en&amp;user=lh-wJ1EAAAAJ" rel="me">
            <svg class="icon" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><path d="M12 3L1 9l11 6 9-4.91V17h2V9L12 3zM5 13.18v4L12 21l7-3.82v-4L12 17l-7-3.82z"/></svg>
            google scholar
          </a>
        </li>
        <li>
          <a href="https://x.com/ControlFreq" rel="me">
            <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.25" stroke-linecap="square" stroke-linejoin="miter" aria-hidden="true"><line x1="4" y1="4" x2="20" y2="20"/><line x1="20" y1="4" x2="4" y2="20"/></svg>
            x.com/ControlFreq
          </a>
        </li>
      </ul>
    </section>

    <footer class="site-footer">
      &copy; Sean Howell. Built with HTML &amp; CSS. Set in Berkeley Mono.
    </footer>

  </main>
</body>
</html>
```

- [ ] **Step 2: Verify the file is valid HTML (basic structural check)**

```bash
wc -l index.html
grep -c '<section' index.html
grep -c '</section>' index.html
```
Expected: ~170-200 lines; `<section` count equals `</section>` count equals 5.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add index.html with full site content"
```

---

### Task 6: Local preview and visual verification

**Files:**
- No new files. Visual inspection only.

- [ ] **Step 1: Start a local HTTP server in the background**

Run in the repo root with the Bash tool's `run_in_background: true`:

```bash
python3 -m http.server 8765
```

This serves the repo at `http://localhost:8765/` with the correct absolute paths that GitHub Pages will use. Backgrounding keeps the server alive across subsequent verification steps without blocking the session.

- [ ] **Step 2: Open in a browser and verify visually**

Open `http://localhost:8765/` in Safari or Chrome. Confirm:

1. **Berkeley Mono renders.** The page is in monospace with distinct Berkeley Mono character shapes (not the system fallback). If you see Menlo or SF Mono, the font didn&rsquo;t load &mdash; check DevTools Network tab for the `.woff2` request.
2. **All five content sections appear** in order: About, Experience, Projects, Publications, Contact.
3. **Masthead anchor links work.** Click each anchor link; the page jumps to the correct section.
4. **Resume link downloads the PDF.** Click `[ download resume (pdf) ]`.
5. **All 14 projects appear** across four groups (Agents &amp; research infrastructure: 6, ZX &amp; FTQC: 2, QEC: 3, Systems: 3).
6. **Contact icons render as monochrome SVGs** (envelope, GitHub octocat, scholar cap, X).

- [ ] **Step 3: Test JavaScript-disabled mode**

In Safari: Develop &rarr; Disable JavaScript. Reload the page. Confirm everything still renders and all links still work.

- [ ] **Step 4: Test dark mode**

On macOS: System Settings &rarr; Appearance &rarr; Dark. Reload. Confirm:
1. Background turns near-black (`#0e0e0e`).
2. Text turns near-white (`#ededed`).
3. Section rules and dividers invert correctly.
4. Berkeley Mono still loads.

Return to light mode.

- [ ] **Step 5: Test mobile viewport**

Open Safari Responsive Design Mode (or Chrome DevTools device emulation). Test at 375px width. Confirm:
1. Entries collapse to single-column stacks.
2. Padding tightens.
3. No horizontal scroll.
4. Text remains legible.

- [ ] **Step 6: Kill the local server**

```bash
# Ctrl-C the server, or:
pkill -f "http.server 8765" || true
```

- [ ] **Step 7: No commit**

This task is verification only. If issues were found, fix them in a follow-up commit before proceeding.

---

## Chunk 4: README and deploy

### Task 7: Rewrite `README.md`

**Files:**
- Modify: `README.md` (replace contents)

- [ ] **Step 1: Replace the README**

Overwrite `README.md` with this content:

```markdown
# sghowell.github.io

Personal website for Sean Howell. Deployed at <https://sghowell.github.io>.

## Local preview

```
python3 -m http.server 8765
```

Then open <http://localhost:8765>.

## Structure

- `index.html` &mdash; page content
- `style.css` &mdash; styling
- `fonts/` &mdash; self-hosted Berkeley Mono (see `fonts/LICENSE.md`)
- `assets/` &mdash; downloadable resume
- `.nojekyll` &mdash; disables Jekyll on GitHub Pages
- `docs/superpowers/specs/` &mdash; design spec
- `docs/superpowers/plans/` &mdash; implementation plan

## Font licensing

Berkeley Mono is used under a personal license to Sean Howell. The `.woff2`
file is for Web Font Use only on this site. **Do not redistribute.** See
`fonts/LICENSE.md`.

## Deploying

This is a GitHub Pages user site. Any push to `main` publishes automatically.
```

- [ ] **Step 2: Verify**

```bash
cat README.md | head -20
```

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "docs: rewrite README with dev notes and structure"
```

---

### Task 8: Confirm and configure GitHub remote

**Files:**
- No files. Git remote + GitHub API only.

- [ ] **Step 1: Check whether the remote repo already exists**

```bash
gh repo view sghowell/sghowell.github.io --json name,visibility,url 2>&1
```

**Two possible outcomes:**

- **Repo exists:** Output includes `name`, `visibility: PUBLIC`, and the URL. Continue to Step 2a.
- **Repo does not exist:** Output contains `Could not resolve to a Repository`. Continue to Step 2b.

- [ ] **Step 2a (only if repo exists): Add the SSH remote**

```bash
git remote -v
# If 'origin' is not set:
git remote add origin git@github.com:sghowell/sghowell.github.io.git
git remote -v
```

Skip to Step 3.

- [ ] **Step 2b (only if repo does not exist): Create the repo and set it as origin**

```bash
gh repo create sghowell/sghowell.github.io \
  --public \
  --description "Sean Howell's personal website" \
  --source=. \
  --remote=origin
git remote -v
```

- [ ] **Step 3: Verify remote is set**

```bash
git remote get-url origin
```
Expected: `git@github.com:sghowell/sghowell.github.io.git` (SSH) or `https://github.com/sghowell/sghowell.github.io.git` (HTTPS — either is fine).

- [ ] **Step 4: No commit**

This task changes only git remote configuration, not tracked files.

---

### Task 9: Push to `main` and verify deployment

**Files:**
- No files. Network deploy only.

- [ ] **Step 1: Push**

```bash
git push -u origin main
```

- [ ] **Step 2: Check Pages status**

```bash
gh api repos/sghowell/sghowell.github.io/pages 2>&1
```

**If this returns a 404**, Pages is not yet enabled. Enable it:

```bash
gh api --method POST repos/sghowell/sghowell.github.io/pages \
  -f 'source[branch]=main' \
  -f 'source[path]=/'
```

Then re-run the status check. Expected output includes `"status": "building"` or `"status": "built"` and `"html_url": "https://sghowell.github.io/"`.

- [ ] **Step 3: Poll until the build is live, then verify**

First Pages builds typically take 30&ndash;90 seconds. Poll the root URL until it returns 200:

```bash
for i in $(seq 1 24); do
  code=$(curl -s -o /dev/null -w '%{http_code}' https://sghowell.github.io/)
  if [ "$code" = "200" ]; then echo "live"; break; fi
  echo "attempt $i: $code — retrying in 5s"
  sleep 5
done
```

Expected final line: `live`. Once live:

```bash
curl -sSI https://sghowell.github.io/ | head -5
```
Expected: `HTTP/2 200` and `content-type: text/html`.

```bash
curl -sSI https://sghowell.github.io/fonts/BerkeleyMonoVariable.woff2 | head -5
```
Expected: `HTTP/2 200` and `content-type: font/woff2`.

```bash
curl -sSI https://sghowell.github.io/assets/seanhowell-resume-042026.pdf | head -5
```
Expected: `HTTP/2 200` and `content-type: application/pdf`.

- [ ] **Step 4: Open the live URL in a browser**

Open <https://sghowell.github.io/> and repeat the Task 6 visual checks against the deployed site. Specifically confirm that Berkeley Mono loads from the live origin.

- [ ] **Step 5: No commit**

Deployment is complete. No further source changes in this task.

---

## Acceptance

The v1 implementation is complete when all of the following are true:

1. All 9 tasks in this plan are checked off.
2. `https://sghowell.github.io/` renders the full site in Berkeley Mono.
3. The page passes visual inspection in a modern browser with JavaScript disabled.
4. Dark mode renders correctly.
5. Mobile (≤640px) layout collapses to single-column.
6. All contact links and the resume link work.
7. `curl` confirms 200 OK for `/`, `/fonts/BerkeleyMonoVariable.woff2`, and `/assets/seanhowell-resume-042026.pdf`.
8. `git status` is clean on `main`.

## Out of scope (reiterated)

Favicon, analytics, custom domain, blog, light/dark toggle, contact form, Open Graph tags, and print stylesheet are all explicitly out of scope for v1. Do not add them.
