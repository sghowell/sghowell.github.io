# Writing section — design spec

Date: 2026-04-14

## Goal

Add a "writing" destination to sghowell.github.io that currently hosts two essays, each readable on its own URL. Visual identity of the one-pager is preserved; long-form reading gets a body font built for it.

## Scope

- New section on `index.html` listing available essays, linked from the anchor rail.
- Two standalone essay pages under `/writing/`.
- Shared `style.css` extended with essay-scoped rules.
- Self-hosted IBM Plex Sans added to `fonts/`.

Out of scope: separate writing index page, RSS, tags, reading-time estimates, drop caps, essay-specific background art, JS.

## Files touched or created

- `index.html` — add `<section id="writing">` between `projects` and `publications`; add `writing` to the anchor rail.
- `style.css` — add `@font-face` block for IBM Plex Sans (regular, italic, semibold, woff2, self-hosted), plus a `.essay` scope with rules for essay header, article body, pullquote, sources list, back-links.
- `fonts/IBMPlexSans-Regular.woff2` (new)
- `fonts/IBMPlexSans-Italic.woff2` (new)
- `fonts/IBMPlexSans-SemiBold.woff2` (new)
- `fonts/IBMPlexSans-LICENSE.md` (new) — OFL 1.1 license text from upstream
- `writing/ai-control-plane.html` (new) — essay 1, restyled to site aesthetic
- `writing/microfauna-of-megaprojects.html` (new) — essay 2, restyled to site aesthetic

## Design decisions

### Typography

- Chrome (masthead, eyebrows, section labels, dates, footnotes, nav, pullquote attribution if any): Berkeley Mono, unchanged.
- Essay body paragraphs and pullquote copy: IBM Plex Sans ~17px, line-height ~1.7, max-width ~64ch.
- No drop cap. The source essays have drop caps in a serif face; they fight Berkeley Mono visually.
- Font feature settings: keep `ss01, ss02` for Berkeley Mono; no special features for Plex.

### Palette

Essays inherit the existing `--bg / --fg / --muted / --faint / --rule / --subtle` variables and the `prefers-color-scheme: dark` media block. No essay-specific colors.

### Index-page writing section

Shape matches existing `experience` and `projects` `.entry` grid so it reads as native:

```
// writing

2026-02   essay   AI as the Control Plane for Impossible Machines →
                  dek line pulled from source essay
2026-03   essay   On the Microfauna of Megaprojects →
                  dek line pulled from source essay
```

Entries are ordered newest-first on the index; `Microfauna` appears above `Control Plane`.

- `lhs`: date (e.g., `2026-04`) and a small `essay` label.
- `rhs`: title (link to essay) + one-line dek.
- Hover/focus affordance: same underline-border treatment as project titles.

### Essay page structure

Top to bottom:

1. **Top strip** — `SEAN HOWELL / WRITING` in Berkeley Mono uppercase at masthead scale, plus `← back` link to `/#writing`. Left-aligned, same max-width container as index.
2. **Essay header** — `// essay` eyebrow, `<h1>` title in Berkeley Mono 700, dek paragraph (muted), date line. Same `border-top` rule as index sections.
3. **Body** — IBM Plex Sans, paragraphs, `<h2>` subheads in Berkeley Mono uppercase with `//` prefix (matching existing `section > h2::before`), pullquotes, numbered footnote anchors.
4. **Pullquote** — left border rule in `--fg`, italic Plex at ~1.15em, no card, no shadow, no cream background. Just rule + text.
5. **Sources list** — `// sources` heading; ordered list matching `.pubs` styling; footnote anchors link back and forth via `id` + `href`.
6. **Bottom strip** — `← back to writing` link, then the global `site-footer` line.

Container: reuse `.container` with max-width tightened to ~64ch for the article body wrapper so long prose doesn't run too wide. The top/bottom strips can span the standard 78ch.

### Styling scope

Essay rules live under a `.essay` class applied to `<body>` on essay pages. Rules are scoped as `.essay .article`, `.essay .pullquote`, etc. The index page never sees `.essay` and never loads Plex rules in a way that affects it. Both pages load the same `style.css`; Plex `@font-face` declarations are cheap and don't cause network fetches unless referenced by an applied rule.

### Font sourcing

IBM Plex Sans is OFL 1.1. Pull official woff2 files (Regular, Italic, SemiBold — weights 400, 400 italic, 600). Latin + Latin-ext subset. Store under `fonts/` alongside Berkeley Mono. Include upstream `LICENSE.md` as `fonts/IBMPlexSans-LICENSE.md`.

### Footer note

Update the global footer to mention both faces: `Built with HTML & CSS. Set in Berkeley Mono and IBM Plex Sans.`

## Content mapping from source HTML

For each essay, preserve: title, dek, section headings (`h2`), paragraphs (including `<em>` emphasis), pullquote, numbered footnote markers, and the sources list with its URLs. Drop: all source-file CSS, `.sheet`/`.hero`/`.article-wrap`/`.rule` scaffolding, drop-cap class, meta pills, eyebrow divider decoration.

Essay 1 title: "AI as the Control Plane for Impossible Machines" (February 2026)
Essay 2 title: "On the Microfauna of Megaprojects" (March 2026)

## Validation

- Visual pass: load `index.html` in a browser (light + dark), confirm writing section renders and links work.
- Visual pass: load each essay page (light + dark), confirm Plex body, Berkeley Mono chrome, working footnotes, working back link.
- Print pass: `Cmd+P` on each essay to confirm it prints cleanly (no broken card shadows, no cut pullquotes). The site already has no JS so no runtime checks required.
- Link pass: click every footnote marker and the back links.
- Responsive pass: narrow viewport (~375px) and confirm the writing section, essay header, and body all reflow.
