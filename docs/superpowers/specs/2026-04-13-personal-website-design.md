# Personal Website Design — sghowell.github.io

**Date:** 2026-04-13
**Owner:** Sean Howell
**Status:** Draft — pending spec review

## 1. Goal

A minimal, static personal website for Sean Howell, deployed to `https://sghowell.github.io` via GitHub Pages. The site is a durable home-on-the-internet: it introduces who Sean is, summarizes his research and engineering work, catalogs ongoing projects, and provides contact information.

Non-goals: blogging, analytics, SEO optimization, forms, dynamic content, a build pipeline, JavaScript frameworks, third-party dependencies at runtime.

## 2. Design direction

**Systems / Terminal aesthetic.** Monospace typography (Berkeley Mono), pure grayscale palette, hairline rules, tight but breathable layout, zero color accents. The site should read top-to-bottom like a well-designed man page or a plan9 manual entry. This choice signals the engineering-depth of Sean's work (Rust, CUDA, systems software, HPC) and is deliberately distinct from the sea of Swiss-minimalist developer portfolios.

### Rationale

Berkeley Mono is a distinctive, licensed typeface with a strong engineering-graphics heritage. Its use as the sole typographic system gives the site a coherent identity without ornament. Hierarchy comes from weight (400 / 600 / 700), size, and vertical spacing — not from multiple typefaces or colors.

## 3. Technical constraints

- **No build tools.** Plain HTML + CSS. No bundler, no preprocessor, no package manager.
- **No JavaScript at runtime.** The site is 100% static and functional with JS disabled.
- **No external network requests at runtime.** All fonts, assets, and content are self-hosted. No Google Fonts, no CDNs, no analytics pixels.
- **One HTML file, one CSS file.** Single-page, anchor-navigated.
- **GitHub Pages deployment** from the repo root (`main` branch).
- **Responsive.** Works well on mobile down to 320px width and up to wide desktop.
- **Dark mode** via `prefers-color-scheme` — pure CSS, no toggle.

## 4. Information architecture

Single page. A masthead at the top, then five content sections in this order:

0. **Header** (masthead, not a nav bar)
1. **About**
2. **Experience**
3. **Projects**
4. **Publications**
5. **Contact**

A small anchor rail under the header links to each content section (`about / experience / projects / publications / contact`). There is no sticky top nav — the page is short enough to scroll.

### 4.1 Header

- Name: `SEAN HOWELL` (uppercase, letter-spaced, heaviest weight).
- Tagline: `researcher // engineer // technical leader` (muted, lower weight).
- Location: `Los Angeles · San Francisco Bay Area` (muted).
- Anchor rail: plain text links separated by ` / `, all lowercase, muted.
- Resume link: a small `[ download resume (pdf) ]` link that points to `/assets/seanhowell-resume-042026.pdf`.

### 4.2 About

A distilled version of the PDF summary (2–3 sentences). Closing line: `B.S. Physics & Mathematics, University of Maryland, College Park` as a right-aligned or muted single-line footer to the about block.

Content:

> Researcher and engineer focused on AI-driven scientific discovery — building agents, models, and infrastructure that allow machines to participate meaningfully in physics and mathematics research and control complex systems. Recent work includes neural decoders and RL for fault-tolerant quantum computing, VLA models for autonomous experimental science, autonomous agent systems for physics research, formalization and verification, and compiler toolchains grounded in ZX-Calculus. Fifteen years in AI/ML, with R&D leadership across three quantum computing organizations.

### 4.3 Experience

Two sub-blocks:

**Featured roles** (full treatment — date, title, company, location, 2–3 bullet highlights):

1. **PsiQuantum** — Principal Engineer, AI for Quantum & Director, Quantum OS (2024 – Present, Palo Alto, CA)
   - Founded and leading AI for Quantum R&D: ML-based QEC decoders, RL for adaptivity in fault-tolerant quantum computing, scientific agents for physics research, multi-agent systems for calibration, control, and operation of quantum systems.
   - Building agentic discovery infrastructure for scientific AI: harnesses, trajectory capture, structured evaluation of AI-generated scientific artifacts.
   - Directed Quantum OS: designed scalable control plane, fabrics, OS software for utility-scale photonic QC. Grew engineering org by 15+ ICs YoY.

2. **Independent Research & Development — AI for Science & Systems** (2025 – Present, Los Angeles, CA)
   - Developing architectures for AI-assisted scientific reasoning: evaluation, harnesses, provenance systems, and long-horizon agent frameworks for physics and mathematics.
   - Research at the intersection of reasoning models and formal methods, targeting formalization of results and open problems in quantum information science.
   - Structured representations of reasoning (typed graphs over claims, evidence, derivations, simulations, proofs) for verifiable multi-agent collaboration.

