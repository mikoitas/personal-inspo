# Deploying Personal inspo (GitHub + Vercel)

You just need to (1) put this `site/` folder on GitHub as a **private** repo and (2) import it into Vercel. ~5 minutes. Two routes below — pick the web one if you don't want to touch a terminal.

> Note: the very first command below re-initializes git fresh on your Mac. (A repo was started in the build sandbox, but sandboxes can't finalize git cleanly, so we just start it clean on your machine — git works natively there.)

> Keep the repo **private**. Vercel deploys the built site publicly regardless, and a private repo keeps any deleted screenshots out of public git history (your privacy guardrail).

---

## Route A — Web UI (no terminal)

### 1. Create the GitHub repo
1. Go to https://github.com/new
2. Name it e.g. `personal-inspo`. Set it to **Private**. Do **not** add a README/.gitignore (the repo already has them).
3. Create repository. Leave the page open — you'll need the two commands it shows under "…push an existing repository".

### 2. Push this folder
Open Terminal, then:
```bash
cd "/Users/uxidbi/CLAUDE/Projects/Personal inspiration/site"
git remote add origin https://github.com/mikoitas/personal-inspo.git
git push -u origin main
```
(It'll ask you to sign in to GitHub the first time.)

### 3. Import into Vercel
1. Go to https://vercel.com/new
2. Sign in with GitHub, authorize Vercel to access the repo.
3. Import `personal-inspo`.
4. Settings:
   - **Framework Preset:** Other
   - **Root Directory:** `./` (the repo root *is* the site)
   - **Build & Output:** leave empty — it's a static site, no build step.
5. Click **Deploy**. In ~20s you'll get a live `*.vercel.app` URL.

Done. Every future `git push` auto-redeploys.

---

## Route B — CLI

```bash
cd "/Users/uxidbi/CLAUDE/Projects/Personal inspiration/site"

# GitHub (needs the GitHub CLI: https://cli.github.com)
gh repo create personal-inspo --private --source=. --remote=origin --push

# Vercel (needs the Vercel CLI: npm i -g vercel)
vercel            # follow prompts: scope, link, root = ./
vercel --prod     # promote to production
```

---

## After it's live

- **Custom domain (optional):** Vercel → Project → Settings → Domains.
- **Updating content:** the site reads `data/index.json`. The Mac sync agent (next build step) will optimize new screenshots into `data/images/`, append records to `data/index.json` (+ `index.js`), commit, and push — which auto-redeploys. Removing a screenshot removes its image + record. No HTML changes ever needed.
- **Indexing:** `robots.txt` + the `noindex` meta keep it out of search results while still reachable by link.

## To do before this is "real"
- Self-host the 11 category 3D icons (currently Thiings CDN) into `data/icons/` for resilience.
- Optionally vendor Tailwind + Lucide (currently CDN) so the site has zero runtime third-party dependencies.
