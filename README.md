# sennevleminckx.be

Personal site — living CV and consultancy shopfront for Senne Vleminckx.
Built with [Quarto](https://quarto.org) (HTML website), self-hosted fonts, a
single SCSS theme, and no JavaScript framework.

## Run locally

Requires Quarto (this was built with 1.9.x):

```bash
quarto preview      # live-reloading local server
quarto render       # one-off build into _site/ (git-ignored)
```

A build must finish with **no warnings**.

## Project layout

```
index.qmd          Home (client-facing)
services.qmd       What I offer
work.qmd           Case-study index (auto-listing over work/)
work/_template.qmd Copy this to add a case study
work/NN-*.qmd      Case studies (three stubs for now)
cv.qmd             Academic CV
publications.qmd   Rendered from references.bib
contact.qmd        Email + profiles
legal.qmd          Company details + privacy notice
references.bib     Publications source (edit this to add one)
styles.scss        The whole theme (palette, type, layout, rota motif)
assets/fonts/      Self-hosted woff2 (IBM Plex Sans/Mono, Source Serif 4)
assets/cv.pdf      Downloadable CV (placeholder — replace)
CNAME              Custom domain; copied into _site on every render
```

Navbar: **Home · Services · Work · CV · Publications**, with **Contact** at the
right. Footer links to **Legal & privacy**.

## Add a case study

1. Copy the template:
   ```bash
   cp work/_template.qmd work/04-short-slug.qmd
   ```
2. Fill in `title`, `subtitle`, `categories`, and the five sections. Keep it
   150–250 words. Keep the final **“What I’d do differently”** section — it is
   deliberate.
3. `quarto preview` — it appears on `work.qmd` automatically, no index editing.

## Add a publication

Add one entry to `references.bib`. Type controls grouping:

- `@article` — journal articles
- `@inproceedings` — conference contributions
- `@phdthesis` / `@mastersthesis` — theses

Include a `doi = {...}` field where one exists; it renders as a link. The
Publications page picks it up automatically (newest first) via `nocite: "@*"`.

> The two entries currently in `references.bib` are **format examples**.
> Replace them; do not ship them.

## Design in one paragraph

One measured reading column; structure comes from hairline rules and space, not
boxes. Headings in **IBM Plex Sans**, body in **Source Serif 4**, small labels
and numbers in **IBM Plex Mono** (the “data voice”). Palette is a cool
off-white paper with deep-slate ink and a single **brick accent** spent in one
place: the **rota band** — an abstract nurse roster where one cell is
highlighted, the signal in the scheduling data. All colours meet WCAG AA;
motion respects `prefers-reduced-motion`; fonts are self-hosted.

## Deploy

See **[DEPLOY.md](DEPLOY.md)** for GitHub Pages, the custom domain, and the
exact DNS records.

## Placeholders still to fill (`TODO(senne)`)

Search the project for `TODO(senne)`:

```bash
grep -rn "TODO(senne)" . --include=*.qmd --include=*.bib --include=*.yml
```

Current list: headshot; real `assets/cv.pdf`; CV awarding institution, nurse
degree, teaching/supervision/service entries; real publications; contact email;
LinkedIn + ORCID; legal identification (trading name, address, ondernemingsnummer,
BTW); the case-study bodies; and the day-rate / ORCID-sync / analytics decisions.
