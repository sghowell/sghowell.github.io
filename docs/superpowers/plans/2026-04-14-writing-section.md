# Writing Section Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a "writing" destination to sghowell.github.io with two essays, each on its own URL, keeping the site's Berkeley Mono chrome and adding IBM Plex Sans for long-form body prose.

**Architecture:** Static HTML/CSS, no JS, no build step. Essays live under `/writing/` as standalone HTML files sharing `style.css`. Essay-specific rules are scoped under a `.essay` body class. The main `index.html` gets a new section listing the essays.

**Tech Stack:** Hand-written HTML + CSS. Self-hosted woff2 fonts. GitHub Pages. No package manager, no bundler, no tests — verification is manual browser inspection.

**Source essays (content is copied from these files):**
- `/Users/seanhowell/Downloads/ai_control_plane_impossible_machines_essay.html`
- `/Users/seanhowell/Downloads/on_the_microfauna_of_megaprojects.html`

**Spec reference:** `docs/superpowers/specs/2026-04-14-writing-section-design.md`

---

## File Structure

**New files:**
- `fonts/IBMPlexSans-Regular.woff2`
- `fonts/IBMPlexSans-Italic.woff2`
- `fonts/IBMPlexSans-SemiBold.woff2`
- `fonts/IBMPlexSans-LICENSE.md`
- `writing/ai-control-plane.html`
- `writing/microfauna-of-megaprojects.html`

**Modified files:**
- `style.css` — add `@font-face` for Plex, add `--font-prose` variable, generalize `.project .title a` to `.entry .title a`, add `.essay` scoped rules, add `.writing` entry styling if needed.
- `index.html` — add `writing` to anchor rail, add `<section id="writing">`, update footer text.

---

## Task 1: Download IBM Plex Sans fonts and license

**Files:**
- Create: `fonts/IBMPlexSans-Regular.woff2`
- Create: `fonts/IBMPlexSans-Italic.woff2`
- Create: `fonts/IBMPlexSans-SemiBold.woff2`
- Create: `fonts/IBMPlexSans-LICENSE.md`

- [ ] **Step 1: Download the three woff2 files from jsDelivr's IBM/plex mirror**

```bash
cd /Users/seanhowell/dev/sghowell.github.io
curl -sSL -o fonts/IBMPlexSans-Regular.woff2 \
  https://cdn.jsdelivr.net/gh/IBM/plex@master/IBM-Plex-Sans/fonts/complete/woff2/IBMPlexSans-Regular.woff2
curl -sSL -o fonts/IBMPlexSans-Italic.woff2 \
  https://cdn.jsdelivr.net/gh/IBM/plex@master/IBM-Plex-Sans/fonts/complete/woff2/IBMPlexSans-Italic.woff2
curl -sSL -o fonts/IBMPlexSans-SemiBold.woff2 \
  https://cdn.jsdelivr.net/gh/IBM/plex@master/IBM-Plex-Sans/fonts/complete/woff2/IBMPlexSans-SemiBold.woff2
```

Expected: three files downloaded. If any URL 404s, the fallback is the `@ibm/plex-sans` npm package or the google-webfonts-helper at `https://gwfh.mranftl.com/fonts/ibm-plex-sans?subsets=latin`.

- [ ] **Step 2: Verify files are valid woff2 and non-trivial in size**

```bash
ls -la fonts/IBMPlexSans-*.woff2
file fonts/IBMPlexSans-*.woff2
```

Expected: each file is ≥ 40KB and `file` reports `Web Open Font Format (Version 2)`. If any file is smaller than 10KB, the download failed (probably fetched an HTML 404 page) — abort the task and report.

- [ ] **Step 3: Download the SIL OFL 1.1 license to `fonts/IBMPlexSans-LICENSE.md`**

```bash
curl -sSL -o fonts/IBMPlexSans-LICENSE.md \
  https://cdn.jsdelivr.net/gh/IBM/plex@master/LICENSE.txt
```

Expected: a plain-text file containing the OFL 1.1 text. Verify with:

```bash
head -5 fonts/IBMPlexSans-LICENSE.md
```

Expected: output mentions "SIL OPEN FONT LICENSE" or similar.

- [ ] **Step 4: Commit**

```bash
git add fonts/IBMPlexSans-Regular.woff2 fonts/IBMPlexSans-Italic.woff2 fonts/IBMPlexSans-SemiBold.woff2 fonts/IBMPlexSans-LICENSE.md
git commit -m "fonts: add self-hosted IBM Plex Sans (regular, italic, semibold)"
```

---

## Task 2: Add Plex @font-face declarations and prose variable to style.css

**Files:**
- Modify: `style.css` (insert after the existing Berkeley Mono `@font-face` block and `:root` definition)

- [ ] **Step 1: Add three @font-face blocks for IBM Plex Sans**

In `style.css`, directly after the existing Berkeley Mono `@font-face` block (currently ends at line 12), insert:

```css
@font-face {
  font-family: "IBM Plex Sans";
  src: url("/fonts/IBMPlexSans-Regular.woff2") format("woff2");
  font-weight: 400;
  font-style: normal;
  font-display: swap;
}

@font-face {
  font-family: "IBM Plex Sans";
  src: url("/fonts/IBMPlexSans-Italic.woff2") format("woff2");
  font-weight: 400;
  font-style: italic;
  font-display: swap;
}

@font-face {
  font-family: "IBM Plex Sans";
  src: url("/fonts/IBMPlexSans-SemiBold.woff2") format("woff2");
  font-weight: 600;
  font-style: normal;
  font-display: swap;
}
```

- [ ] **Step 2: Add `--font-prose` variable inside `:root`**

In the existing `:root` block, directly after the `--font` line, add:

```css
  --font-prose: "IBM Plex Sans", ui-sans-serif, -apple-system, BlinkMacSystemFont, "Segoe UI", Inter, Roboto, Helvetica, Arial, sans-serif;
```

The `:root` block should now contain `--font` followed by `--font-prose`, then the existing colors.

- [ ] **Step 3: Verify the index page still renders unchanged**

Serve locally and load `http://localhost:8000/`:

```bash
cd /Users/seanhowell/dev/sghowell.github.io
python3 -m http.server 8000
```

Expected: the site renders identically to before. The three Plex files should be available at `/fonts/IBMPlexSans-*.woff2` but the browser does not fetch them because nothing references the `"IBM Plex Sans"` family yet. Confirm in DevTools → Network that no `IBMPlexSans-*` requests occur on the index.

