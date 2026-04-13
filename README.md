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