3. **AWS Center for Quantum Computing** — Software Engineering Manager (2022 – 2024, Pasadena, CA)
   - Led engineering and research in quantum compilers, control software/hardware, QEC frameworks, and a distributed runtime for quantum information processing systems.
   - Built and mentored a team of scientists and engineers (0–30 years experience) developing scalable, fault-tolerant quantum computing systems.

4. **Q-CTRL** — Head of US Division (2019 – 2022, Los Angeles, CA)
   - Led US R&D, software engineering, and product. Grew team 0→20 engineers, drove 10× YoY revenue growth, achieved 9000× algorithm performance improvement.
   - First demonstration of reinforcement learning for gateset design and calibration on a superconducting quantum processor. Pioneering work in AI for NISQ hardware.

**Prior positions** (compact list — date, role, company, one-line description each):

- **2018 – 2019** — Quantitative Research Engineer, BlackRock. *Quantitative research and ML for systematic trading strategies.*
- **2017** — AI Software Engineer, Cogitai. *Continual learning research and RL for robotics.*
- **2016 – 2017** — Senior Scientist, Areté Associates. *Deep learning for computer vision and chemical detection in hyperspectral data.*
- **2015 – 2016** — Computer Scientist, Novetta. *Deep learning for web-scale multimedia classification and indexing.*
- **2013 – 2015** — Research Engineer, NASA Goddard Space Flight Center. *ML for multimodal geoscience and remote sensing.*
- **2012 – 2013** — Software Engineer, Oceaneering Advanced Technologies. *Deep learning and computer vision for autonomous underwater robotics.*
- **2009 – 2011** — Research Assistant, University of Maryland. *Scientific ML for cosmology and gravitational wave astrophysics.*

### 4.4 Projects

14 curated projects from Sean's GitHub (mix of public and private repos — private repo names are explicitly approved for public listing by the owner). Grouped by theme, with a compact entry format per project:

```
[lang]  repo-name  —  description  (public|private)
```

**Group 1 — Agents & research infrastructure**

1. **lem** — Scientific agent for physicists unifying an Aletheia-style GVR loop and RLM techniques. `rust, private`
2. **deep-gvr** — Minimal generate-validate-revise loop for Hermes-Agent, inspired by DeepMind's Aletheia agent for autonomous research with empirical, formal, and human verification. `python, public`
3. **skaffen-mono** — General-purpose personal research, development, engineering, and inquiry agent. `rust, private`
4. **culture** — Multi-agent orchestration and AI research assistants. `rust, private`
5. **parallax-science** — A platform and protocols for AI agents to engage in structured scientific discourse. `python, private`
6. **hilbert** — Solving open problems in quantum information science using AI. `tex, private`

**Group 2 — ZX-calculus & fault-tolerant quantum computing**

7. **loom-zx** — A ZX-Calculus focused proof-carrying FTQC platform. `rust, private`
8. **zx-webapp** — Interactive webapp for exploring fault tolerance for quantum computing in the ZX-Calculus framework. `python, private`

**Group 3 — Quantum error correction & decoders**

9. **nano-qec** — Autoresearch-inspired neural decoder training using Hermes Agent and Stim. `python, public`
10. **sabaki** — RL training for QEC pre-decoders using Pufferlib, Stim, PyMatching, and sinter. `python, private`
11. **open-qvla** — Open vision-language-action models for quantum control, calibration, and error correction. `python, private`

**Group 4 — Systems & platforms**

12. **bandalong** — Authority plane reference implementation in Rust. `rust, private`
13. **chapterhouse** — Reference intelligence plane implementation. `go, private`
14. **pelagos** — A modern, scientist-focused platform for oceanography. `python, private`

### 4.5 Publications

Three selected publications as compact one-line entries (author list, title italicized, venue, year):

1. H. Levine, A. Haim, et al. *Demonstrating a long-coherence dual-rail erasure qubit using tunable transmons.* Physical Review X, 2024.
2. Y. Baum, M. Amico, S. Howell, et al. *Experimental Deep Reinforcement Learning for Error-Robust Gateset Design on a Superconducting Quantum Computer.* PRX Quantum, 2021.
3. Y. Baum, S. Howell, et al. *Reinforcement Learning for Error Robust Control on Cloud-Based Superconducting Hardware.* Bulletin of the American Physical Society, 2021.

### 4.6 Contact

Four contact methods, each as an icon link (inline SVG) + label:

- **Email** — `sean.g.howell@gmail.com` (`mailto:` link)
- **GitHub** — `github.com/sghowell` → `https://github.com/sghowell`
- **Google Scholar** — `google scholar` → `https://scholar.google.com/citations?hl=en&user=lh-wJ1EAAAAJ`
- **X (Twitter)** — `@ControlFreq` → `https://x.com/ControlFreq`

