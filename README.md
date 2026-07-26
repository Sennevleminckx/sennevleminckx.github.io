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

Only `*.qmd` files render as site pages (`project.render` in `_quarto.yml`). The repo
docs — this README, `TODO.md`, `DEPLOY.md` — are deliberately **not** published.

## Project layout

```
index.qmd          Home — role, one-line research statement, two CTAs, portrait
cv.qmd             Curriculum vitae (one line per entry; the PDF is the full record)
publications.qmd   Hand-authored, grouped by status, in the three-column row pattern
consultancy.qmd    The consultancy offer (Data analysis + five secondary services)
work.qmd           Case-study listing (Quarto listing over work/*.qmd)
work/              One .qmd per case study (+ _template.qmd, currently three TODO stubs)
contact.qmd        Email + profiles
legal.qmd          Company details + privacy notice
styles.scss        The whole theme (brown palette, Zilla Slab display, eyebrows, rows)
assets/work-listing.ejs   Custom listing template (emits <h2> titles for heading order)
assets/fonts/      Self-hosted subsets: IBM Plex Sans (400/500/600) + Zilla Slab (500/700)
assets/img/        Portrait (senne.jpg) + favicon.svg
assets/cv.pdf      Downloadable CV (see below)
bib/               Machine-readable record of the publications, mirroring the LaTeX CV
CNAME              Custom domain; copied into _site on every render
```

Navbar: **Home · CV · Publications · Consultancy · Work**, with **Contact** at the
right. Footer carries the company line, the external profile links, and **Legal &
privacy**.

## Design in one paragraph

The thesis palette drives everything: dark brown body text (`#54392D`) and a rust
accent (`#864A33`) on a cool off-white ground — a deliberate counter to the warm-cream
Didone look of peers in the same field. Type pairs **IBM Plex Sans** for body/UI with a
**Zilla Slab** display face for the name, page titles, and section headings. Every
non-Home page opens with a letterspaced small-caps **eyebrow** label, the one device
that ties the pages together. Publications and the CV share a **three-column row**
(meta · content · action) with hairline rules; there is no TOC — a single centred
measure throughout. Fonts are self-hosted and subset; colours meet WCAG AA; motion
respects `prefers-reduced-motion`; images carry alt text and intrinsic dimensions.

## Add a publication

`publications.qmd` is hand-authored so it reads cleanly. Each entry is a three-column
row — year, then title + author list (your name bold) + venue, then a single link.
Copy an existing block under the right `##` status heading and edit it:

```markdown
:::: {.pub}
::: {.pub__meta}
2026
:::
::: {.pub__body}
[Full title here.]{.pub__title}

Author, A., [Vleminckx, S.]{.me}, Other, B. *Venue*.
:::
::: {.pub__action}
[DOI](https://doi.org/…)
:::
::::
```

Wrap your own name in `[…]{.me}` to bold it. Omit the whole `.pub__action` block if
there is no link. The `bib/*.bib` files are a machine-readable mirror of the same list
(and of the LaTeX CV) — update them alongside if you want them to stay current; they are
**not** rendered into the site.

## Add a case study

Copy the template and fill it in:

```bash
cp work/_template.qmd work/your-slug.qmd
```

Set the `title`, `subtitle` (one-line problem summary) and `categories`, then write the
five sections (Context / The question / Approach / Outcome / What I'd do differently),
150–250 words. It appears on `/work` automatically. The three `case-study-*.qmd` files
are placeholders — **do not publish them until they describe real work.**

## The CV PDF

`assets/cv.pdf` is generated from the separate LaTeX CV project (`~/Downloads/autoCV`):

```bash
cd ~/Downloads/autoCV && latexmk -pdf cv.tex
cp cv.pdf ~/senne-site/assets/cv.pdf
```

The on-site CV (`cv.qmd`) is a skimmable index; the PDF is the complete record. Keep them
in step when your CV changes.

## Deploy

See **[DEPLOY.md](DEPLOY.md)** for GitHub Pages, the custom domain, and the exact DNS
records.

## Placeholders still to fill (`TODO(senne)`)

```bash
grep -rn "TODO(senne)" . --include=*.qmd --include=*.bib --include=*.yml
```

Current list: the three `work/case-study-*.qmd` stubs — real case studies to be written
(or the stubs removed). Everything else the earlier draft listed (headshot, staff-profile
URL, day-rate, legal identifiers) is now filled in.
