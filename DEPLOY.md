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

Push to `main` and GitHub Actions renders and publishes automatically (see the
section below). So the normal flow is just:

```bash
git push origin main
```

Manual publish still works as a fallback (e.g. if Actions is down), and produces
the same result:

```bash
quarto publish gh-pages
```

`CNAME` and the custom domain survive automatically (step 4).

---

## Render on push via GitHub Actions (enabled)

`.github/workflows/publish.yml` runs on every push to `main`: it sets up Quarto,
renders, and publishes to `gh-pages` using the built-in `GITHUB_TOKEN`. You can
also trigger it by hand from the repo's **Actions** tab (`workflow_dispatch`).

This means you can edit `.qmd` files from anywhere (even the GitHub web editor)
without a local Quarto install. The site has no executable code cells, so the
workflow only needs Quarto — no Python/R kernel.

One-time repo settings to confirm: **Settings → Actions → General → Workflow
permissions** must allow **Read and write** (needed to push to `gh-pages`), and
**Settings → Pages → Source** stays **Deploy from a branch → `gh-pages` / root**.

Manual `quarto publish gh-pages` still works and is the fallback if Actions is
unavailable.

---

## Analytics (optional, cookieless only)

If you want visit stats, use a **cookieless** service — **GoatCounter** or
**Plausible** — added via `include-in-header` in `_quarto.yml`. Do **not** add
Google Analytics: it would create a cookie-consent-banner obligation the site
otherwise avoids entirely. If you enable one, update the privacy notice in
`legal.qmd` to name it.
