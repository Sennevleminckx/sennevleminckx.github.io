# sennevleminckx.be

Personal academic site — a CV that also offers consultancy. Built with
[Quarto](https://quarto.org) (HTML website), self-hosted fonts, and a single SCSS
theme. No JavaScript framework, no build step beyond Quarto.

## Run locally

Requires Quarto (built with 1.9.x):

```bash
quarto preview      # live-reloading local server
quarto render       # one-off build into _site/ (git-ignored)
```

A build must finish with **no warnings**.

## Project layout

```
index.qmd          Home — who I am, what I research, links
cv.qmd             Curriculum vitae (built from the real CV)
publications.qmd   Rendered from bib/*.bib
consultancy.qmd    The consultancy offer
contact.qmd        Email + profiles
legal.qmd          Company details + privacy notice
bib/               One .bib per publication status (see below)
assets/reverse-chron.csl   Citation style (newest first, links DOIs/URLs)
styles.scss        The whole theme (quiet, sans-serif, portrait-led)
assets/fonts/      Self-hosted IBM Plex Sans (400/500/600)
assets/cv.pdf      Downloadable CV (see below)
assets/img/        Portrait (TODO) + favicon
CNAME              Custom domain; copied into _site on every render
```

Navbar: **Home · CV · Publications · Consultancy**, with **Contact** at the right.
Footer links to **Legal & privacy**.

## Add a publication

Publications live in `bib/`, split by status:

- `bib/peer-reviewed.bib`
- `bib/under-review.bib`
- `bib/in-preparation.bib`
- `bib/outputs.bib` (datasets, protocols)
- `bib/thesis.bib`

Add one BibTeX entry to the matching file. Include `doi = {...}` (or `url = {...}`
for datasets/registrations); it renders as a link. The Publications page groups by
these files (via the committed `_extensions/pandoc-ext/multibib` filter) and lists each
group newest first — no other edit needed.

## The CV PDF

`assets/cv.pdf` is generated from the separate LaTeX CV project (`~/Downloads/autoCV`):

```bash
cd ~/Downloads/autoCV && latexmk -pdf cv.tex
cp cv.pdf ~/senne-site/assets/cv.pdf
```

The on-site CV (`cv.qmd`) and the PDF are maintained together — update both when your
CV changes.

## Design in one paragraph

Quiet and restraint over ornament: one centred column, generous whitespace, hairline
rules, IBM Plex Sans throughout, a near-monochrome palette with a single restrained
link colour. The home page leads with a portrait, a one-line role, a short plain
statement, and a row of external links — academic-first, consultancy second. Fonts are
self-hosted; colours meet WCAG AA; motion respects `prefers-reduced-motion`.

## Deploy

See **[DEPLOY.md](DEPLOY.md)** for GitHub Pages, the custom domain, and the exact DNS
records.

## Placeholders still to fill (`TODO(senne)`)

```bash
grep -rn "TODO(senne)" . --include=*.qmd --include=*.bib --include=*.yml
```

Current list: headshot (`assets/img/senne.jpg`); University staff-profile URL; whether
to use a separate consultancy email; day-rate decision; ORCID-sync vs manual bib; legal
identifiers (trading name, address, ondernemingsnummer, BTW) and the analytics clause.