Stop the server (`Ctrl+C`) before moving on.

- [ ] **Step 4: Commit**

```bash
git add style.css
git commit -m "style: register IBM Plex Sans font-faces and prose variable"
```

---

## Task 3: Generalize project title link styling and add entry-title-link support

**Files:**
- Modify: `style.css` (existing `.project .title a` rule around lines 240-250)

- [ ] **Step 1: Change the selector from `.project .title a` to `.entry .title a`**

Find this existing block in `style.css`:

```css
.project .title a {
  color: inherit;
  text-decoration: none;
  border-bottom: 1px solid var(--faint);
  padding-bottom: 1px;
}

.project .title a:hover,
.project .title a:focus-visible {
  border-bottom-color: var(--fg);
}
```

Replace with:

```css
.entry .title a {
  color: inherit;
  text-decoration: none;
  border-bottom: 1px solid var(--faint);
  padding-bottom: 1px;
}

.entry .title a:hover,
.entry .title a:focus-visible {
  border-bottom-color: var(--fg);
}
```

This generalization lets the upcoming writing entries pick up the same title-link treatment without duplicating the rule. No existing non-project entry currently has a title link, so this is a pure widen-the-selector change with no regressions.

- [ ] **Step 2: Verify no regression on the index**

```bash
cd /Users/seanhowell/dev/sghowell.github.io
python3 -m http.server 8000
```

Load `http://localhost:8000/` and visually confirm the project titles that are links (`deep-gvr`, `nano-qec`) still render with the faint underline and turn dark on hover. Stop the server.

- [ ] **Step 3: Commit**

```bash
git add style.css
git commit -m "style: generalize title-link rule from .project to .entry"
```

---

## Task 4: Add .essay scoped styles to style.css

**Files:**
- Modify: `style.css` (append at end of file, before any trailing empty lines)

- [ ] **Step 1: Append the full essay scope block**

Add this block at the end of `style.css`:

```css
/* ==========================================================================
   Essay pages (/writing/*.html)
   Scoped under body.essay so nothing leaks into the one-pager.
   ========================================================================== */

.essay .top-strip {
  display: flex;
  align-items: baseline;
  justify-content: space-between;
  gap: 16px;
  margin-bottom: 32px;
  padding-bottom: 14px;
  border-bottom: 1px solid var(--rule);
}

.essay .top-strip .brand {
  font-size: 0.8125rem;
  font-weight: 700;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  color: var(--fg);
  text-decoration: none;
}

.essay .top-strip .brand .sep {
  color: var(--faint);
  margin: 0 6px;
  font-weight: 400;
}

.essay .top-strip .back {
  font-size: 0.75rem;
  color: var(--muted);
  text-decoration: none;
  border-bottom: 1px solid transparent;
  padding-bottom: 1px;
  white-space: nowrap;
}

.essay .top-strip .back:hover,
.essay .top-strip .back:focus-visible {
  color: var(--fg);
  border-bottom-color: var(--fg);
}

.essay .essay-header {
  margin-bottom: 40px;
}

.essay .essay-header .eyebrow {
  font-size: 0.75rem;
  font-weight: 700;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--muted);
  margin: 0 0 14px;
}

.essay .essay-header .eyebrow::before {
  content: "// ";
  color: var(--faint);
}

.essay .essay-header h1 {
  margin: 0 0 14px;
  font-family: var(--font);
  font-size: 1.5rem;
  font-weight: 700;
  letter-spacing: 0.02em;
  line-height: 1.25;
  max-width: 32ch;
  text-transform: none;
}

.essay .essay-header .dek {
  margin: 0 0 14px;
  max-width: 64ch;
  font-family: var(--font-prose);
  font-size: 0.9375rem;
  line-height: 1.6;
  color: var(--muted);
}

.essay .essay-header .date {
  margin: 0;
  font-size: 0.75rem;
  letter-spacing: 0.04em;
  text-transform: uppercase;
  color: var(--faint);
}

.essay .article {
  max-width: 64ch;
  font-family: var(--font-prose);
  font-size: 1.0625rem;
  line-height: 1.72;
  color: var(--fg);
}

.essay .article p {
  margin: 0 0 1.15em;
}

.essay .article h2 {
  margin: 2.2em 0 0.9em;
  font-family: var(--font);
  font-size: 0.8125rem;
  font-weight: 700;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--muted);
}

.essay .article h2::before {
  content: "// ";
  color: var(--faint);
}

.essay .article em {
  font-style: italic;
}

.essay .article a {
  color: var(--fg);
  text-decoration: none;
  border-bottom: 1px solid var(--subtle);
  padding-bottom: 1px;
}

.essay .article a:hover,
.essay .article a:focus-visible {
  border-bottom-color: var(--fg);
}

.essay .article .pullquote {
  margin: 1.8em 0;
  padding: 0.3em 0 0.3em 1.25em;
  border-left: 2px solid var(--fg);
}

.essay .article .pullquote p {
  margin: 0;
  font-style: italic;
  font-size: 1.125em;
  line-height: 1.55;
  color: var(--fg);
}

.essay .article sup.note {
  font-family: var(--font);
  font-size: 0.68em;
  vertical-align: super;
  line-height: 0;
  margin-left: 0.1em;
  white-space: nowrap;
}

.essay .article sup.note a {
  color: var(--muted);
  text-decoration: none;
  border-bottom: none;
  font-weight: 700;
}

.essay .article sup.note a:hover,
.essay .article sup.note a:focus-visible {
  color: var(--fg);
}

.essay .sources {
  margin-top: 48px;
  padding-top: 14px;
  border-top: 1px solid var(--rule);
  max-width: 64ch;
}

.essay .sources h2 {
  font-size: 0.8125rem;
  font-weight: 700;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--muted);
  margin: 0 0 16px;
}

.essay .sources h2::before {
  content: "// ";
  color: var(--faint);
}

.essay .sources ol {
  list-style: decimal;
  padding-left: 20px;
  margin: 0;
}

.essay .sources li {
  padding: 10px 0;
  font-family: var(--font-prose);
  font-size: 0.875rem;
  color: var(--muted);
  line-height: 1.55;
  border-bottom: 1px solid var(--subtle);
}

.essay .sources li:last-child {
  border-bottom: 0;
}

.essay .sources li a {
  color: var(--fg);
  text-decoration: none;
  border-bottom: 1px solid var(--subtle);
  padding-bottom: 1px;
}

.essay .sources li a:hover,
.essay .sources li a:focus-visible {
  border-bottom-color: var(--fg);
}

.essay .bottom-strip {
  margin-top: 40px;
  padding-top: 14px;
  border-top: 1px solid var(--subtle);
}

.essay .bottom-strip .back {
  font-size: 0.75rem;
  color: var(--muted);
  text-decoration: none;
  border-bottom: 1px solid transparent;
  padding-bottom: 1px;
}

.essay .bottom-strip .back:hover,
.essay .bottom-strip .back:focus-visible {
  color: var(--fg);
  border-bottom-color: var(--fg);
}

@media (max-width: 640px) {
  .essay .top-strip { flex-wrap: wrap; gap: 8px; }
  .essay .essay-header h1 { font-size: 1.25rem; }
  .essay .article { font-size: 1rem; line-height: 1.68; }
}
```