No phone number. No form. No LinkedIn (not requested).

## 5. Typography

### 5.1 Font stack

```css
@font-face {
  font-family: "Berkeley Mono";
  src: url("/fonts/BerkeleyMonoVariable.woff2") format("woff2");
  font-weight: 100 900;
  font-style: normal;
  font-display: swap;
}

:root {
  --font: "Berkeley Mono", ui-monospace, "SF Mono", Menlo, Consolas, monospace;
}
```

Berkeley Mono is a variable font supplied as a single `.woff2` file (~166KB). A single `@font-face` declaration with a weight range covers all weights. If the font fails to load, the system monospace fallback maintains the mono aesthetic gracefully.

### 5.2 Scale & hierarchy

All sizes in rem, base `16px`:

- Name (`h1`): `1.625rem`, weight 700, letter-spacing `0.06em`, uppercase
- Section header (`h2`): `0.8125rem`, weight 700, letter-spacing `0.14em`, uppercase, prefixed with `// ` in content
- Entry title (`h3` or `.role`): `0.875rem`, weight 700
- Body text: `0.875rem`, weight 400, line-height `1.6`
- Muted / meta: `0.75rem`, weight 400, muted color
- Tagline: `0.8125rem`, weight 400, muted

### 5.3 Line length

Content max-width `78ch` (monospace characters are wider than proportional, so 78ch ≈ 72ch proportional — comfortable reading length).

## 6. Color palette (monochrome)

Two modes, auto-switched via `prefers-color-scheme`. No color accents; no brand color.

### Light mode
```
--bg:       #fafafa   /* page background */
--fg:       #111111   /* primary text */
--muted:    #666666   /* meta, dates, descriptions */
--faint:    #999999   /* tertiary text, separators */
--rule:     #111111   /* hairline rules (section tops) */
--subtle:   #e5e5e5   /* dividers between entries */
```

### Dark mode
```
--bg:       #0e0e0e
--fg:       #ededed
--muted:    #8a8a8a
--faint:    #5f5f5f
--rule:     #ededed
--subtle:   #1f1f1f
```

Links: underlined, no color change. `:hover` thickens the underline or switches to `text-decoration-style: double`. `:focus-visible` uses a 2px outline in `--fg`.

## 7. Layout

### 7.1 Grid

Single column, left-aligned. Container:

```css
.container {
  max-width: 78ch;
  margin: 0 auto;
  padding: 48px 32px 96px;
}
@media (max-width: 640px) {
  .container { padding: 32px 20px 64px; }
}
```

### 7.2 Entry grid (two-column)

Inside experience and project sections, each entry uses a two-column grid:

```css
.entry {
  display: grid;
  grid-template-columns: 150px 1fr;
  gap: 24px;
  padding: 12px 0;
}
@media (max-width: 640px) {
  .entry { grid-template-columns: 1fr; gap: 6px; }
}
```

Left column: date (experience) or language tag (projects). Right column: title + description.

### 7.3 Section rules

Each section begins with a 1px top rule in `--rule` color and a small uppercase header below. Entries within a section are separated by a 1px `--subtle` rule.

## 8. Responsive behavior

- ≥ 640px: two-column entry grid.
- < 640px: single-column stacked entries; reduced padding; unchanged type scale (mono at 14px is already mobile-friendly).
- No viewport meta shenanigans: `<meta name="viewport" content="width=device-width, initial-scale=1">`.

## 9. Font licensing compliance

The Berkeley Mono Personal Font License (DX-100-05, April 2023) grants Web Font Use for personal websites under §1.08 and §2 Table 2.1, with these constraints:

1. **Only `.woff2` is hosted.** No `.otf`, `.ttf`, `.woff`. The source file is `Berkeley Mono Variable.woff2`.
2. **`@font-face` only.** The font is referenced exclusively through the CSS `@font-face` rule. There are no `<a href="...woff2">` links anywhere on the site.
3. **No direct distribution.** No mention of the font file path in HTML content, no "download" link, no documentation that would aid casual copying.
4. **License acknowledgment.** A `fonts/LICENSE.md` file documents:
   - The font is Berkeley Mono, licensed under a personal license to Sean Howell.
   - Redistribution is prohibited.
   - The file is used solely for Web Font Use on this site per the license.
5. **Single-user scope.** This is a single personal site. The site is non-commercial.

### Residual exposure

The file `fonts/BerkeleyMonoVariable.woff2` will be reachable via `raw.githubusercontent.com/sghowell/sghowell.github.io/main/fonts/BerkeleyMonoVariable.woff2` because GitHub Pages user-site repos must be public. This is a side effect of the hosting platform, not an intentional public link. We honor §5 by not referencing that URL anywhere and by including the license acknowledgment. The licensee has read and accepted this residual exposure.

