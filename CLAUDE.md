# inctasoft.github.io — Company Website

Static landing page for **inctasoft.com**, hosted on GitHub Pages.

**Live at:** `https://inctasoft.com` (GitHub Pages with custom domain)

## Architecture

Single-file static site — no build step, no framework, no dependencies.

```
inctasoft.github.io/
├── index.html    ← Landing page (HTML + inline CSS, Google Fonts)
├── CNAME         ← GitHub Pages custom domain config
└── CLAUDE.md     ← This file
```

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Hosting | GitHub Pages (auto-deploy on push to `main`) |
| Domain | `inctasoft.com` (AWS Route 53 registrar + DNS) |
| TLS | GitHub Pages auto-provisioned Let's Encrypt cert |
| Design | Dark theme, Inter font, responsive 2-column grid |

## Deployment

Push to `main` → GitHub Pages auto-builds (usually <1 min). No CI/CD config needed — `.github.io` repos auto-enable Pages.

## DNS Configuration

Domain `inctasoft.com` is managed in AWS Route 53 (root account):

| Type | Name | Value |
|------|------|-------|
| A | `inctasoft.com` | `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153` |
| CNAME | `www.inctasoft.com` | `inctasoft.github.io` |
| MX | `inctasoft.com` | Google Workspace mail servers (ASPMX.L.GOOGLE.COM, etc.) |
| CNAME | `jlzjdu..._domainkey.inctasoft.com` | Amazon SES DKIM |
| CNAME | `lyz66p..._domainkey.inctasoft.com` | Amazon SES DKIM |
| CNAME | `zpv6hc..._domainkey.inctasoft.com` | Amazon SES DKIM |

Hosted zone ID: `Z0573802XYE3J3UOKLJ1`. These are GitHub Pages' official IPs. The `CNAME` file in the repo tells GitHub to serve this site for `inctasoft.com`.

## Content

Landing page for Inctasoft — "a small, family-run team that lives and breathes AI."

Four service cards (2x2 grid):
1. **Workflow Automation** — AI agents that integrate into existing stacks
2. **Strategic Upskilling** — hands-on workshops for engineering teams
3. **Quality Assurance** — eval suites, living documentation, regression testing
4. **Agentic Engineering** — AI agents as true engineering collaborators

Contact: `office@inctasoft.com`

## Blog (`/blog/`)

Hugo-powered blog at `inctasoft.com/blog/`. Custom `inctasoft` theme matches the landing page design (dark bg, Inter font, blue accent, glassmorphism cards).

```
hugo.toml                          ← Hugo config (baseURL = /blog/)
content/posts/*.md                 ← Blog posts (Markdown + frontmatter)
themes/inctasoft/                  ← Custom theme
├── layouts/_default/baseof.html   ← Base template (starfield bg, nav, footer)
├── layouts/_default/list.html     ← Blog index (glass post cards)
├── layouts/_default/single.html   ← Single post (glass article card)
└── layouts/partials/              ← head.html, header.html, footer.html
```

**How it works:** GitHub Actions builds Hugo → copies `public/` into `_site/blog/`, puts `index.html` + `CNAME` at root → deploys via `actions/deploy-pages`. Landing page stays hand-crafted; Hugo only owns `/blog/`.

**Adding a post:** Create `content/posts/my-post.md` with frontmatter (`title`, `date`, `author`, `description`). Use `<!--more-->` to control the summary on the list page.

**Local preview:** `hugo server` → `localhost:1313/blog/`

## Conventions

- Landing page: single `index.html` with inline `<style>` — no external CSS files
- Blog: Hugo with custom theme, inline CSS in `head.html` partial
- Google Fonts loaded via `<link>` (Inter)
- Landing page JS: star generator + card hover interactivity
- Blog JS: star generator only
- Keep it minimal — this is a company presence page + blog, not a web app

## Gotchas

- **TLS is GitHub Pages' responsibility, not Caddy's.** `inctasoft.com` A records point to GitHub's IPs (185.199.x.x), not our server. Let's Encrypt certificates are provisioned by GitHub Pages automatically. Caddy on `akrsmv-Vortex-Ultimate` cannot obtain a cert for this domain (HTTP-01 challenge would hit GitHub, not our server). If GitHub Pages cert provisioning fails, check the repo Settings → Pages → custom domain + "Enforce HTTPS".
- **No A/CNAME conflict.** The apex (`inctasoft.com`) uses A records, `www` uses CNAME — this is correct. Having both A and CNAME for the *same* name would be a conflict, but they're on different names.
- **GitHub Pages HTTPS provisioning:** After changing DNS, the TLS certificate takes 5-15 minutes to provision. HTTPS enforcement can only be enabled after the cert exists.
- **CNAME file:** Must contain exactly the custom domain (`inctasoft.com`), no trailing slash. GitHub reads this to configure the custom domain.
- **AWS Route 53:** DNS is at Route 53, NOT Cloudflare (unlike `akrsmv.net`). Root account access required to modify records.
- **DKIM records:** 3x Amazon SES `_domainkey` CNAME records exist for email signing. These don't affect web hosting or TLS.