- [ ] **Step 2: Verify the index page still renders unchanged**

The new rules are all scoped under `.essay`, so loading `index.html` (which has no `.essay` class on `body`) should look identical.

```bash
cd /Users/seanhowell/dev/sghowell.github.io
python3 -m http.server 8000
```

Load `http://localhost:8000/`, confirm no visual change. Stop the server.

- [ ] **Step 3: Commit**

```bash
git add style.css
git commit -m "style: add scoped essay-page layout rules"
```

---

## Task 5: Create writing/ai-control-plane.html

**Files:**
- Create: `writing/ai-control-plane.html`

**Source for content:** `/Users/seanhowell/Downloads/ai_control_plane_impossible_machines_essay.html`, lines 297–481 (article body at 297–438 plus sources list at 440–481).

- [ ] **Step 1: Create the directory**

```bash
mkdir -p /Users/seanhowell/dev/sghowell.github.io/writing
```

- [ ] **Step 2: Write the essay shell**

Create `writing/ai-control-plane.html` with exactly this content. The article body paragraphs are lifted verbatim from the source file — paragraph order, punctuation, `<em>` tags, and `<sup class="note">` references are preserved. The original's `class="lead"` on the first paragraph is dropped. Decorative scaffolding (`.sheet`, `.hero`, `.article-wrap`, `.meta`, `.eyebrow::before` rules, `.rule` dividers, `.footer-note`) is removed.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>AI as the Control Plane for Impossible Machines — Sean Howell</title>
  <meta name="description" content="Why utility-scale quantum computers, autonomous laboratories, advanced fabs, and large robotic systems will need advanced AI as an operating layer—not as a convenience feature, but as part of the machine.">
  <link rel="icon" href="data:,">
  <link rel="stylesheet" href="/style.css">
