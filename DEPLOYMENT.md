# Deployment & Domain Migration Guide

This guide walks you through moving the Braunstein Construction website **off Squarespace** and onto **GitHub Pages**, while keeping the same domain (braunsteinconstruction.com).

## Cost comparison

| Item | Squarespace (current) | GitHub Pages (proposed) |
|---|---|---|
| Hosting | ~$192–$432 / year (Personal–Business plan) | **$0 / year** |
| Custom domain support | Included | Included (free) |
| HTTPS / SSL | Included | Included (auto via Let's Encrypt) |
| Domain renewal | ~$20 / year (Squarespace registrar) or external | ~$12–20 / year (Cloudflare Registrar is cheapest, at-cost) |
| **Total per year** | **~$210–$450** | **~$12–20** |

**Annual savings: ~$200 to $430.**

## Files in this folder (what gets deployed)

- `index.html` — the actual site
- `robots.txt` — tells search engines and AI crawlers they're welcome
- `sitemap.xml` — index of pages for search engines
- `llms.txt` — plain-text summary written for AI agents and LLMs
- `CNAME` — tells GitHub Pages to serve the site at `braunsteinconstruction.com`
- `.nojekyll` — disables GitHub's default Jekyll processor (we don't need it)
- `DEPLOYMENT.md` — this guide
- `README.md` — short project overview

---

## Step 1 — Get the files into GitHub

You'll need a free GitHub account (https://github.com/signup).

1. **Create a new repository.** On GitHub click `+` → `New repository`. Name it something like `braunstein-construction-website`. Make it **Public** (free Pages requires public for org accounts, but personal accounts get private Pages too). Don't initialize with a README — we already have files.

2. **Upload the files.** On the empty repo page, click `uploading an existing file`. Drag the entire contents of this folder (`index.html`, `robots.txt`, `sitemap.xml`, `llms.txt`, `CNAME`, `.nojekyll`, `DEPLOYMENT.md`, `README.md`) into the upload area. Commit.

   *Alternative — if you use Git locally:*
   ```bash
   git init
   git add .
   git commit -m "Initial site"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/braunstein-construction-website.git
   git push -u origin main
   ```

## Step 2 — Turn on GitHub Pages

1. In the repo, go to `Settings` → `Pages` (left sidebar).
2. Under **Source**, choose **Deploy from a branch**.
3. Branch: `main`, folder: `/ (root)`. Save.
4. Wait ~1 minute. GitHub will give you a URL like `https://YOUR_USERNAME.github.io/braunstein-construction-website/`. Open it — your site should be live there.

## Step 3 — Point braunsteinconstruction.com at GitHub Pages

This is the DNS step. The exact screens depend on **where the domain is currently registered**.

### A) If the domain is registered with Squarespace

1. Log in to Squarespace → `Settings` → `Domains` → click `braunsteinconstruction.com`.
2. Open **DNS Settings** (or "Advanced DNS").
3. Delete the existing A records and CNAME records that point to Squarespace.
4. Add the four GitHub Pages A records for the apex domain:
   ```
   A    @    185.199.108.153
   A    @    185.199.109.153
   A    @    185.199.110.153
   A    @    185.199.111.153
   ```
5. Add the CNAME for `www`:
   ```
   CNAME    www    YOUR_USERNAME.github.io
   ```
6. Save. DNS propagation takes 5 minutes to a few hours.

### B) Recommended: move the domain to Cloudflare Registrar (cheapest)

Cloudflare sells domains at wholesale cost — `.com` is around USD $10.44/year. To move:

1. Sign up at https://dash.cloudflare.com (free).
2. Add your site → Cloudflare scans your existing DNS records.
3. Cloudflare gives you two nameservers. Log in to Squarespace and change the nameservers on the domain to the ones Cloudflare provides.
4. Once Cloudflare shows the domain as "Active", go to `Registrar` → `Transfer` to move the domain from Squarespace to Cloudflare. (You'll first need to **unlock** the domain in Squarespace and get an auth code.)
5. In Cloudflare DNS, set the same A records and CNAME shown above in section A.

### C) If the domain is at GoDaddy / Namecheap / etc.

Same A and CNAME records as section A — just in that registrar's DNS panel instead of Squarespace's.

## Step 4 — Tell GitHub Pages to use the custom domain

1. Back in GitHub `Settings` → `Pages`.
2. Under **Custom domain**, enter `braunsteinconstruction.com`. Save.
3. (The `CNAME` file in the repo already handles this, but the UI step makes GitHub provision the SSL certificate.)
4. Check **Enforce HTTPS** once it becomes available (usually within an hour).

## Step 5 — Verify

- Open `https://braunsteinconstruction.com` — the new site should load.
- Open `https://braunsteinconstruction.com/robots.txt` — should return the robots file.
- Open `https://braunsteinconstruction.com/llms.txt` — should return the LLM summary.
- Open `https://braunsteinconstruction.com/sitemap.xml` — should return the sitemap.
- Run https://search.google.com/test/rich-results on the homepage to confirm the JSON-LD structured data (GeneralContractor + FAQPage) is being detected.
- Submit the sitemap to Google: https://search.google.com/search-console — add the property, verify ownership via DNS TXT record, then submit `sitemap.xml`.

## Step 6 — Cancel Squarespace

**Do this last, after the new site has been live and working for a week.**

1. Log in to Squarespace → `Settings` → `Billing & Account`.
2. Cancel the **website subscription** (the hosting plan).
3. If you moved the domain to Cloudflare in step 3B, also cancel any domain renewal on Squarespace. If you kept the domain at Squarespace, leave the domain subscription active — just cancel the website plan.

## Updating the site later

Two options:

1. **In the browser:** edit any file in the GitHub repo through github.com, commit. Site updates in 1–2 minutes.
2. **Locally:** clone the repo, edit, `git push`. Same result.

Common updates:
- Add a phone number → edit the Contact section in `index.html` and the JSON-LD `telephone` field.
- Add project photos → drop images into the folder, reference them in the hero / a new gallery section.
- New service area → update both `index.html` (areaServed in JSON-LD) and `llms.txt`.
- Add a blog post → create `/blog/post-name.html`, add it to `sitemap.xml`.