## 10. Deployment

### 10.1 Repo setup

Current state: repo is initialized locally, has `LICENSE`, `README.md`, and the resume PDF. No remote is configured. Branch is `main`.

Steps:

1. Move `seanhowell-resume-042026.pdf` from the repo root to `assets/seanhowell-resume-042026.pdf` (via `git mv` to preserve history).
2. Verify no GitHub repo exists at `sghowell/sghowell.github.io` via `gh repo view`. If it does not exist, create it via `gh repo create sghowell/sghowell.github.io --public --source=. --remote=origin`. If it exists, add the SSH remote manually.
3. Confirm the working tree contains all intended files.
4. Commit.
5. Push to `main`.

### 10.2 GitHub Pages configuration

User sites (`<username>.github.io`) are automatically published from the `main` branch root when the repo is public. No workflow YAML needed.

- Add `.nojekyll` to disable Jekyll processing (we are not a Jekyll site).
- No custom domain for v1.
- Confirm Pages is enabled via `gh api repos/sghowell/sghowell.github.io/pages` after push; enable via `gh api --method POST repos/sghowell/sghowell.github.io/pages -f source[branch]=main -f source[path]=/` if needed.
- Final URL: `https://sghowell.github.io/`.

### 10.3 Verification

After deployment, confirm:
- `https://sghowell.github.io/` returns HTTP 200 and renders the page.
- `https://sghowell.github.io/fonts/BerkeleyMonoVariable.woff2` returns the font (MIME type `font/woff2`).
- `https://sghowell.github.io/assets/seanhowell-resume-042026.pdf` returns the PDF.
- Page works with JS disabled.
- Dark mode renders correctly (test via OS toggle or devtools).
- Mobile viewport renders correctly.

## 11. File structure (target)

```
sghowell.github.io/
├── .git/
├── .github/                         (reserved, unused for v1)
├── .gitignore                       # existing — adds .superpowers/ and .DS_Store
├── .nojekyll                        # new, empty file
├── LICENSE                          # existing
├── README.md                        # updated — site description + dev notes
├── index.html                       # new — the site
├── style.css                        # new — all styling
├── fonts/
│   ├── BerkeleyMonoVariable.woff2   # copied from ~/Downloads/...
│   └── LICENSE.md                   # font license acknowledgment
├── assets/
│   └── seanhowell-resume-042026.pdf # moved from repo root
└── docs/
    └── superpowers/
        └── specs/
            └── 2026-04-13-personal-website-design.md  # this file
```

The root-level PDF is relocated to `assets/` to keep the repo root tidy.

## 12. Icons

Four inline SVG icons for the contact section. All are drawn in a 24×24 viewBox, stroke or fill `currentColor`, sized at 16px in the document. Icon glyphs used:

- **Email** — simple envelope outline (SVG path).
- **GitHub** — the standard Octocat silhouette, simplified.
- **Google Scholar** — the graduation-cap-over-shield glyph, simplified.
- **X** — the simple two-stroke glyph (two crossed strokes forming an X), drawn with `currentColor` strokes.

Icons live inline in `index.html` so there are no external font or image requests. CSS class `.icon` handles sizing and vertical alignment.

## 13. README update

Replace the one-line README with:

- Brief description of the site.
- How to preview locally (`python -m http.server` or `open index.html`).
- Note that Berkeley Mono is self-hosted under a personal license and must not be redistributed.
- Link to the design spec for future reference.

## 14. Out of scope (v1)

- Blog / posts / RSS.
- Light/dark toggle button (we rely on `prefers-color-scheme`).
- Analytics of any kind.
- Contact form.
- Custom domain.
- Sitemap.xml, robots.txt customization.
- Open Graph / Twitter Card meta tags. (v1 ships only `charset`, `viewport`, `title`, and a single `description` meta tag.)
- Favicon (may be added as a trivial follow-up; not required for v1).
- Print stylesheet.

## 15. Open questions

None. All decisions have been made and approved.

## 16. Acceptance criteria

The v1 is complete when:

1. `https://sghowell.github.io/` renders the site with the content and layout described.
2. Berkeley Mono loads correctly.
3. The page works with JavaScript disabled.
4. The page passes visual inspection in Safari, Chrome, and Firefox on macOS, and in Safari on iOS (or a narrow-viewport approximation).
5. Dark mode renders correctly when the OS is in dark mode.
6. All contact links work.
7. The resume PDF link downloads the file.
8. The repo has no uncommitted files except ignored paths.
9. GitHub Pages reports a successful deploy.