</head>
<body class="essay">
  <main class="container">

    <div class="top-strip">
      <a class="brand" href="/">SEAN HOWELL<span class="sep">/</span>WRITING</a>
      <a class="back" href="/#writing">← back</a>
    </div>

    <header class="essay-header">
      <p class="eyebrow">essay</p>
      <h1>AI as the Control Plane for Impossible Machines</h1>
      <p class="dek">Why utility-scale quantum computers, autonomous laboratories, advanced fabs, and large robotic systems will need advanced AI as an operating layer—not as a convenience feature, but as part of the machine.</p>
      <p class="date">February 2026</p>
    </header>

    <article class="article">

      <p>I have spent enough time around quantum hardware, control stacks, and AI-driven scientific workflows to think the answer is settled: the most important machines we build in the next decade will not be operable at scale without advanced AI. A utility-scale quantum computer is not just a better chip. It is a heterogeneous system of chips, control electronics, cryogenics, networking, compilers, decoders, schedulers, data systems, and operators tied together by feedback loops. An autonomous laboratory has the same shape. So does a modern chip fab. So does any serious multi-robot installation. Once you cross a certain threshold of scale and coupling, human operators stop being the runtime. They become the supervisory layer.</p>

      <h2>Quantity has a quality of its own</h2>

      <p>What changes at scale is not just size; it is category. IBM's roadmap now talks openly about modular systems, multiple quantum processors linked in a data-center environment, real-time decoding, and a path to 200 logical qubits running 100 million gates by 2029.<sup class="note"><a href="#ref1">1</a></sup> Google's recent Willow results make the same point from another direction: useful applications are expected to need billions if not trillions of reliable operations, and the latest error-correction results already depend on classical software built around machine learning, reinforcement learning, graph-based decoding, and real-time measurement processing.<sup class="note"><a href="#ref2">2</a></sup> Photonic roadmaps push the infrastructure logic further still: foundry-scale wafer production, cryogenic cabinets supporting hundreds of chips, and dedicated characterization and calibration facilities treated as part of the machine rather than as support functions.<sup class="note"><a href="#ref3">3</a></sup></p>

      <p>That is the real shift. The bottleneck is no longer whether an isolated component can hit a target fidelity or throughput number. The bottleneck is whether the whole system can keep itself inside a viable operating envelope while objectives, workloads, and physical conditions change. At utility scale you are managing drift, correlated failures, latency budgets, maintenance windows, admission control, and scheduling across layers that were once treated as separate disciplines. This is a cross-stack control problem.</p>

      <div class="pullquote">
        <p>At utility scale, the hard part is not building a machine that works once. It is building one that can keep working while everything around it moves.</p>
      </div>

      <p>In my own work, I keep coming back to the same pattern: architecture search, photonic design, calibration, error correction, compilers, experimental workflows, and system operations are converging into one continuous optimization problem. We draw boundaries for organizational convenience, but the machine does not respect them. A change in device design changes calibration burden. A change in decoder latency changes useful algorithm depth. A change in scheduling policy changes the shape of the error budget. Once you see the system whole, it becomes hard to imagine scaling it with disconnected tools.</p>

      <h2>Why rules are not enough</h2>

      <p>We already know how to automate narrow loops. FPGAs, control firmware, recipe engines, advanced process control, and carefully tuned heuristics remain essential. I am not arguing that advanced AI replaces those tools. Low-level control should remain as deterministic, testable, and boring as we can make it. The problem is that fixed automation does not scale cleanly into environments that are high-dimensional, partially observed, nonstationary, and safety-constrained. Hand-built rules work until they collide with a regime change, a new operating point, or an interaction that no one explicitly encoded.</p>

      <p>Advanced AI matters because it can sit above and across those loops. It can fuse multimodal telemetry, learn surrogate models where first-principles simulation is too slow, infer latent state from messy data, rank interventions by expected value, and plan under uncertainty. In a quantum system, that means deciding what to recalibrate and when, rerouting workloads around drifting subsystems, trading decoder latency against logical fidelity, and translating application intent into live resource allocation. In a fab, it means detecting excursions earlier and navigating thousands of interdependent process variables. In a large robotic system, it means coordinating perception, planning, task allocation, and failure recovery without drowning operators in exception handling.</p>

      <h2>The pattern</h2>

      <p>None of this is speculative. Berkeley Lab's A-Lab was built as a closed-loop materials discovery system and reports throughput on the order of 50 to 100 times a human operator, with 24/7 operation and 100–200 samples per day.<sup class="note"><a href="#ref4">4</a></sup> The corresponding <em>Nature</em> paper reported 41 novel compounds realized from 58 targets in 17 days of continuous operation.<sup class="note"><a href="#ref5">5</a></sup> In semiconductor manufacturing, Applied Materials publicly describes platforms that take millions of measurements across wafers and individual chips while optimizing thousands of process variables in real time, spanning research, ramp, and high-volume manufacturing.<sup class="note"><a href="#ref6">6</a></sup></p>

      <p>These are early forms of the same architecture: a machine whose performance depends as much on orchestration, inference, and adaptive decision-making as on the raw capability of any single instrument. Once that architecture appears, the separation between "the machine" and "the software that helps run it" starts to break down. The software is part of the system's achievable operating regime.</p>

      <h2>What "advanced AI" actually means here</h2>

      <p>When I say advanced AI, I do not mean a chatbot bolted onto a dashboard. I mean models that understand time series, logs, images, layouts, text, and simulator output together; agents that can propose and evaluate actions; and policy layers that enforce hard constraints before anything touches hardware. The architecture I have in mind is conservative where it must be and adaptive where it can be. Hard real-time control stays classical. Safety interlocks stay explicit. AI handles orchestration, diagnosis, forecasting, design-space search, and cross-layer optimization.</p>

      <p>The same AI stack that will operate these systems is already useful in designing them. Architecture discovery, photonic inverse design, decoder search, compiler co-optimization, experiment planning, and resource estimation are all versions of the same problem: exploring very large design spaces under hard physical constraints. The boundary between design-time AI and runtime AI is going to blur.</p>

      <p>Of course, this only works if the AI layer is engineered like critical infrastructure rather than consumer software. That means data provenance, audit trails, simulator-in-the-loop evaluation, rollback, calibrated uncertainty, action whitelists, structured escalation, and continuous benchmarking against baselines. Humans still set goals, define invariants, approve policy, and own accountability. But asking humans to be the real-time operating system for million-component scientific infrastructure is not realism. It is nostalgia.</p>

      <h2>Practical consequences</h2>

      <p>If we want utility-scale quantum computers, self-driving laboratories, next-generation fabs, or large robotic installations to work in the next decade, we should stop treating AI as an optional accelerator for decoders, compilers, lab notebooks, or operations dashboards. We should design it as part of the control plane from the start. That means instrumenting every subsystem for learning, building simulators and digital twins alongside hardware, exposing clean operational abstractions, and creating policy engines that can mediate between human intent and machine action.</p>

      <p>I think this is one of the deepest engineering shifts now underway. For a long time, intelligence sat outside the machine: the scientist at the bench, the operator in the control room, the engineer on call. The systems now arriving are too dynamic, too coupled, and too large for that model. They need an adaptive layer between human intent and physical execution. In practice, that layer will be advanced AI. For utility-scale quantum computers especially, it will matter as much as the qubits themselves.</p>

    </article>

    <section class="sources" aria-labelledby="sources-heading">
      <h2 id="sources-heading">sources</h2>
      <ol>
        <li id="ref1">
          IBM Quantum,
          <a href="https://www.ibm.com/quantum/hardware">"Hardware for useful quantum computing"</a>,
          accessed March 29, 2026; and IBM Quantum Blog,
          <a href="https://www.ibm.com/quantum/blog/large-scale-ftqc">"How IBM will build the world's first large-scale, fault-tolerant quantum computer"</a>,
          June 10, 2025.
        </li>
        <li id="ref2">
          Google Research,
          <a href="https://research.google/blog/making-quantum-error-correction-work/">"Making quantum error correction work"</a>,
          December 9, 2024.
        </li>
        <li id="ref3">
          PsiQuantum,
          <a href="https://www.psiquantum.com/about">"About"</a>,
          accessed March 29, 2026; and PsiQuantum,
          <a href="https://www.psiquantum.com/news-import/omega">"PsiQuantum Announces Omega, a Manufacturable Chipset for Photonic Quantum Computing"</a>,
          February 26, 2025.
        </li>
        <li id="ref4">
          Berkeley Lab,
          <a href="https://newscenter.lbl.gov/2023/04/17/meet-the-autonomous-lab-of-the-future/">"Meet the Autonomous Lab of the Future"</a>,
          April 17, 2023.
        </li>
        <li id="ref5">
          Nathan J. Szymanski et al.,
          <a href="https://www.nature.com/articles/s41586-023-06734-w">"An autonomous laboratory for the accelerated synthesis of inorganic materials"</a>,
          <em>Nature</em>, 2023.
        </li>
        <li id="ref6">
          Applied Materials,
          <a href="https://investor.appliedmaterials.com/news-releases/news-release-details/applied-materials-aix-platform-harnesses-power-big-data-and-ai/">"Applied Materials AI(x) Platform Harnesses the Power of Big Data and AI to Accelerate Semiconductor Technology Breakthroughs from Lab to Fab"</a>,
          April 5, 2021.
        </li>
      </ol>
    </section>

    <div class="bottom-strip">
      <a class="back" href="/#writing">← back to writing</a>
    </div>

  </main>

  <footer class="site-footer">
    &copy; Sean Howell. Built with HTML &amp; CSS. Set in Berkeley Mono and IBM Plex Sans.
  </footer>

