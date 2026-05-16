# Braunstein Construction Website

Source code for **braunsteinconstruction.com** — a single-page static site for Braunstein & Braunstein Construction Ltd., a Toronto-based family-owned builder specializing in garden suites, laneway suites, multiplex conversions, and multi-family construction across the GTA.

## Stack

- Plain HTML + CSS + a small amount of vanilla JS. No build step, no framework.
- Hosted on **GitHub Pages** (free).
- Custom domain via `CNAME` file.
- SEO: full meta tags, OpenGraph, Twitter cards, and JSON-LD structured data (`GeneralContractor`, `FAQPage`).
- AI-crawler friendly: `llms.txt` for LLM agents, explicit `robots.txt` allow rules for GPTBot / ClaudeBot / PerplexityBot / etc.

## Files

| File | Purpose |
|---|---|
| `index.html` | The entire site |
| `robots.txt` | Crawl directives (allows all reputable bots and AI crawlers) |
| `sitemap.xml` | Sitemap for search engines |
| `llms.txt` | Plain-text summary for AI agents |
| `CNAME` | Custom domain config for GitHub Pages |
| `.nojekyll` | Tells GitHub Pages to skip Jekyll processing |
| `DEPLOYMENT.md` | Full step-by-step migration + hosting guide |

## Editing

Open `index.html` in any editor. Save. Commit. GitHub Pages redeploys in ~1 minute.

## Deployment

See `DEPLOYMENT.md` for the full migration-from-Squarespace + custom-domain walkthrough.
