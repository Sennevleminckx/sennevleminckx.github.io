# Deployment

The site is a Quarto website deployed to **GitHub Pages** from a `gh-pages`
branch, served at the custom domain **sennevleminckx.be**.

Repo: `sennevleminckx.github.io` (a GitHub *user site*, so it serves at the
domain root — no `/repo/` path segment).

---

## 1. Build clean

```bash
quarto render
```

Must finish with **no warnings**. The output goes to `_site/` (git-ignored).

## 2. First deploy

```bash
quarto publish gh-pages
```

This renders, pushes the output to the `gh-pages` branch, and opens the site.
In the GitHub repo, set **Settings → Pages → Build and deployment → Source:
Deploy from a branch → `gh-pages` / root**.

Verify it loads at **https://sennevleminckx.github.io/** before touching DNS.

## 3. Custom domain — set it in the repo *before* pointing DNS

In **Settings → Pages → Custom domain**, enter `sennevleminckx.be` and save.

> ⚠️ Do this **before** the DNS records exist. Configuring the domain in the
> repo first claims it; pointing DNS at GitHub Pages before the repo owns the
> name leaves a window for **subdomain takeover**.

## 4. CNAME persistence (already wired — verify, don't assume)

`quarto render` wipes `_site/`, so `CNAME` must be regenerated into the output
each build. It is: `CNAME` lives in the project root and is listed under
`project.resources` in `_quarto.yml`, so Quarto copies it into `_site/CNAME`
on every render.

Verify after any render:

```bash
quarto render && cat _site/CNAME     # -> sennevleminckx.be
```

If this ever comes back empty, the custom domain silently detaches on the next
publish.

## 5. DNS records to enter at EasyHost

Point the apex and `www` at GitHub Pages. **Do not touch existing `MX` records**
(email) or you will break mail.

**Apex `sennevleminckx.be` — four A records:**

| Type | Name | Value |
|------|------|-------|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |

**Apex — four AAAA records (IPv6):**

| Type | Name | Value |
|------|------|-------|
| AAAA | @ | 2606:50c0:8000::153 |
| AAAA | @ | 2606:50c0:8001::153 |
| AAAA | @ | 2606:50c0:8002::153 |
| AAAA | @ | 2606:50c0:8003::153 |

**`www` subdomain — one CNAME:**

| Type | Name | Value |
|------|------|-------|
| CNAME | www | sennevleminckx.github.io. |

Propagation can take **up to 24 hours**. Only once it resolves will the
**Enforce HTTPS** checkbox become available in Settings → Pages — tick it then.

## 6. Routine updates

Edit content, then:

```bash
quarto publish gh-pages
```

`CNAME` and the custom domain survive automatically (step 4).

---

## Optional — render on push via GitHub Actions (proposed, not enabled)

This lets you edit `.qmd` files from anywhere (even the GitHub web editor)
without a local Quarto install. **Not created** — review and add
`.github/workflows/publish.yml` yourself if you want it:

```yaml
name: Publish site
on:
  push:
    branches: [main]
permissions:
  contents: write
jobs:
  build-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: quarto-dev/quarto-actions/setup@v2
      - uses: quarto-dev/quarto-actions/publish@v2
        with:
          target: gh-pages
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

Trade-off: it adds a CI dependency and a second way to publish. If you only
ever deploy from this machine, the manual `quarto publish gh-pages` is simpler.

---

## Analytics (optional, cookieless only)

If you want visit stats, use a **cookieless** service — **GoatCounter** or
**Plausible** — added via `include-in-header` in `_quarto.yml`. Do **not** add
Google Analytics: it would create a cookie-consent-banner obligation the site
otherwise avoids entirely. If you enable one, update the privacy notice in
`legal.qmd` to name it.