</body>
</html>
```

- [ ] **Step 3: Verify the essay renders, footnotes jump correctly, and Plex loads**

```bash
cd /Users/seanhowell/dev/sghowell.github.io
python3 -m http.server 8000
```

Load `http://localhost:8000/writing/ai-control-plane.html` and confirm:
1. Body text is in IBM Plex Sans (DevTools → Computed → `font-family` on a paragraph should resolve to `"IBM Plex Sans"`).
2. Headings / eyebrow / top strip are in Berkeley Mono.
3. Clicking each of the six footnote superscripts jumps to the matching source entry.
4. The pullquote renders as italic Plex with a left rule.
5. The "← back" top-strip link and "← back to writing" bottom-strip link both point to `/#writing`.
6. DevTools → Network shows `IBMPlexSans-Regular.woff2` and `IBMPlexSans-Italic.woff2` being fetched (SemiBold may or may not load depending on whether any heading uses 600 weight — it's registered for future use).
7. Toggle system dark mode and confirm the page inverts correctly.

Stop the server.

- [ ] **Step 4: Commit**

```bash
git add writing/ai-control-plane.html
git commit -m "writing: add 'AI as the Control Plane for Impossible Machines' essay"
```

---

## Task 6: Create writing/microfauna-of-megaprojects.html

**Files:**
- Create: `writing/microfauna-of-megaprojects.html`

**Source for content:** `/Users/seanhowell/Downloads/on_the_microfauna_of_megaprojects.html`, lines 295–577 (article body at 295–493 plus sources list at 495–577).

- [ ] **Step 1: Write the essay shell**

Create `writing/microfauna-of-megaprojects.html` with exactly this content. Same template as Task 5 with the content swapped.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>On the Microfauna of Megaprojects — Sean Howell</title>
  <meta name="description" content="Why the biggest machines of the next decade will be inhabited by dense ecologies of small machine intelligences—and why the organizations that matter should start cultivating them now.">
  <link rel="icon" href="data:,">
  <link rel="stylesheet" href="/style.css">
</head>
<body class="essay">
  <main class="container">

    <div class="top-strip">
      <a class="brand" href="/">SEAN HOWELL<span class="sep">/</span>WRITING</a>
      <a class="back" href="/#writing">← back</a>
    </div>

    <header class="essay-header">
      <p class="eyebrow">essay</p>
      <h1>On the Microfauna of Megaprojects</h1>
      <p class="dek">Why the biggest machines of the next decade will be inhabited by dense ecologies of small machine intelligences—and why the organizations that matter should start cultivating them now.</p>
      <p class="date">March 2026</p>
    </header>

    <article class="article">

      <p>I increasingly think the control-plane metaphor is too neat for what is coming. It still imagines a clean hierarchy: machine below, intelligence above, human at the top. The next decade's serious systems will not look like that. A utility-scale quantum computer, an autonomous lab, an advanced chip fab, or a large robot fleet will be inhabited by something more like microfauna: dense populations of small, specialized machine intelligences living inside the project and feeding on its telemetry, schedules, faults, work orders, and simulations. Some will watch drift. Some will route jobs. Some will reconcile sensors. Some will triage repairs. Some will negotiate between throughput and risk. Most of them will be boring. All of them will matter.</p>

      <p>My own work on scientific agents is part of why I have grown impatient with the word <em>copilot</em>. A copilot sits beside a human and waits to be asked. These systems need resident operators woven into the substrate. Quantum already tells the story. IBM's current fault-tolerant plan is explicitly modular and adaptive, with real-time decoded measurements that can change subsequent instructions, a compact decoder architecture aimed at FPGA or ASIC implementation, and a 2029 target of 200 logical qubits running 100 million gates.<sup class="note"><a href="#ref1">1</a></sup> Google reports the same basic truth from another direction: on Willow, error correction now works well enough that decoder latency itself becomes a first-order systems problem, with 50–100 microseconds of delay already material to performance.<sup class="note"><a href="#ref2">2</a></sup> PsiQuantum, meanwhile, is no longer talking like a lab prototype vendor; it is talking in the language of tier-1 foundries, thousands of wafers, cryogenic cabinets supporting hundreds of chips, and million-qubit machines.<sup class="note"><a href="#ref3">3</a></sup> These are not isolated devices anymore. They are operational ecologies.</p>

      <h2>The small agents arrive first</h2>

      <p>The same pattern is now showing up in other domains with surprisingly little disguise. Applied Materials describes chipmaking as a continuous chain from atomic-scale materials simulation to fab-scale digital twins, with millions of measurements, thousands of process variables, and virtual studies that now collapse weeks into same-day work.<sup class="note"><a href="#ref4">4</a></sup> The U.S. Commerce Department has backed a semiconductor digital-twin institute with explicit goals to cut development and manufacturing costs by more than 40 percent and cycle times by 35 percent.<sup class="note"><a href="#ref5">5</a></sup> BMW is scaling digital twins across more than 30 production sites, expects up to 30 percent lower planning costs, and says the system is being expanded with generative and agentic AI assistants.<sup class="note"><a href="#ref6">6</a></sup> Amazon has deployed its millionth robot and says its DeepFleet model should improve travel efficiency across the fleet by 10 percent.<sup class="note"><a href="#ref7">7</a></sup> NVIDIA is now publishing blueprints for robot-fleet twins and AI-factory operations that explicitly connect power, cooling, networking, and operations agents.<sup class="note"><a href="#ref8">8</a></sup> Siemens, working with NVIDIA, is talking openly about an industrial AI operating system and fully AI-driven adaptive manufacturing blueprints starting in 2026, while PepsiCo is already using its new twin stack to catch potential facility issues before physical changes are made.<sup class="note"><a href="#ref9">9</a></sup></p>

      <p>In science, the boundary is dissolving just as fast. <em>Nature</em> papers now describe autonomous mobile robots for exploratory chemistry, LLM-driven systems that can design, plan, and execute experiments through tool use, and growing interest in self-driving labs as shared network infrastructure rather than one-off curiosities.<sup class="note"><a href="#ref10">10</a></sup><sup class="note"><a href="#ref11">11</a></sup> The practical point is that autonomy will arrive as graded resident capability, not as a single dramatic handoff. The important architectural fact is that these roles are local, narrow, and continuous. A quantum computer needs agents that watch syndrome statistics, prioritize recalibration, detect creeping thermal or RF pathologies, choose when to reroute workloads, and translate application demand into live resource commitments. A fab needs agents that sit between chamber physics, metrology, recipes, and throughput. A robot fleet needs traffic organisms, map maintainers, collision forecasters, charge schedulers, and exception triage. A self-driving lab needs experiment proposers, instrument translators, literature scavengers, and safety clerks. These systems do not want a single giant model making every decision from first principles. They want many smaller models with limited authority, shared state, and hard boundaries.</p>

      <div class="pullquote">
        <p>Megaprojects do not become autonomous by growing a brain. They become autonomous by growing a biosphere.</p>
      </div>

      <h2>The project becomes a managed ecosystem</h2>

      <p>This is the part I think people still underweight. The important agents will not mostly look like scientists or strategists. They will look like janitors, dispatchers, inspectors, bookkeepers, and mechanics. One will notice that a particular syndrome pattern in a quantum module is the early smell of a calibration slip. Another will delay a process step because a metrology instrument is starting to drift. Another will rewrite robot traffic after a small layout change. Another will refuse an experiment plan because the solvent cabinet, glovebox schedule, and waste stream are already near constraint. The hard problem is not producing brilliant answers in the abstract. It is maintaining local order in a system that never stops moving.</p>

      <p>That is why I do not expect the winning pattern to be one giant model sitting in a control room. Central planners are useful, but the combinatorics arrive faster than any single loop can absorb them. The winning pattern is a society of narrower models with limited authority, shared state, and explicit law. Think of a megaproject less as a machine with a brain than as a habitat with an immune system, a circulatory system, and a bookkeeping layer. If the data model is clean and the rules are tight, this ecology becomes extraordinarily powerful. If the data model is dirty and the rules are fuzzy, it goes feral fast.</p>

      <p>This also changes the shape of the firm. Project management stops being mostly a matter of meetings and starts becoming a live inference system. Schedules become live beliefs about what the plant can really do this week. Change control moves from PDF procedure to machine-readable law. The org chart matters less than the permission graph: who, or what, is allowed to sense, decide, and act.</p>

      <p>My aggressive bet is that by the early 2030s the decisive moat in many of these systems will sit less in the headline hardware than in the resident ecology around it. Two teams may have comparable robot cells, comparable process tools, comparable qubits, even comparable foundation models. One will have years of replayable telemetry, shadow agents, simulator traces, policy history, and incident memory. The other will have dashboards, tickets, heroics, and tribal knowledge. From the outside they will look similar. Inside, one will compound and the other will stall.</p>

      <h2>Predictions, sooner than people think</h2>

      <p>By 2027, every credible fault-tolerant quantum effort will have agentic operations in shadow mode across calibration prioritization, decoder tuning, workload admission, and failure diagnosis. By 2028, at least one major fab, robot-heavy factory, or autonomous-lab network will treat the live digital twin as a commissioning requirement rather than a planning aid. By 2029, large industrial systems will begin to budget inference, simulation, and policy infrastructure the way they already budget power, cooling, and cleanroom utilities. That same year, I would expect the first utility-scale quantum programs to discover that the real bottleneck is not a single hardware metric but the coordination burden of keeping the whole stack inside its useful envelope.</p>

      <p>By 2030, the phrase <em>operator headcount</em> will need an asterisk. The median consequential decision inside a frontier lab, fab, or robot fleet will already have been made, filtered, or scheduled by machine intelligence before a human ever sees it. Humans will still set objectives, own risk, sign off on escalations, and intervene in novel situations. But they will no longer be the runtime. They will be the constitutional layer.</p>

      <p>By 2032, procurement, finance, and regulation will start to absorb this. Black-box equipment that cannot emit structured state, accept policy-mediated commands, or participate in replayable digital twins will begin to look obsolete. Insurers and auditors will want incident lineage, action traces, uncertainty thresholds, and rollback proofs. In other words, the microfauna will stop being an internal convenience and become part of the official operating model.</p>

      <h2>What to do now</h2>

      <p>The immediate work is much less glamorous than the vision. First, instrument the system as if it is going to host machine operators. High-frequency telemetry, calibration histories, maintenance records, operator notes, work orders, simulator outputs, and control actions need to land in a shared event model with stable identifiers and real timestamps. You cannot grow useful microfauna in a swamp of screenshots, spreadsheets, and PDF procedures.</p>

      <p>Second, stop treating digital twins as presentationware. Build them for replay, counterfactuals, and shadow operation. Every meaningful intervention should be testable against a twin or at least a surrogate before it touches live hardware. Every incident should become training material. Every near miss should be reproducible enough that an agent can learn from it without rerunning the failure in the plant.</p>

      <p>Third, write a machine constitution before you write a machine personality. Most of the engineering here is permissions, not prompt craft: what an agent may observe, propose, execute, defer, cancel, or escalate; what uncertainty bars apply; what hard invariants may never be crossed; what rollback path exists if the model is wrong; which human signatures are required when the stakes change. A megaproject without this will still grow microfauna. It will just grow them accidentally.</p>

      <p>Fourth, seed the ecology with narrow residents. Give one agent ownership of calibration prioritization. Give another maintenance triage. Give a third responsibility for routing robot traffic, ranking experiments, or reconciling metrology against throughput. Run them in shadow mode. Compare them against human baselines. Keep their action surfaces small until they earn more. The goal is not to conjure an industrial superintelligence. The goal is to discover which organisms are actually useful and under what constraints.</p>

      <p>Finally, staff this like core infrastructure. The right team is not an AI initiative off to the side. It is a joint operating group of controls people, software people, domain scientists, reliability engineers, and whoever carries operational accountability. If the system is strategic, the ecology around it is strategic.</p>

      <p>I do not think the next decade ends with one majestic industrial mind quietly running everything. I think it ends with something stranger and more practical: megaprojects that are dense with constrained machine operators, each unremarkable on its own, together forming the metabolism of the system. The question is no longer whether such microfauna will appear. They already have. The question is whether we cultivate their habitat deliberately enough that they make the project coherent instead of feral.</p>

    </article>

    <section class="sources" aria-labelledby="sources-heading">
      <h2 id="sources-heading">sources</h2>
      <ol>
        <li id="ref1">
          IBM Quantum Blog,
          <a href="https://www.ibm.com/quantum/blog/large-scale-ftqc">"How IBM will build the world's first large-scale, fault-tolerant quantum computer"</a>,
          June 10, 2025; and IBM,
          <a href="https://www.ibm.com/quantum/hardware">"Quantum computing hardware and roadmap"</a>,
          accessed March 30, 2026.
        </li>
        <li id="ref2">
          Google Research,
          <a href="https://research.google/blog/making-quantum-error-correction-work/">"Making quantum error correction work"</a>,
          December 9, 2024; and Michael Newman et al.,
          <a href="https://www.nature.com/articles/s41586-024-08449-y">"Quantum error correction below the surface code threshold"</a>,
          <em>Nature</em>, 2025.
        </li>
        <li id="ref3">
          PsiQuantum,
          <a href="https://www.psiquantum.com/news-import/omega">"PsiQuantum Announces Omega, a Manufacturable Chipset for Photonic Quantum Computing"</a>,
          February 26, 2025; and PsiQuantum,
          <a href="https://www.psiquantum.com/about">"About"</a>,
          accessed March 30, 2026.
        </li>
        <li id="ref4">
          Applied Materials,
          <a href="https://www.appliedmaterials.com/us/en/semiconductor/solutions-and-software/ai-x.html">"AIx"</a>,
          accessed March 30, 2026; and Applied Materials,
          <a href="https://www.appliedmaterials.com/us/en/newsroom/quick-takes/applied-materials-accelerates-end-to-end-chip-manufacturing-with-nvidia-ai.html">"Applied Materials Collaborates With NVIDIA to Accelerate End-to-End Chip Manufacturing"</a>,
          February 18, 2026.
        </li>
        <li id="ref5">
          U.S. Department of Commerce,
          <a href="https://www.commerce.gov/news/press-releases/2025/01/biden-harris-administration-awards-semiconductor-research-corporation">"Biden-Harris Administration Awards Semiconductor Research Corporation Manufacturing Consortium Corporation $285M for New CHIPS Manufacturing USA Institute for Digital Twins"</a>,
          January 3, 2025.
        </li>
        <li id="ref6">
          BMW Group,
          <a href="https://www.press.bmwgroup.com/global/article/detail/T0450699EN/bmw-group-scales-virtual-factory?language=en">"BMW Group scales Virtual Factory"</a>,
          June 11, 2025; and NVIDIA,
          <a href="https://www.nvidia.com/en-us/case-studies/bmw-group-develop/">"BMW Group Develop Custom Application on NVIDIA Omniverse"</a>,
          accessed March 30, 2026.
        </li>
        <li id="ref7">
          Amazon,
          <a href="https://www.aboutamazon.com/news/operations/amazon-million-robots-ai-foundation-model">"Amazon launches a new AI foundation model to power its robotic fleet and deploys its 1 millionth robot"</a>,
          June 30, 2025.
        </li>
        <li id="ref8">
          NVIDIA,
          <a href="https://blogs.nvidia.com/blog/mega-omniverse-blueprint/">"NVIDIA Unveils 'Mega' Omniverse Blueprint for Building Industrial Robot Fleet Digital Twins"</a>,
          January 6, 2025; NVIDIA,
          <a href="https://nvidianews.nvidia.com/news/nvidia-releases-vera-rubin-dsx-ai-factory-reference-design-and-omniverse-dsx-digital-twin-blueprint-with-broad-industry-support">"NVIDIA Releases Vera Rubin DSX AI Factory Reference Design and Omniverse DSX Digital Twin Blueprint With Broad Industry Support"</a>,
          March 16, 2026; and NVIDIA,
          <a href="https://www.nvidia.com/en-us/omniverse/">"NVIDIA Omniverse"</a>,
          accessed March 30, 2026.
        </li>
        <li id="ref9">
          NVIDIA Newsroom,
          <a href="https://nvidianews.nvidia.com/news/siemens-and-nvidia-expand-partnership-industrial-ai-operating-system">"Siemens and NVIDIA Expand Partnership to Build the Industrial AI Operating System"</a>,
          January 6, 2026; and Siemens,
          <a href="https://press.siemens.com/global/en/pressrelease/siemens-unveils-technologies-accelerate-industrial-ai-revolution-ces-2026">"Siemens unveils technologies to accelerate the industrial AI revolution at CES 2026"</a>,
          January 6, 2026.
        </li>
        <li id="ref10">
          Tianyi Dai et al.,
          <a href="https://www.nature.com/articles/s41586-024-08173-7">"Autonomous mobile robots for exploratory synthetic chemistry"</a>,
          <em>Nature</em>, 2024; and Daniel A. Boiko et al.,
          <a href="https://www.nature.com/articles/s41586-023-06792-0">"Autonomous chemical research with large language models"</a>,
          <em>Nature</em>, 2023.
        </li>
        <li id="ref11">
          R. B. Canty et al.,
          <a href="https://www.nature.com/articles/s41467-025-59231-1">"Science acceleration and accessibility with self-driving labs"</a>,
          <em>Nature Communications</em>, 2025; and L. Hung et al.,
          <a href="https://pubs.rsc.org/en/content/articlelanding/2024/dd/d4dd00059e">"Autonomous laboratories for accelerated materials discovery: a community survey and practical insights"</a>,
          <em>Digital Discovery</em>, 2024.
        </li>
      </ol>
    </section>

    <div class="bottom-strip">
      <a class="back" href="/#writing">← back to writing</a>
    </div>

  </main>

  <footer class="site-footer">
    &copy; Sean Howell. Built with HTML &amp; CSS. Set in Berkeley Mono and IBM Plex Sans.
  </footer>

</body>
</html>
```

- [ ] **Step 2: Verify the essay renders and all eleven footnotes jump correctly**

```bash
cd /Users/seanhowell/dev/sghowell.github.io
python3 -m http.server 8000
```

Load `http://localhost:8000/writing/microfauna-of-megaprojects.html` and confirm the same items as Task 5 step 3. Additionally, the "small agents arrive first" section has two adjacent footnote superscripts (refs 10 and 11); confirm both render and link correctly. Stop the server.

- [ ] **Step 3: Commit**

```bash
git add writing/microfauna-of-megaprojects.html
git commit -m "writing: add 'On the Microfauna of Megaprojects' essay"
```

---

## Task 7: Add writing section and anchor-rail link to index.html, update footer

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add `writing` to the anchor rail**

Find the current anchor rail (around line 18–20):

```html
<nav class="anchor-rail" aria-label="Sections">
  <a href="#about">about</a><span class="sep">/</span><a href="#experience">experience</a><span class="sep">/</span><a href="#projects">projects</a><span class="sep">/</span><a href="#publications">publications</a><span class="sep">/</span><a href="#contact">contact</a>
</nav>
```

Replace with:

```html
<nav class="anchor-rail" aria-label="Sections">
  <a href="#about">about</a><span class="sep">/</span><a href="#experience">experience</a><span class="sep">/</span><a href="#projects">projects</a><span class="sep">/</span><a href="#writing">writing</a><span class="sep">/</span><a href="#publications">publications</a><span class="sep">/</span><a href="#contact">contact</a>
</nav>
```

- [ ] **Step 2: Insert the writing section between `projects` and `publications`**

Find the closing `</section>` of the `projects` section (immediately before `<section id="publications" class="pubs">`) and insert this block after it:

```html
    <section id="writing">
      <h2>writing</h2>

      <div class="entry writing">
        <div class="lhs">2026-03</div>
        <div class="rhs">
          <div class="title"><a href="/writing/microfauna-of-megaprojects.html">On the Microfauna of Megaprojects &rarr;</a></div>
          <div class="desc">Why the biggest machines of the next decade will be inhabited by dense ecologies of small machine intelligences&mdash;and why the organizations that matter should start cultivating them now.</div>
        </div>
      </div>

      <div class="entry writing">
        <div class="lhs">2026-02</div>
        <div class="rhs">
          <div class="title"><a href="/writing/ai-control-plane.html">AI as the Control Plane for Impossible Machines &rarr;</a></div>
          <div class="desc">Why utility-scale quantum computers, autonomous laboratories, advanced fabs, and large robotic systems will need advanced AI as an operating layer&mdash;not as a convenience feature, but as part of the machine.</div>
        </div>
      </div>

    </section>
```

Newest entry (March 2026) is listed first; February entry follows.

- [ ] **Step 3: Update the footer text to mention both fonts**

Find the footer (currently line 209–211):

```html
<footer class="site-footer">
  &copy; Sean Howell. Built with HTML &amp; CSS. Set in Berkeley Mono.
</footer>
```

Replace with:

```html
<footer class="site-footer">
  &copy; Sean Howell. Built with HTML &amp; CSS. Set in Berkeley Mono and IBM Plex Sans.
</footer>
```

- [ ] **Step 4: Verify the index page**

```bash
cd /Users/seanhowell/dev/sghowell.github.io
python3 -m http.server 8000
```

Load `http://localhost:8000/` and confirm:
1. Anchor rail shows `about / experience / projects / writing / publications / contact`.
2. Clicking `writing` in the rail jumps to the new section.
3. The writing section renders with Microfauna on top and Control Plane below, each title underlined faintly and hover-darkening.
4. Clicking either title navigates to the corresponding essay page.
5. Footer reads "Set in Berkeley Mono and IBM Plex Sans".
6. All other sections (experience, projects, publications, contact) look unchanged.
7. Toggle dark mode and confirm the new section inverts correctly.

Stop the server.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "content: add writing section with two essays and update footer"
```

---

## Task 8: Final cross-page verification pass

**Files:**
- None modified. Pure manual verification.

- [ ] **Step 1: Start the server**

```bash
cd /Users/seanhowell/dev/sghowell.github.io
python3 -m http.server 8000
```

- [ ] **Step 2: Full navigation flow, light mode**

1. Open `http://localhost:8000/`.
2. Click `writing` in the anchor rail → confirm smooth jump to the writing section.
3. Click the Microfauna title → confirm navigation to the essay page.
4. Click "← back" in the top strip → confirm return to `/#writing`.
5. Click the Control Plane title → confirm navigation.
6. Click "← back to writing" in the bottom strip → confirm return to `/#writing`.
7. Back on the essay pages, click a footnote superscript → confirm jump to the correct `#refN` entry and that external links in source entries open to the right URL (right-click → copy link address to confirm without navigating away).

- [ ] **Step 3: Responsive check**

Open DevTools → device toolbar → narrow to 375px width. Confirm:
1. Index writing section reflows with date stacked above title (existing `.entry` mobile rule handles this).
2. Each essay page's top strip wraps cleanly, body text reflows, pullquote still renders.

- [ ] **Step 4: Dark mode check**

Toggle system color scheme to dark (macOS: System Settings → Appearance → Dark; or DevTools → Rendering → Emulate CSS media feature → `prefers-color-scheme: dark`). Reload each page:
1. Index page inverts as before, writing section included.
2. Each essay page inverts cleanly: body text `--fg` on dark `--bg`, Plex stays readable, pullquote border inverts, footnotes stay legible.

- [ ] **Step 5: Print preview check**

On each essay page, `Cmd+P` → confirm the preview renders to a clean single article (the previous essay-source print styles no longer apply since we dropped all that CSS — the default site styles print acceptably). No need to produce a PDF unless a rendering issue is visible in preview.

- [ ] **Step 6: Stop the server**

`Ctrl+C`.

- [ ] **Step 7: Final status check**

```bash
git status
git log --oneline -10
```

Expected: working tree clean, recent commits in order:
1. fonts: add self-hosted IBM Plex Sans (regular, italic, semibold)
2. style: register IBM Plex Sans font-faces and prose variable
3. style: generalize title-link rule from .project to .entry
4. style: add scoped essay-page layout rules
5. writing: add 'AI as the Control Plane for Impossible Machines' essay
6. writing: add 'On the Microfauna of Megaprojects' essay
7. content: add writing section with two essays and update footer

No eighth commit — Task 8 is verification only.

---

## Self-Review Notes

- **Spec coverage:** Every item in the spec has a corresponding task. Fonts → Task 1. `@font-face` and `--font-prose` → Task 2. Generalizing title-link selector → Task 3 (scope expansion; minor addition not explicitly in spec but enables the writing entries to pick up the same affordance). Essay CSS scope → Task 4. Essay HTML files → Tasks 5 and 6. Index changes (rail + section + footer) → Task 7. Validation pass → Task 8.
- **Placeholder scan:** All code blocks are complete. No "similar to above", no "TBD", no "add appropriate…".
- **Type consistency:** Class names are consistent throughout — `.essay`, `.top-strip`, `.brand`, `.back`, `.essay-header`, `.eyebrow`, `.dek`, `.date`, `.article`, `.pullquote`, `.sources`, `.bottom-strip`. All HTML files use the same names. Anchor target `#writing` is referenced by rail link, top strip "← back", bottom strip "← back to writing", and section `id`.
- **Known trade-off:** The existing `section > h2::before` rule in `style.css` already adds `// ` before any `h2` inside any `section` element. The essay `h2` inside `article.article` is NOT inside a `section` (it's inside `article`), so the existing rule doesn't fire — which is why Task 4 adds an explicit `.essay .article h2::before` rule. The `.essay .sources h2` is inside `<section class="sources">`, so it would pick up the existing rule AND the explicit rule in Task 4. Both render identical `// ` prefixes, so there's no visual conflict, but the explicit rule keeps the essay scope self-contained. Leaving both is fine.
